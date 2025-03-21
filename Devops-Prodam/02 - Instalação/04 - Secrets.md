### Secrets
Componente do kubernets que pode ser senha de algum serviço, por exemplo do git onde vai se baixar as imagens ou pode ser também o certificado ssl dos sites. Ele é criado por um comando kubernets de acordo com o seu objetivo.

Para se criar uma chave de certificado ssl

```bash 
kubectl create secret tls <tls-secret> \
--cert=/caminho-certificaodo/certificado.pem \
--key=/caminho-chave/private.key \
-n <namespace>
```

Para se criar o secret relacioado a senha do git
```bash
kubectl create secret docker-registry <git-secret> \
  --docker-server=<url> \
  --docker-username=<username> \
  --docker-password=<password> 
```