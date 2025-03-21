### Instalação do Ingress-controller
Fazendo uma analogia, o ingress-controller é o componente que expõe os serviços que estão no cluster. Ele vai receber o IP, no nosso caso que é on premises, usamos o metallb, escolhemos um que esteja no range já configurado. [Intalação do comando Metallb](https://metallb.universe.tf/installation/)
Outro ponto é que nesse caso estamos segregando o ip e o namespace de atuaçãoo, caso não fizer isso, o ip é fornecido ao componente e caso ele for reiniciado pode ser atribuido outro ip, podendo deixar o serviço inoperante externamente.
A instalação é feita via comando helm,  [Intalação do comando helm](https://helm.sh/docs/intro/install/) e também se faz importante ter o comando kubectl para verificar o status da instalação [Intalação do comando kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

Instalação do repositório e do ingress-controller
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install <nome-ingress> ingress-nginx/ingress-nginx \
  --namespace <namespace> \
  --set controller.ingressClassResource.name=<ingress-class> \
  --set controller.ingressClassResource.controllerValue="k8s.io/ingress-nginx" \
  --set controller.service.type=LoadBalancer \
  --set controller.service.loadBalancerIP="<ip>" \
  --set controller.replicaCount=<numero-replicas> \
  --set controller.extraArgs.watch-namespace=<namespace>
```
Comandos para a verificação da instalação:
```bash
kubectl get pods -A | grep ingress
kubectl get svc -n <namespace
kubectl get ingressclass
```
Caso se faça necessário a troca do ip, pode ser trocado editando o svc do ingress com o comando:
```bash
kubectl edit svc ingress-nginx-controller -n ingress-nginx
```