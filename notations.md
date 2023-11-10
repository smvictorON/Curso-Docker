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

### ----------------------- SWARM -----------------------

É uma ferramenta nativa do proprio docker para osquestrar containers!
Alguns conceitos são utizados na orquestração como

- Nodes: são instancias(maquinas) que participam do Swarm
- Manager Node: é a máquina que gerencia as outras máquinas, pode ter mais de um.
- Worker Node: são as máquinas que "trabalham" para as Managers Nodes
- Service: conjunto de tasks que o manager manda para o worker executar.
- Tasks: comandos que são executados nos nodes.

*Ao criar um novo node na AWS deve se instalar e iniciar o docker*
> sudo yum install docker
> sudo service docker start

**NODE1**
ssh -i "curso_docker.cer" ec2-user@ec2-3-15-21-149.us-east-2.compute.amazonaws.com

**NODE2**
ssh -i "curso_docker.cer" ec2-user@ec2-3-144-25-145.us-east-2.compute.amazonaws.com

**NODE3**
ssh -i "curso_docker.cer" ec2-user@ec2-3-137-176-59.us-east-2.compute.amazonaws.com

**CONECTAR NODES AO SWARM**
sudo docker swarm join --token SWMTKN-1-343bgai9mhpdnhzn3nku6p5e4idvhqc4iewnl3vrvqjfw9qvc2-1jdraj5fy7scdf654fj1f7a8h 3.15.21.149:2377


# Iniciar Swarm (pode ser necessário declarar IP --advertise-addr)
> docker swarm init

# Sair do Swarm (pode ser necessário usar -f se for o manager)(se for um node ele deixa o status "down" porém continua mapeado no swarm)
> docker swarm leave

# Remover node do swarm
> docker node rm id_do_node

# Verificar nodes ativos no swarm (só para node manager)
> docker node ls

# Adicionar nodes (comando que aparece ao dar o docker swarm init)
> docker swarm join --token <TOKEN> <IP>:<PORTA>

# Subindo serviço (pode ser necessário usar o -p 80:80)
> docker service create --name nome_do_serviço nome_da_imagem

# Listar serviços no swarm (só para node manager)
> docker service ls

# Remover serviço no swarm (só para node manager)
> docker service rm nome_ou_id_do_serviço

# Replicar serviços no swarm (só para node manager)
> docker service create --name nome_do_serviço --replicas numero_de_replicas nome_da_imagem

- Ao executar as replicas, mesmo que entremos nas replicas e paramos o container manualmente o node manager se encarregar de subir o container novamente!

# Checando token do swarm (só para node manager)(token para add um manager)
> docker swarm join-token manager

# Checando token do swarm (só para node manager)(token para add um worker)
> docker swarm join-token worker

# Checar estrutura do swarm
> docker info

# Inspecionar serviços (só para node manager)
> docker service inspect id_ou_nome_do_serviço

# Verificar contianers ativados pelo service
> docker service ps id_do_service

# Rodar compose com swarm
> docker stack deploy -c arquivo.yaml nome

# Escalando aplicação com replicas
> docker service scale nome_do_service=numero_de_replicas

# Ignorar tasks no node (drain ignora, para voltar só trocar por active)
> docker node update --availability drain id_do_node

# Atualiza imagem no swarm (somente nodes active receberão a atualização)
> docker service update --image nome_da_imagem id_do_service

# Atualiza network no swarm (somente nodes active receberão a atualização)
> docker service update --network-add nome_da_rede id_do_service

# Criar rede para swarm (overlay é o tipo de conexao entre nodes)
> docker network create --driver overlay nome_da_rede

### ----------------------- KUBERNETES (K8s) -----------------------

Muito semelhante ao Swarm que é nativo do Docker, porém Kubernetes veio antes e foi criado pelo Google para orquestração de containers.

Alguns conceitos

- Control Pane: onde é gerenciado o controle dos processos dos nodes.
- Nodes: maquinas gerenciadas pelo control pane.
- Deployment: execução de uma imagem por um pod.
- Pod: um ou mais containers em um node.
- Services: serviços que expõe os pods ao mundo externo.
- kubectl: CLI do kubernetes

