### Cluster rke2
##### Antes de instalar:
Consultar a versão compatível do kubernets compatível com o rancher, [Versões kubernets compátiveis com o racher.](https://www.suse.com/suse-rancher/support-matrix/all-supported-versions) 
Após consultar a versão compatível rodar esse comando para que ele der o nome da versão completa, isso será necessário para instalar o cluster. 
```bash
curl -s https://api.github.com/repos/rancher/rke2/releases | grep 'tag_name' | cut -d\" -f4 | sort -V | grep -v 'rc'

```
##### Instalação
Criar a pasta
```bash
mkdir -p /etc/rancher/rke2
```
Criar o arquivo
```bash
touch /etc/rancher/rke2/config.yaml
```
Configuração do arquivo conf
```yaml
write-kubeconfig-mode: "0600"
cluster-init: true
cni: calico
disable:
  - rke2-ingress-nginx
  - rke2-canal
tls-san:
  - <ip-cluster>
  - <dns-cluster>
  ```
> **IMPORTANTE**:
> o proxy já deve ta configurado e o seu dns, salvo se for usar apenas um controlplane, assim o ip do cluster e o dns deve ser o mesmo da vm.

Baixar a instalação
```bash
curl -sfL https://get.rke2.io | INSTALL_RKE2_VERSION="<versao>" INSTALL_RKE2_TYPE="server" sh -
```
Iniciar o rke2-server
```bash
systemctl start rke2-server
systemctl enable rke2-server 
```
Para configurar o manifesto do cluster para rodar os comandos kubectl 
```bash
mkdir -p ~/.kube; sudo cp /etc/rancher/rke2/rke2.yaml ~/.kube/config; sudo chown $(id -u):$(id -g) ~/.kube/config
```
Teste do cluster
```bash
kubectl get no
```
Para que o pods do cluster consigam resolver nomes externos, é necessário fazer a configuração do rk2-coredns
```bash
kubectl edit configmap rke2-coredns-rke2-coredns -n kube-system
```
Adicionar
Caso o nome do configmap não seja rke2-coredns-rke2-coredns, buscar com o comando 
```bash
kubectl get configmap -n kube-system
```

##### Controlplanes adicionais

 Para o cadastro de outros servidores é preciso buscar o token na vm do primeiro controlplane.
 ```bash
 cat /var/lib/rancher/rke2/server/node-token
```
Criar a pasta
```bash
mkdir -p /etc/rancher/rke2
```
Criar o arquivo
```bash
touch /etc/rancher/rke2/config
```
Adicionar o conteúdo
```yaml
server: https://<ip>:9345 #IP DO HAPROXY
token: <token>
write-kubeconfig-mode: "0600"
cni: calico
disable:
  - rke2-ingress-nginx
  - rke2-canal
tls-san:
  - <ip-cluster>
  - <dns-cluster>
log-file: "/var/log/rke2/rke2.log"
log-level: "info"
  ```

Executar o comando
```bash
curl -sfL https://get.rke2.io | INSTALL_RKE2_VERSION="<versao>" INSTALL_RKE2_TYPE="server" sh -
```
> Antes de executar o inicio do serviço, inserir o ip do novo controlplane no proxy, para que ambos se comuniquem, caso isso não seja feito o controlplane não vai ser reconhecido no cluster

Para iniciar o serviço se usa o mesmo comando usado no controlplane1
```bash
systemctl start rke2-server
systemctl enable rke2-server
```
> Para saber se está tudo bem, pode-se rodar esse comando abrindo em outro terminal
> ```bash
> journalctl -f -eu rke2-server
> ``` 

### Instalação dos nós works
Criar a pasta
```bash
mkdir -p /etc/rancher/rke2
```
Criar o arquivo
```bash
touch /etc/rancher/rke2/config.yaml
```
Incluir no arquivo de configuração
```yaml
write-kubeconfig-mode: "0600"
cni: calico # Rede que é usada
server: https://<ip> # IP do cluster
token: <token>
```                                                   
logging:                                                                    

Verificar a versão do cluster, caso não tenha conhecimento, no terminal de algma vm que tenha o kubectl

```bash
kubectl get no
```
Após isso na vm onde que vai ser o work, executar o comando de instalação
```bash
curl -sfL https://get.rke2.io | INSTALL_RKE2_VERSION="<versao>" INSTALL_RKE2_TYPE="agent" sh -
```
Inciar o serviço
```bash
systemctl start rke2-agent
systemctl enable rke2-agent
```
> Para saber se está tudo bem, pode-se rodar esse comando abrindo em outro terminal
> ```bash
> journalctl -f -eu rke2-agent
> ``` 
