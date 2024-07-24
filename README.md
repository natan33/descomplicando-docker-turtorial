# DESCOMPLICANDO DOCKER

### O que é um container ? 
Container é, em português claro, o agrupamento de uma aplicação junto com suas dependências, que compartilham o kernel do sistema operacional do host, ou seja, da máquina (virtual ou física) onde está rodando.

Containers são bem similares às máquinas virtuais, porém mais leves e mais integrados ao sistema operacional da máquina host, uma vez que, como já dissemos, compartilha o seu kernel, o que proporciona melhor desempenho por conta do gerenciamento único dos recursos.


    
Um container é uma forma de isolar recursos para uma determinado fim:
    
* Dividir os recursos;
* isolar os recursos para não impactar outros processos; 



## Comando Docker 

depois de testar se o docker instar inataldo na maquina execute, para testar um container simples:

    > docker container run hello-world


Para listas os container instalados:

    > docker container ls
    // Para mostras todos os container
    > docker container ls -a // -> mostro os que estão e os que não estão em execução


## Criando e gerenciando o primeiro container

Vamos instalar o container do Ubutun:

    // comando para instalar, e se caso sair do container rode o mesmo comando e entrarar no container
    > docker container run -it ubuntu

Para saber a versão do ubutun instalada:

    > cat /etc/issue

* Para sair do container e para a execução:
        
    > crtl + d

* Para saur do container e deixar o container em execução:

    > crtl + p + q

* Para entrar no container sem precisar criar um novo:
    // SÓ SERVE SE O CONTAINER TIVER UM BASH, SE CASO NÃO TIVER UM BASH VAI BATER NO PROCESSO DO ENGINEX 
    > docker container attach "id_container"

* Para criar um container com nome:

    > docker container run  --name toskao -it ubuntu

* Para poder iniciar um container sem precisa entrar nele 

    > docker container start "nome_container" ou "id_container"

Para poder para o container:

     // para pausar o container use o  "pause" inves de "stop"
    > docker container stop "nome_container" ou "id_container"


    // Para tirar do pause use o "unpause"

Para poder remover um container usamos o `rm`:

    > docker container rm "nome_container"

   


## Vizualizando métricas e a utilização de recursos pelo containers

Para saber estaticas dos containers que estão em operação:

    > docker container stats
    // com o -a no final mostra todos os que estão e que não estão em execução

    // Para que não seje preciso mostra na tela inteira mais só mostre um print

    > docker container stats --no-stream

Para ver tudo o que está rodando dentro do container usamos o `top`:

    > docker container stop "nome_container"

Para apagar todos os container de vez:

    > docker container prune

Para ver os logs do constainer:

    > docker container logs 
    
* Comandos do docker logs para filtrar e visualizar logs de contêineres:

    ``--details:`` Mostra detalhes extras nos logs

    ``-f, --follow:`` Exibe logs em tempo real

    ``--since string:`` Exibe logs desde um determinado momento

    ``-n, --tail string:`` Define o número de linhas do final do log para exibir

    ``-t, --timestamps:`` Exibe marcas de tempo para cada linha do log
    
    ``--until string:`` Exibe logs até um determinado momento


## Visualizando e especionando Imagens e containers

Para visualizar as imagens que estão disponiveis na minha maquina:

    > docker image ls 
    // Ou do jeito antigo:
    > docker images 

Para remover uma imagem do docker:

    > docker image rm "ID_IMAGE"

`OBS:para remover uma imagem do docker é preciso que remova o container que está utlizando a imagem`

Para força a remoção de um container que está rodando utilizamos o -f:

        > docker container rm -f "nome_container"


Para inspecionar um container e saber as infomrções dele por um `json`:

    // Essa seria a forma antiga:
    > docker inspect "nome_container"

    // Para inpecionar um container diretamente:
    > docker container inspect "nome_container"


Para inspecionar uma imagem pelo inspect :

    > docker image inspect "id_image"


## Criando um container Dettached e o Docker exec

Vamos usar um exemplo com nginx como suar o Dettached :

    > docker container run -d --name my-nginx nginx
    // Depois de executar esse comando será buscado a imagem do nginx no docker hub

* Depois vamos ver se o a imagem do nginx está na maquina:

    > docker images

* Vamos ver se o container do nginx está rodando:

    > docker container ls 

    * Vamos ver como esta o processo do container pelo attach:

            > docker attach "id_container"
            // se passar os 4 primeiras caracteres do id_container tambem funcionarar:

        para sair use `CRTL + C` e ira para o container:


Vamos ver como executar alguma coisa dentro de um container:

* Para podemaos pergar o localhost do container:
    
    > docker container exec -it my-nginx curl localhost
    // Ira trazer o codigo html do localhost

    * Para um teste melhor vamos pegar o html do site do google

        > docker container exec -it my-nginx curl https://www.google.com/

    * Também podemos pedir para executar um bash dentro do container:

        > docker container exec -it my-nginx bash 

    * Vamos brincar um pouco dentro do container:

        para fazer um curl dentro do container

             > curl localhost
            // retornarar um html 

        para fazer um echo dentro do html do nginx:

             > root@ec3d8e8f831e:/# cd /usr/share/nginx/html/

             > echo BLBALBALBL > index.html

Para que possamos roda o container do nginx no nosso navegador use o seguinte:

     > docker container run -d -p 8080:80 --name my-nginx nginx

    // passe o docker ps para saber se está rodando 
    > docker ps -a

* Vamos testar se funcionou:
    1. Vamos pegar o ip da maquina:

         > ip a

         //  ip passado: 172.19.238.175
    
    2. Vamos testar no nosso navegador com o ip passado:

        172.19.238.175/:8080 


Vamos aprender a baixar a imagem sem precisar baixar o container 

     // Baixar a imagem do CentOS na versão 7
     > docker pull centos:7 

Para criar um container sem precisar executar ele:

    > docker container create --name opa nginx