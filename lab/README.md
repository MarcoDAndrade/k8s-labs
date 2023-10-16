# Usando este lab

Copiei os arquivos relevantes do repositorio, e customizei para criação de uma ambiente limpo, que me permita reproduzir todos os labs:

- settings.yaml - Arquivo com a parametrização do ambiente;
- Vagrantfile - Arquivo que define a configuração do ambiente;
- scripts/init.sh - Inicia o ambiente já atualizado

Para subir o ambiente, basicamente, entre na pasta lab e execute os comandos abaixo:

- Crie o host Master

    ```shell
    cd lab
    vagrant up master
    ```

- Acesse o host master

    ```shell
    vagrant ssh master
    ```

- Agora é com você!

***Dicas***:

  1.  A pasta /vagrant é o seu diretorio local, então pode brincar com scripts!
  2. As minhas anotações estão na pasta day-1, seguindo o material referenciado na proxima sessão.
  3. O principal da abordagem é que você pode *suspender* um ambiente e *criar um novo*, só copiar os arquivos do Vagrant para uma nova pasta!

