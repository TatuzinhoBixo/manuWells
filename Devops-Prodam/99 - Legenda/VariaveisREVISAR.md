### Variáveis usadas
&lt;ip&gt; -  IP do de rede, ex: 192.168.1.10
&lt;login&gt; - Nome se usuário, ex: prodam
&lt;senha&gt; - Senha, ex: Senhas@@
&lt;rede-broadcast&gt; - Rede broadcast, ex: 192.168.0.0
&lt;mask&gt; - Máscara de rede, ex: 24
&lt;dns&gt; - Ip do dns, ex: 10.20.1.126
&lt;gw&gt; - Ip do gateway, ex: 10.20.1.1
&lt;ip-cluster&gt; - Ip do cluster, é o mesmo do proxy
&lt;dns-cluster&gt; - Dns do cluster apontado para o proxy
&lt;cluster-kube&gt; - Versão do kubernets informada via comando
```bash
curl -s https://api.github.com/repos/rancher/rke2/releases | grep 'tag_name' | cut -d\" -f4 | sort -V | grep -v 'rc'
```
&lt;token&gt; - Chaves com digítos aleatórios algumas vezes são sejas geradas automaticamente durante a instalação
&lt;namespace&gt; - Segregação dos serviços em um único grupo, essa separação é feita geralmente por projeto ou por cliente.
&lt;ingress-class&gt; - Nome que o ingress controller cria quando é instalado, é usado pelo ingress da aplicações para que se tenha comunicação entre o ingress e o ingress-controlller, pode ser compartilhado por diversos ingress que terão o mesmo ip.
&lt;numero-replicas&gt; - Quantidade de pods da aplicações, recomendável existirm mais de um para que se tenha redundância.
&lt;nome-ingress&gt; - Nome do serviço do ingress
&lt;url&gt; - Url completa do serviço
&lt;senha&gt; - Senha escolhida na hora da instalação, recomenda-se que seja complexa para que o serviço a aceite
&lt;tls-secret&gt; - Nome do certificado ssl criado, é recomendavel colocar o nome do dominio para ter fácil identificação, pode ser usada por diversos ingress no mesmo namespace.
&lt;git-secret&gt; - Nome da chave do secret do git, deve ser o hash usado para se fazer o login no git, senha comum não vai servir
&lt;username&gt; - nome do usuário para login
&lt;password&gt; - Senha né? 
nfs-class
domino
ip-nfs
caminho-nfs-remoto
nome-nfs-storage
