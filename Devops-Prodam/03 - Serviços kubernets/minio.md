### Minio 
MinIO é um serviço de armazenamento de objetos compatível com S3, que você pode rodar dentro do Kubernetes como uma alternativa leve ao Amazon S3.
Ele é usado para armazenar arquivos, backups, imagens, logs, etc., e funciona bem com aplicações que já usam S3.
No Kubernetes, você pode deployar o MinIO como um pod ou StatefulSet, com persistência (via PVC) e acesso via serviço interno ou Ingress.
Ideal pra quem quer um S3 privado dentro do cluster.

Antes, já deve existir um pv e pvc para receber os dados e também um ingress-controller com replicas para garantir alta disponiblidade.

A instalação é feita via helm, primeiramente se adiciona o repositório.

```bash
helm repo add minio https://charts.min.io/
helm repo update
```

Criar o arquivo minio-values.yaml onde se tem alguns parâmetros, esses que foram usados mas podem ser alterados de acordo com a necessidade

```yaml
mode: distributed
replicas: 4

persistence:
  enabled: true
  storageClass: <nfs-class>
  size: 100Gi

resources:
  requests:
    memory: 2Gi
    cpu: 500m
  limits:
    memory: 4Gi
    cpu: 1000m

service:
  type: ClusterIP

consoleService:
  type: ClusterIP

extraArgs:
  - "--console-address=:9001"

env:
  - name: MINIO_SERVER_URL
    value: "https://minio.<domino>"
  - name: MINIO_BROWSER_REDIRECT_URL
    value: "https://minio-console.<domino>"
```

kubectl create ns minio
helm install minio minio/minio -n minio -f minio-values.yaml

Após isso pode ser feito a configuração de alta disponiblidade. rodando os serguintes manifestos (são de exeplos e podem ser modificados)
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: minio-hpa
  namespace: minio
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: StatefulSet
    name: minio
  minReplicas: 4
  maxReplicas: 8
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
  behavior:
    scaleUp:
      policies:
        - type: Pods
          value: 4  # Sempre escala de 4 em 4
          periodSeconds: 60
      stabilizationWindowSeconds: 120
    scaleDown:
      policies:
        - type: Pods
          value: 4  # Sempre reduz de 4 em 4
          periodSeconds: 60
      stabilizationWindowSeconds: 120
```

Além disso se faz necessário criar o ingress com as portas 9000 e 9001 usadas para o minio em si e o seu console

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minio-ingress
  namespace: minio
  annotations:
    nginx.ingress.kubernetes.io/proxy-body-size: "0"
    nginx.ingress.kubernetes.io/proxy-read-timeout: "600"
    nginx.ingress.kubernetes.io/proxy-connect-timeout: "600"
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
spec:
  ingressClassName: minio-ingress
  tls:
    - hosts:
        - minio.<dominio>
        - minio-console.<dominio>
      secretName: <tls-secret>  # Certificado SSL já existente
  rules:
    # 🔹 API do MinIO (porta 9000)
    - host: minio.<dominio>
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: minio
                port:
                  number: 9000

    # 🔹 Console Web do MinIO (porta 9001)
    - host: minio-console.<dominio>
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: minio-console
                port:
                  number: 9001
```
