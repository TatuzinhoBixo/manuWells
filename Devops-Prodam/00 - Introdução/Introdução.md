# Introdução
Neste documeto está descrito os passos para a instalação de um ambiente de alta escalabilidade, usando as ferrametas do Rancher, Kubernets, Argocd, Haproxy, Minio. Nessa versão estará apenas as partes de configuração com exemplos de manifestos e arquivos .conf que estão em produção na Prodam. O objetivo é entender rooadmap desde instalação do cluster até os passos para o deploy. Não será mostrado aqui a configuração de instalação e de operação do gitlab e registry pois se entende que eles já estão configurados.
Na primeira etapa temos os requisitos de instalação de cada componente após isso é mostrado a sua instalação. Todas as vms tem o O.S Ubuntu 24
Nem todos os comandos tem o campo da variável em aberto, digo, por exemplo, vai ser descrito o nome da aplicação e você tem que fazer os ajustes.


