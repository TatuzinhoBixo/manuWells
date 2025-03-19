### Variáveis usadas
&lt;ip&gt; -  IP do de rede, ex: 192.168.1.10
&lt;login&gt; - Nome se usuário, ex: prodam
&lt;senha&gt; - Senha, ex: Senhas@@
&lt;rede-broadcast&gt; - Rede broadcast, ex: 192.168.0.0
&lt;mask&gt; - Máscara de rede, ex: 24
&lt;dns&gt; - Ip do dns, ex: 10.20.1.126
&lt;gw&gt; - Ip do gateway, ex: 10.20.1.1
&lt;ip-cluster&gt; - Ip do cluster, é o mesmo do proxy
&lt;dns-cluster&gt; - dns do cluster apontado para o proxy
&lt;versao&gt; - Versão do cluster informada pelo comando 
```bash
curl -s https://api.github.com/repos/rancher/rke2/releases | grep 'tag_name' | cut -d\" -f4 | sort -V | grep -v 'rc'
```
&lt;token&gt; - chaves com digítos aleatórios