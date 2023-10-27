- Toda vez  que um container é executado com run o docker tenta achar o container
localmente senão ele baixa o container.

- Geralmente a porta 80 é a home dos sites.

- Docker run sempre cria um container novo se ja existir deve se usar o start.

- Imagem nada mais é que um conjunto de comandos que vai ser executado pelo container.

- Nesse site são encontrados container e imagens oficiais ou não https://hub.docker.com/

- Para criar uma imagem precisamos de um Dockerfile.

- Sempre que fizer alguma alteração é necessário buildar a imagem/dockerfile novamente, porém a execução será mais rapida pois ele só vai executar a camada modificada, as outras estão cacheadas.

- Os dois pontos (:) é usado no docker para determinar a versão de uma imagem.

- Sempre que for subir uma atualização da imagem para o hub do docker deve se fazer um novo build trocando a tag.

### ----------------------- CONTAINERS -----------------------

# Executar container de test do proprio docker
> docker run docker/whalesay cowsay Algum_Texto

# Exibir os containers estão sendo executados
> docker ps
ou
> docker container ls

# Exibir os containers que foram executados na maquina
> docker ps -a

# Executar container e deixar rodando no terminal
> docker run -it nome_ou_id_do_container

# Executar container e desatachar da execução
> docker run -d nome_ou_id_do_container

# Remove o container após o uso
> docker run --rm nome_ou_id_do_container

# Executar container com a exposição de portas (o primeiro numero é a porta de deseja expor(container), e o segundo é através de qual porta expor(pc))
> docker run -p 3000:80 nome_ou_id_do_container

# Inicia execução do container (pode ser usado somente as 3 primeiras letras do id)
> docker start id_do_container

# Para execução do container (id ou nome)
> docker stop nome_ou_id_do_container

# Define um nome para o container
> docker run --name nome_do_container nome_ou_id_daimagem_desejada

# Exibe os ultimos logs do container
> docker logs id_do_container

# Exibe os logs do container em tempo real
> docker logs -f id_do_container

# Remover container (irreversível)
> docker rm nome_ou_id_do_container

# Força a remoção do container mesmo que esteja sendo executado (irreversível)(o comando stopa e depois remove)Þ
> docker rm -f nome_ou_id_do_container

# Utilizando o help é mostrado uma lista de possibilidades do comando
> docker run --help

# Copiar arquivo do container para o pc ou vice versa
> docker cp nome_do_container:/diretório/arquivo.extensao ./diretório

# Verificar informações de processamento
> docker top nome_ou_id_do_container

# Verificar dados do container
> docker inspect nome_ou_id_do_container

# Verificar dados do docker
> docker stats

# Logar no docker
> docker login

# Deslogar no docker
> docker logout

# Roda um container com um network junto(-e é uma env)
> docker run -d -p 3307:3306 --name mysql_api_cont --rm --network flasknetwork -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysqlapinetwork

### ----------------------- IMAGENS -----------------------

**Existem algumas camadas na criação da imagem(dockerfile) que precisam ser configuradas para o container conseguir executar a imagem**

Nessa camada voce define a imagem base do container
>FROM imagem_base

Nessa camada é o diretório de trabalho
>WORKDIR /dir

Nessa camada são a copia de alguns arquivos se necessário(quais arquivos e para onde, o ponto sozinho é o proprio container)
>COPY package*.json .

Nessa camada são executados os comandos
>RUN npm i

Nessa camada é definida as portas para serem expostas
>EXPOSE 3000

Nessa camada é feita a execução semelhante ao run, porém permite um array de comandos
>CMD ["node", "app.js"]

# Gerando uma imagem
> docker build .

# Gerando uma imagem com nome/tag
> docker build -t nome_da_imagem:tag_da_imagem .

# Lista as imagens
> docker images

# Baixar imagem do docker hun
> docker pull nome_da_imagem

# Nomear e tagear imagem, tag nao é obrigatório
> docker tag id_da_imagem nome_desejado:tag_desejada

