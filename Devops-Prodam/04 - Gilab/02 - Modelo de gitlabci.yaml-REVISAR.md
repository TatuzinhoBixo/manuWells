update_deployment:
  stage: push
  tags:
    - docker
  script:
    - |
      echo "✅ Clonando o repositório gitops-infra"
      git clone https://oauth2:${CI_JOB_TOKEN}@git.prodam.am.gov.br/geinfs/dsred/gitops-talao-retaguarda-web-mg.git
      cd gitops-talao-retaguarda-web-mg/talao-web
 
      echo "🔍 Antes de editar deployment.yaml:"
      cat deployment.yaml | grep "image:"
 
      echo "🚀 IMAGE_TAG definido como: $IMAGE_TAG"
 
      echo "🛠 Atualizando a tag da imagem no deployment.yaml"
      sed -i "s|image: registry3.prodam.am.gov.br/hom/talao-retaguarda-web-mg.*|image: ${IMAGE_TAG}|" deployment.yaml
 
      echo "🔍 Depois de editar deployment.yaml:"
      cat deployment.yaml | grep "image:"
 
      # Verifica se houve mudanças no arquivo antes de tentar o commit
      if git diff --exit-code deployment.yaml; then
        echo "🚨 Nenhuma alteração detectada. Pulando commit."
        exit 0
      fi
 
      git config --global user.email "$CI_EMAIL"
      git config --global user.name "GitLab CI"
      git add deployment.yaml
      git commit -m "✅ Atualizando a tag da imagem para $CI_COMMIT_SHORT_SHA"
      git push origin main || (echo "❌ Erro no push, tentando novamente..." && sleep 5 && git push origin main)

