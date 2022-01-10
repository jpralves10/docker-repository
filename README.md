## Docker

**`docker info`** <br/>
Quando nós acabamos de subir o Docker Engine, nós utilizamos esse comando para verificarmos as informações do nosso Docker Host <br/>

**`docker version`** <br/>
Com o version nós conseguimos ver a versão do nosso Client e Server pelo OS/Arch. Quanto ao OS/Arch do server nós podemos trabalhar com Windows ou Linux, caso você esteja utilizando o Docker for Windows basta clicar em Swith to Windows Containers <br/>

**`docker images`** <br/>
Para listarmos as imagens que nós temos no nosso host. <br/>

**`docker search (parametro)`** <br/>
Para que possamos procurar uma imagem, nós podemos utilizar o comando a baixo com o parâmetro nome Ex.: ubuntu, dotnetcore, node … etc, assim ele irá buscar as imagens que são compatíveis com o nosso server que para esse exemplo estamos utilizando o Linux. <br/>

**`docker pull (parametro)`** <br/>
Quando encontrarmos a imagem que precisamos para a nossa aplicação, nós precisamos baixar ela para o nosso host, exemplo: `docker pull node`. Após baixar a imagem execute o comando `docker images` para que possamos listar ela no nosso host. <br/>

**`docker run hello-world`** <br/>
Para que nós possamos criar um contêiner, nós precisamos de uma imagem. Caso você não tenha essa imagem no seu host ainda ele irá até o repositório central e irá baixar ela para o seu host, em seguida ele irá criar o contêiner. Agora execute o comando `docker images` novamente no seu terminal e note que temos uma nova imagem. <br/>

**`docker ps` e `docker ps -a`** <br/>
Notem que ele está retornando uma lista vazia, isso acontece porque os contêineres nascem e morrem, quando nos executamos o comando docker run hello-world ele realizou todos os processos e morreu. Para que possamos ver ele podemos executar o comando docker ps com o parâmetro `-a`, assim teremos uma lista de todos os contêineres do nosso host. <br/>

**`docker stats (id ou apelido do container)`** <br/>
Para que possamos ter informações sobre um contêiner nos executamos o comando acima, ele nos retorna dados como: <br/>
   `CONTAINER` — ID do Container; <br/>
   `CPU %` — uso de CPU em porcentagem; <br/>
   `MEM USAGE / LIMIT` — Memória usada/Limite que você pode ter setado; <br/>
   `MEM` — uso de memória em porcentagem; <br/>
   `NET I/O` — I/O de Internet; <br/>
   `BLOCK IO` — Outros processos de I/O; <br/>

**`docker inspect (id da imagem ou container)`** <br/>
Caso você precise de mais detalhes sobre a sua imagem ou o seu contêiner, podemos utilizar o comando inspect. Ele irá retornar um json com todas as informações relacionadas a nossa busca. <br/>

**`docker inspect {{id container}} | grep -i ip`** <br/>
Para inspecionar o IP Adress 

**`docker rmi (nome da imagem)`** <br/>
Caso você tenha baixado uma imagem errada ou queira deletar alguma por um outro motivo, basta executar o comando a cima que ele deleta ela do seu host. <br/>

### Execução de Containers <br/>

**`docker exec (id_container ou nome_container)`** <br/>
Com o exec nós podemos executar qualquer comando nos nossos contêineres sem precisarmos estar na console deles. <br/>

**`docker container run <parâmetros> <imagem> <CMD> <argumentos>`** <br/>
Os parâmetros mais utilizados na execução do container são: <br/>
   `-i` Permite interagir com o container <br/>
   `-t` Associa o seu terminal ao terminal do container <br/>
   `-it` É apenas uma forma reduzida de escrever `-i -t` <br/>
   `--name algum-nome` Permite atribuir um nome ao container em execução <br/>
   `-p 8080:80` Mapeia a porta 80 do container para a porta 8080 do host <br/>
   `-d` Executa o container em background <br/>
   `-v /pasta/host:/pasta/container` Cria um volume '/pasta/container' dentro do container com o conteúdo da pasta '/pasta/host' do host <br/>
   `--rm` Automaticamente remove o container após finalização (Não funciona com -d) <br/>
   `-m` Limitar o uso de memória RAM <br/>
   `-c` Balancear o uso de CPU <br/>

**`docker exec -it 8b54c76e81b7 /bin/bash`** <br/>
Podemos acessar o bash do container com o comando acima <br/>

**`docker exec 8b54c76e81b7 mkdir /temp/`** <br/>
Estamos agora criando um novo diretório dentro do nosso contêiner sem precisarmos entrar nele. <br/>

**`docker exec 8b54c76e81b7 touch /temp/dotnetsp.txt`** <br/>
Agora criamos um arquivo dentro do nosso diretório temp que acabamos de criar dentro do nosso contêiner <br/>

