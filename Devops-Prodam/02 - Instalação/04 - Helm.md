### Instalação comando helm
Comando **helm** é usado para a instalação de aplicações no cluster, alguns exemplos são: rancher, minio, nfs-storage, ingress-controller e etc. 
Geralmente esse comando é instalado no primeiro controlplane, mas pode ficar em qualquer vm que tenha acesso para a adminstração do cluster.

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

[Intalação do comando helm](https://helm.sh/docs/intro/install/)