### Instalação do rancher
Antes de fazer a instalação é necessário ter instalado o comando helm e também o comando kubectl. As instruções pode ser encontrada na sessão "03 - Helm" ou por esse link. Além dessa instalação, ja devem está inclusas no cluster o ingress-controller e o secret que nada mais é o secret do certificado ssl

[Intalação do comando helm](https://helm.sh/docs/intro/install/)
[Intalação do comando kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)
#LINK DO METALLBINSTALL

Criação do namespace para o rancher
kubectl create ns cattle-system

helm upgrade --install rancher rancher-stable/rancher \
  --namespace cattle-system \
  --set hostname=&lt;url&gt; \
  --set replicas=&lt;numero-replicas&gt; \
  --set bootstrapPassword=&lt;senha&gt; \
  --set ingress.tls.source=&lt;ssl-secret&gt; \
  --set ingress.ingressClassName=&lt;ingress-class&gt;