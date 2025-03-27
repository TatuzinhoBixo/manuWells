### Instalação comando Metallb
Comando **mettalb** para que os serviços de obtenham os ips para o seu serviço é usando o Metallb, um compomente instalado no kubernets. Nele é possível configurar um ranger de ips, semelhante a um dhcp, que vão ser usados.

Para instalar é necessário ter o kubectl configurado e acessando o cluster.
[Intalação do comando Metallb](https://metallb.universe.tf/installation/)
 
Comando para a instalação:
```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.8/config/manifests/metallb-native.yaml
```
Se cria o manifesto, "metallb.yaml" para aplicar:
```yaml
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: first-pool
  namespace: metallb-system
spec:
  addresses:
    - <ip>-<ip>
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: metallbex
  namespace: metallb-system
spec:
  ipAddressPools:
    - first-pool
```
Aplicando o manifesto
```bash
kubectl apply -f metallb.yaml
```

Caso precise incluir uma nova faixa de ips, é so acrescentar a nova faixa abaixo da configuração de ips já existente e depois reaplicar o manifesto

Comando para mostrar o pools já criados assim como seus ips reservados
```bash
kubectl get ipaddresspool -A
```