**`docker attach (id_container ou nome_container)`** <br/>
Vamos agora entrar dentro do nosso contêiner e verificar o nosso novo arquivo criado. Para isso execute o comando acima + o nome do ou id do seu contêiner, podemos observar que a execução dele irá nos dar acesso de root no nosso contêiner. <br/>
Agora execute o comando `ls /temp` para verificar o nosso arquivo dentro diretório que nós criamos chamado temp <br/>
Para sair do contêiner execute o comando exit <br/>

**`docker start (id do container)`** <br/>
Quando nós saimos do nosso container com o comando exit, ele mata o nosso processo. Pelo comando `docker ps` ele não está mais sendo listado <br/>
Para que possamos dar um dettach ou em outras palavras sair sem fecharmos ele, nós precisamos dos comandos: Ctrl+p + Ctrl+q. Para que possamos subir ele novamente vamos utilizar o comando start. <br/>
Agora execute o comando attach novamente para entrar no seu contêiner e depois os comandos : Ctrl+p + Ctrl+q, em seguida podemos verificar que o nosso contêiner ainda está em execução <br/>

**`docker stop (id ou nome container)`** <br/>
Para que possamos parar um contêiner que esteja sendo executado, nós utilizamos o comando stop.

**`docker run -it -d -p 8080:80 nginx`** <br/>
Para que possamos ter um contêiner sendo executado em background, nós podemos utilizar eles em segundo plano. Para isso, só precisamos passar o comando -d. Caso o seu contêiner ainda esteja sendo executado em primeiro plano, execute o comando `control C` para sair dele, em seguida execute o mesmo comando anterior só que agora com o parâmetro -d <br/>

**`docker container run -it --rm -m 512M python`** <br/>
Para limitar o uso de memória RAM que pode ser utilizada por esse container, basta executar o comando acima

**`docker container run -it --rm -c 512 python`** <br/>
Para balancear o uso da CPU pelos containers, utilizamos especificação de pesos para cada container, quanto menor o peso, menor sua prioridade no uso. Os pesos podem oscilar de 1 a 1024. Caso não seja especificado o peso do container, ele usará o maior peso possível, nesse caso 1024. Para entendimento, vamos imaginar que três containers foram colocados em execução. Um deles tem o peso padrão 1024 e dois têm o peso 512. Caso os três processos demandem toda CPU o tempo de uso deles será dividido 50%, 25% e 25%.

*Volumes Anonymos*

**`docker volume ls`** <br/>

**`docker volume`** <br/>

**`docker volume create <nome_volume>`** <br/>

**`docker volume inspect <nome_volume>`** <br/>

Com isso é possível compartilhar o mesmo volume para mais de um container <br/>

*Volumes Windows*

Para habilitar um diretório do windows é necessário primeiramente ir em Settings > Resources > File Sharing <br/>
Clicar no botão de plus para adicionar um diretório que será usado para montar com um volume do container <br/>

**`docker run -it -v c:/data:/windows ubuntu`** <br/>
Exemplo de mapeamento de volume compartilhado através da vm do docker <br/>

### Criando Imagens <br/>

É possível criar imagens executando o comando commit, relacionado a um container. Esse comando usa o status atual do container escolhido e cria a imagem com base nele. <br/>
Vamos ao exemplo. Primeiro criamos um container qualquer: <br/>
**`docker container run -it --name containercriado ubuntu:16.04 bash`** <br/>

Agora que estamos no bash do container, instalamos o nginx: <br/>
**`apt-get update`** <br/>
**`apt-get install nginx -y`** <br/>
**`exit`** <br/>
**`docker container stop containercriado`** <br/>

Agora, efetuamos o commit desse container em uma imagem: <br/>
**`docker container commit containercriado meuubuntu:nginx`** <br/>
No exemplo do comando acima, `containercriado` é o nome do container criado e modificado nos passos anteriores; o nome `meuubuntu:nginx` é a imagem resultante do commit; o estado do containercriado é armazenado em uma imagem chamada meuubuntu:nginx que, nesse caso, a única modificação que temos da imagem oficial do ubuntu na versão 16.04 é o pacote nginx instalado. A primeira parte do nome da imagem `meuubuntu:nginx` representa o repositorio (meuubuntu) e a segunda parte representa a tag (nginx) <br/>

**`docker container run -it --rm meuubuntu:nginx dpkg -l nginx`** <br/>
Para testar sua nova imagem, vamos criar um container a partir dela e verificar se o nginx está instalado  <br/>

**`docker container run -it --rm ubuntu:16.04 dpkg -l nginx`** <br/>
Se quiser validar, pode executar o mesmo comando na imagem oficial do ubuntu  <br/>

-----

*Gerando uma imagem apartir de um Dockerfile*

Observação: se o projeto utilizar maven, rodar: `mvn clean package` antes de executar o build

