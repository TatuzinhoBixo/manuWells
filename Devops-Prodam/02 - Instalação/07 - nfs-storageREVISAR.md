### Nfs-Storage
NFS Storage é um tipo de armazenamento em rede que permite que vários pods no Kubernetes acessem os mesmos arquivos de forma compartilhada, como se fosse um "disco" comum montado em todos eles.
A instalação é feita via helm, adicionando primeiramente o repositório
```bash

helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
helm repo update
helm install <nome-nfs-storage> nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
  --namespace kube-system \
  --set nfs.server=<ip-nfs> \
  --set nfs.path=<caminho-nfs-remoto> \
  --set storageClass.name=<nfs-class> \
  --set storageClass.defaultClass=true \
  --set storageClass.reclaimPolicy=Retain
```

Dessa forma esse nfs-storage vai ser o default, todos os pods vão buscar ele primeiro para fazer a conexão, caso exista a necessidade atual ou futura de outros nfs-storages, a nova instalação deve ser feita com a seguinte modificação:

```bash
helm install <nome-nfs-storage> nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
  --namespace kube-system \
  --set nfs.server=<ip>\
  --set nfs.path=<caminho-nfs-remoto> \
  --set storageClass.name=<nfs-class> \
  --set storageClass.defaultClass=false \
  --set storageClass.reclaimPolicy=Retain
```

Criando o pvc para disponibilzar o espaço no nfs, rode esse manifesto nele será gerado um espaço de 5Gb

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: <nome-pvc>
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 5Gi
  volumeName: <nome-pv>
  storageClassName: "" # vazio porque é um PV estático
```
