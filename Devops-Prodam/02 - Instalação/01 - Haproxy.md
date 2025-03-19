### Instalação do Haproxy

Primeiramente é necessário a criação do registro dns do cluster, o ip de destino deve ser o mesmo do haproxy. Quanto aos nós, eles devem ser os ips dos controplanes.

> Ativar somente um nó de controlplane para subir os nós do cluster. Após a finalização do cluster pode ser ativado os outros nós. Isso é necessário pois o cluster vai querer se comunicar com nós ainda não existentes.

##### Comandos

```bash
sudo apt install haproxy -y
sudo cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.bak
sudo vi /etc/haproxy/haproxy.cfg
```

##### Arquivos de configuração
Configurar o Haproxy seguindo esse modelo:
 ```bash
 x.x.x.x:8404/stats (usuário e senha configurados no haproxy)
userlist stats-auth
    group admin   users admin
    user admin    insecure-password <senha>
    group readonly    users <user>
    user prodam     insecure-password <senha>

frontend stats
    bind *:8404
    stats enable
    stats uri /stats
    stats refresh 10s
    acl AUTH http_auth(stats-auth)
    acl AUTH_ADMIN http_auth_group(stats-auth) admin
    stats http-request auth unless AUTH
    stats admin if AUTH_ADMIN
    acl is_permit src 0.0.0.0/0 localhost <rede-broadcast>
    acl is_stats url_beg url_beg /stats
    http-request deny deny_status 403 if is_stats ! is_permit

frontend kubernetes_api
    bind *:6443
    mode tcp
    option tcplog
    default_backend k8s_control_planes

backend k8s_control_planes
    mode tcp
    balance roundrobin

option tcp-check
    server controlplane1 <ip>:6443 check fall 5 rise 2
    server controlplane2 <ip>:6443 check fall 5 rise 2
    server controlplane3 <ip>:6443 check fall 5 rise 2


frontend rke2_server
  bind *:9345
  mode tcp
  option tcplog
  default_backend rke2_control_planes
backend rke2_control_planes
  mode tcp 
  balance roundrobin
  option tcp-check
  server controlplane1 <ip>:9345 check fall 5 rise 2
  server controlplane2 <ip>:9345 check fall 5 rise 2
  server controlplane3 <ip>:9345 check fall 5 rise 2

```
Comando para fazer o teste de configuração:
> sudo haproxy -c -f /etc/haproxy/haproxy.cfg
##### Configuração opcional
Com o tempo pode surgir a necessidade de se criar mais clusters e como os serviços usam a mesma porta vai dar conflito de redirecionamento. A solucão encontrada é de se incluir um segundo ip no proxy e na configuração do alterar o * para o <ip> referente ao cluster, ficando dessa forma <ip>:6443 e <ip>:9345 e duplicando o balanceamento, um para cada ip de cluster.
Além disso incluir o segundo IP
```yaml
network:
    ethernets:
        ens3:
            addresses:
            - <ip>/16
            - <ip>/16
            nameservers:
                addresses:
                - <dns>
                search: []
            routes:
            -   to: default
                via: <gw>
    version: 2
```