**`docker build --no-cache -t jpralves/exjtecnologia-k8s .`** <br/>
O Comando acima faz o build de uma imagem com o nome "jeanalves/exjtecnologia-k8s" apartir de um Dockerfile na mesma pasta.

**`docker run -p 3000:3000 jpralves/exjtecnologia-k8s`** <br/>
Para testar a imagem é só dar um docker run

**`docker push jpralves/exjtecnologia-k8s:latest`** <br/>
Para Enviar para docker hub

*Enviar suas Imagens Docker para o DockerHub *<br/>
1) Criar conta no DockerHub <br/>
2) Realizar logout **`docker logout`**
3) Realizar login **`docker login -u jpralves -p password`** e realizar o login <br/>
4) **`docker push meuubuntu:nginx`** <br/>

-----

### Dockerfile <br/>
`FROM ubuntu:16.04` <br/>
`RUN apt-get update && apt-get install nginx -y` <br/>
`COPY arquivo_teste /tmp/arquivo_teste` <br/>
`CMD bash` <br/>

`FROM` para informar qual imagem usaremos como base, nesse caso foi ubuntu:16.04. <br/>
`RUN` para informar quais comandos serão executados nesse ambiente para efetuar as mudanças necessárias na infraestrutura do sistema. São como comandos executados no shell do ambiente, igual ao modelo por commit, mas nesse caso foi efetuado automaticamente e, é completamente rastreável, já que esse Dockerfile será armazenado no sistema de controle de versão. <br/>
`COPY` é usado para copiar arquivos da estação onde está executando a construção para dentro da imagem. Usamos um arquivo de teste apenas para exemplificar essa possibilidade, mas essa instrução é muito utilizada para enviar arquivos de configuração de ambiente e códigos para serem executados em serviços de aplicação. <br/>
`CMD` para informar qual comando será executado por padrão, caso nenhum seja informado na inicialização de um container a partir dessa imagem. No exemplo, colocamos o comando bash, se essa imagem for usada para iniciar um container e não informamos o comando, ele executará o bash. <br/>

Após construir seu Dockerfile basta executar o comando abaixo: <br/>

**`docker image build -t meuubuntu:nginx_auto .`**
A opção `-t`, serve para informar o nome da imagem a ser criada. No caso, será meuubuntu:nginx_auto e o `.` ao final, informa qual contexto deve ser usado nessa construção de imagem. Todos os arquivos da pasta atual serão enviados para o serviço do docker e apenas eles podem ser usados para manipulações do Dockerfile (exemplo do uso do COPY).<br/>


## Keycloak

**`docker pull quay.io/keycloak/keycloak:latest`** <br/>
**`docker run -d -p 8080:8080 -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin quay.io/keycloak/keycloak:latest`** <br/>

Link de acesso a API Rest Keycloak: <br/>
**`https://www.keycloak.org/docs-api/5.0/rest-api/index.html`** <br/>

Link para acesso aos endpoints de config: <br/>
**`http://keycloakhost:keycloakport/auth/realms/{realm}/.well-known/openid-configuration`** <br/>

Executar bash como root: <br/>
**`docker exec -u root -it cecc964a946d bash`** <br/>

Ver conteúdo do arquivo: <br/>
**`docker exec -it cecc964a946d cat /opt/jboss/keycloak/standalone/configuration/standalone-ha.xml`** <br/>

As seguintes mudanças para Standalone <br/>
`<theme>` <br/>
&nbsp;&nbsp;`<staticMaxAge>-1</staticMaxAge>` <br/>
&nbsp;&nbsp;`<cacheThemes>false</cacheThemes>` <br/>
&nbsp;&nbsp;`<cacheTemplates>false</cacheTemplates>` <br/>
&nbsp;&nbsp;`...` <br/>
`</theme>` <br/>

Caminhos que se encontram os arquivos para mudar o Thema: <br/>
**`docker exec -u root -it cecc964a946d ls /opt/jboss/keycloak/themes/keycloak/login/resources/img/`** <br/>
**`docker exec -u root -it cecc964a946d ls /opt/jboss/keycloak/themes/keycloak/login/resources/css/`** <br/>


## Kafka

Run Kafka Commands inside the container:

*List Brokers* <br/>
**`docker exec -ti kafka /usr/bin/broker-list.sh`** <br/>

*Create a Topic* <br/>
**`docker exec -ti kafka /opt/bitnami/kafka/bin/kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic testTopic`** <br/>

*List Topics* <br/>
**`docker exec -ti kafka /opt/bitnami/kafka/bin/kafka-topics.sh --list --zookeeper zookeeper:2181`** <br/>

*Producer Payload* <br/>
**`docker exec -ti kafka /opt/bitnami/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic testTopic`** <br/>
>This is my first message <br/>
>Here is my second message <br/>
>Final message <br/>

*Consumer Payload* <br/>
**`docker exec -ti kafka /opt/bitnami/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic testTopic --from-beginning`** <br/>
