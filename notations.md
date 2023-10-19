- Toda vez  que um container é executado com run o docker tenta achar o container
localmente senão ele baixa o container.

- Geralmente a porta 80 é a home dos sites.

- Docker run sempre cria um container novo se ja existir deve se usar o start.

- Imagem nada mais é que um conjunto de comandos que vai ser executado pelo container.

- Nesse site são encontrados container e imagens oficiais ou não https://hub.docker.com/

- Para criar uma imagem precisamos de um Dockerfile.

- Sempre que fizer alguma alteração é necessário buildar a imagem/dockerfile novamente, porém a execução será mais rapida pois ele só vai executar a camada modificada, as outras estão cacheadas.

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
> docker run -it algum_container

# Executar container e desatachar da execução
> docker run -d algum_container

# Executar container com a exposição de portas (o primeiro numero é a porta de deseja expor(container), e o segundo é através de qual porta expor(pc))
> docker run -p 3000:80 algum_container

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

# Força a remoção do container mesmo que esteja sendo executado (irreversível)(o comando stopa e depois remove)
> docker rm -f nome_ou_id_do_container


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

# Executando uma imagem
> docker build diretório_da_imagem

# Lista as imagens
> docker image ls