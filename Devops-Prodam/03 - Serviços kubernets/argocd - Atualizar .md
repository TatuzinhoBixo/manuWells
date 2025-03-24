### Deploy kubernets com Agocd
**Instalação**
#INCIO DA INSTALACAO

Esse é o gerenciador de deploy usado para que se faça o deploy de aplicações de kubernets, seu processe é:

1. Programador faz o deploy usando o gitlab enviado a  imagem para o registry
2. Após o envio o gitlabci faz o versionamento do repositório que possui os manifestos do kubernets no gilab
3. O Argocd possui a rotina de verificação de mudança de versão e logo faz o novo deploy já buscando a nova imagem da aplicação que foi enviada pelo desenvolvedor.

Para fazer a instalação do Argocd é necessário já ter o helm instalado, o ingress-controller já tem que existir assim como o secret referente ao certificado

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
```

#VERIFICAR A QUESTAO O INGRESS

Adicionar o seguinte manifesto do ingress, no caso o nome do arquivo foi o ingress-argocd.yaml:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: <nome-ingress>
  namespace: <namespace>
  annotations:
    nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"
spec:
  ingressClassName: <nome-ingress>
  tls:
  - hosts:
    - <url>
    secretName: <tls-secret>
  rules:
  - host: <url>
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: <service-name>
            port:
              number: 443
```

Se aplica a manifesto do ingress

```bash
kubectl apply -f ingress-argocd.yaml
```
> Comandos para verificar se tudo ocorreu bem
```bash
kubectl get pods -n argocd
kubectl get svc -n argocd
kubectl get ingress -n argocd
```
Comando para buscar a senha do user admin
```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```