# Remover imagem(irreversível)
> docker rmi nome_ou_id_da_imagem

# Força a remoção da imagem mesmo que esteja sendo executado (irreversível)(o comando stopa e depois remove)
> docker rmi -f nome_ou_id_da_imagem

# Remove tudo do docker (imagem, container, networks, cache ...)(irreversível)
> docker system prune

# Enviar imagem para o hub do docker(necessário estar logado)(necessário build a imagem exatamente com o nome do repo)
> docker push seu_nick/seu_repo

### ----------------------- VOLUMES -----------------------

*** Quando salvamos dados no container se deletarmos o container perdermos os dados, o volume vem para resolver isso salvando em volumes ***

- Anônimos: geralmente usados em testes, não é reaproveitado.
- Nomeados: podem ser referenciados por isso é recomendando para aplicações.
- Bind Mounts: salva os dados fora do docker em algum diretório do pc.

# Rodar um container atrelado a um volume anônimo precisa adicionar a flag -v
> docker run -d -p 80:80 --name nome_do_container -v /data phpmessages

# Rodar um container atrelado a um volume nomeado precisa adicionar a flag -v com o nome (igual está no workdir)
> docker run -d -p 80:80 --name nome_do_container -v nome_do_volume phpmessages

# Rodar um container atrelado a um bind mount precisa adicionar a flag -v com o path do diretório(pode dar problema no windows)(colocar entre aspas o path se tiver espaço ou letras maiusculas)
> docker run -d -p 80:80 --name nome_do_container -v path_dir:nome_do_volume phpmessages

# Rodar um container atrelado a um volume somente leitura precisa adicionar a flag -v com o nome e :ro
> docker run -d -p 80:80 --name nome_do_container -v nome_do_volume:ro phpmessages

# Mostrar volumes
> docker volume ls

# Criar volumes
> docker volume create nome_do_volume

# Checar volumes
> docker volume inspect nome_do_volume

# Remover volumes
> docker volume rm nome_do_volume

# Remover volumes em massa
> docker volume prune

### ----------------------- NETWORKS -----------------------

- Externas: conexões com serviços na web ou apis.
- Com o host: conexão com a maquina que está executando docker.
- Entre containers: conexões com outros containers no docker.(utiliza driver bridge)

Tipos de rede(drivers)
- Bridge: o mais comum, utilizado com o default do docker para comunicação entre containers.
- Host: conexão com a máquina que executa o docker.
- Macvlan: conexão com um container por meio de um MAC address.
- None: remove todas as conexões do container.
- Plugins: permite extensões de terceiros para criar redes.

# Mostrar redes
> docker network ls

# Criar redes(como padrão ele criar bridge)(-d para definir driver)
> docker network create nome_da_rede
> docker network create -d tipo_do_driver nome_da_rede

# Remover redes
> docker network rm nome_da_rede

# Remover redes em massa
> docker network prune

# Conectar um container a uma rede
> docker network connect nome_da_rede id_do_container

# Desconectar um container de uma rede
> docker network disconnect nome_da_rede id_do_container

# Inspecionar redes
> docker network inspect nome_da_rede

### ----------------------- YAML -----------------------

Yaml é uma linguagem de serialização e não de marcação como o md, é usada geralmente em configurações

### ----------------------- DOCKER COMPOSE -----------------------

É uma ferramenta do proprio docker para rodar multiplos containers e facilitar o trabalho.

É recomendado criar um env_file(db.env) para controlar as variáveis no compose, para chamar essas variáveis basta usar ${VARIAVEL}

Algumas chaves usadas
- Version: versão do compose.
- Services: containers que vão rodar.
- Volumes: se tiver algum volume.
- Networks: se tiver alguma rede.

# Checar versão
> docker-compose --version

# Rodar compose
> docker-compose up

# Rodar desatachado
> docker-compose up -d

# Parar compose
> docker-compose down

# Verificar compose
> docker-compose ps