**Método imperativo é quando iniciamos a aplicação com comandos**
**Método declarativo funciona como se fosse o docker compose**

Comando mais usados no modo declarativo

- apiVersion: versão da ferramenta
- kind: tipo do arquivo (Deployment ou Service)
- metadada: descrever um objeto com chaves
- replicas: numero de replicas
- containers: definir configurações do container

*Se for colocar o service e o deployment no mesmo yaml, iniciar com --- colocar o service depois na outra linha separar com --- e colocar as config do deployment*

**Para essa sessão instalei o kubectl e minikube**

# Iniciar minikube (driver pode ser o docker ou qualquer outra vm)
> minikube start --driver=<DRIVER>

# Parar minikube
> minikube stop

# Testar minikube
> minikube status

# Saber versão
> minikube version

# Acessar dashboard (adicionar --url para ver somente a url)
> minikube dashboard

# Deploy
> kubectl create deployment <NOME> --image=<IMAGEM>

# Checar deploys
> kubectl get deployments

# Descrever deploys
> kubectl describe deployments

# Checar pods
> kubectl get pods

# Descrever pods
> kubectl describe pods

# Mostrar configs do kubernetes
> kubectl config view

# Criar service (o tipo mais usado é o LoadBalancer)
> kubectl expose deployment <NOME> --type=<TIPO> --port=<PORT>

# Gerando IP para o service
> minikube service <NOME>

# Checar services
> kubectl get services

# Descrever services
> kubectl describe services/<NOME>

# Escalando aplicação (tanto para aumentar quanto para diminuir as replicas)
> kubectl scale deployment/<NOME> --replicas=<NUMERO>

# Checar numero de replicas
> kubectl get rs

# Atualizar imagem
> kubectl set image deployment/<NOME> <CONTAINER>=<IMAGEM>

# Verificar alteração
> kubectl rollout status deployment/<NOME>

# Desfazer alteração (rollback)
> kubectl rollout undo deployment/<NOME>

# Deletar serviço
> kubectl delete service <NOME>

# Deletar deployment
> kubectl delete deployment <NOME>

# Executar arquivo de deployment/service(yaml) no modo declarativo
> kubectl apply -f <ARQUIVO>

# Parar deployment/service(yaml) no modo declarativo
> kubectl delete -f <ARQUIVO>

### ----------------------- COMANDOS TERMINAL (UNIX) -----------------------

**Terminal é onde o shell é executado**
**Shell é o que executa o comando**
**Podemos concatenar comandos com &&**

BANDEIRA + R no terminal abre a busca de comandos <--- Muito útil

Ex: Estrutura de comandos
> comando -opçoes arquivos/diretorios

# cd (change directory)

leva ao ultimo diretório
> cd -

volta para o diretório acima
> cd ..

leva ao diretorio home
> cd ~

mostra diretórios na pasta ou auto-complete
> cd + tab

# ls (list)

mostrar arquivos com detalhes
> ls -l

mostrar arquivos com detalhes e tamanho
> ls -lh

mostrar todos os arquivos ocultos tambem
> ls -a

mostrar todos os arquivos ocultos tambem com detalhes
> ls -la

mostrar arquivos com data da ultima modificação
> ls -ltr

mostrar arquivos do diretorio apontado
> ls Diretório

mostrar arquivos em ordem reversa
> ls -r

mostrar subdiretorios
> ls -R

mostrar arquivos em ordem de tamanho
> ls -S

mostrar arquivos separados por virgula
> ls -m

# cat (concatenate)

ler arquivo de texto
> cat arquivo

ler mais de um arquivo de text
> cat arquivo1 arquivo2

unir os dois arquivos em um terceiro
> cat arquivo1 arquivo2 > arquivo3

ler arquivo de texto com linhas numeradas
> cat -n arquivo

adicionar ao final do arquivo
> cat arquivo_origem >> arquivo_destino

# touch

modifica data de alteração para atual de um arquivo existente
> touch arquivo

criar arquivo
> touch arquivo

criar multiplos arquivos
> touch arquivo1 arquivo2

# man (manual)

mostra o manual do comando
> man comando
