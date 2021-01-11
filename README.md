## Main Installation

`
sudo apt install git htop synaptic curl wget openjdk-8-jdk-headless openjdk-8-doc openjdk-8-source openjdk-8-demo maven postgresql-client mysql-client docker.io virtualbox vlc ssh keepassxc
`

## IDE's & Util's

### VsCode
`
https://code.visualstudio.com/docs/?dv=linux64_deb
sudo dpkg -i code_1.46.1-1592428892_amd64.deb
`

### Dbeaver
`
https://dbeaver.io/files/dbeaver-ce_latest_amd64.deb
sudo dpkg -i dbeaver-ce_7.1.1_amd64
`

### Zoom
`
https://zoom.us/download#client_4meeting
sudo dpkg -i zoom_amd64.deb
sudo apt --fix-broken install
`

### Slack
`
https://slack.com/intl/pt-br/downloads/instructions/ubuntu
sudo dpkg -i slack-desktop-4.4.3-amd64.deb
`

### Teams
`
https://teams.microsoft.com/_#/guestLicense
sudo dpkg -i teams_1.3.00.5153_amd64.deb
`

### Intelij
`
https://www.jetbrains.com/pt-br/idea/download/#section=linux
`

### NodeJs
```
# https://github.com/nodesource/distributions/blob/master/README.md
# https://github.com/nodesource/distributions
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
```


### bombsquad server
```
# libpython3.7m.so.1.0:
sudo apt install python3 python3-dev
wget https://files.ballistica.net/bombsquad/builds/BombSquad_Server_Linux_1.5.19.tar.gz
tar -zxvf ./BombSquad_Server_Linux_1.5.19.tar.gz
```

### portainer.io
```
docker volume create portainer_data && 
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer 

## acesso -> localhost:9000
```

## Config

### ssh

#### Generating key

```
ssh-keygen -o
# id_rsa.pub nome da chave publica
# caso não tenha digitado nome no comando acima para gerar a chave com outro nome
cat ~/.ssh/id_rsa.pub
```

### Swap

```
sudo fallocate -l 4G /swapfile &&
sudo chmod 600 /swapfile &&
sudo mkswap /swapfile &&
sudo swapon /swapfile &&
sudo cp /etc/fstab /etc/fstab.bak &&
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

## Git

Configurar chave ssh por repo
```
git config --local core.sshCommand "/usr/bin/ssh -i ~/.ssh/id_rsa"
```

Configurar usuário por repo
```
git config user.name [userName]
git config user.email [userEmail]
```

Pull com chave ssh especifica
```
git clone git@github.com:user/repo.git --config core.sshCommand="ssh -i ~/.ssh/id_rsa"
```

## Utils

### Copy iso
```
sudo dd status=progress if=name-of.iso of=/dev/sdb
```

### Remove pre-installed softwares
```
tasksel --list-tasks
apt-get purge $(tasksel --task-packages desktop)
apt autoremove
```

### Sensors
```
sudo apt-get install lm-sensors 
sudo sensors-detect
sudo service kmod start
sensors
```

## QEMU

```
sudo apt-get install virt-manager qemu qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
sudo usermod --append --groups libvirt `whoami`

```

## Networkd speed test

``` 
curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python -
```

## SSH

### gerar sh
```
ssh-keygen

```

### copiar ssh para server remoto
```

ssh-copy-id user@192.168.1.121 && ssh user@192.168.1.121
```

### install sudo
```
sudo usermod -aG sudo user && sudo su user
```

## Jedi Academy
### Status
```
printf '\xFF\xFF\xFF\xFFgetstatus' | nc -u -n -w 1 127.0.0.1 29070
```

Info
```
printf '\xFF\xFF\xFF\xFFgetinfo' | nc -u -n -w 1 127.0.0.1 29070
```

## Java

### Update Alternatives
```
sudo update-alternatives --config java
```

## Maven

### Genereate multimodule

```

mvn archetype:generate -DgroupId=com.gamemanager -DartifactId=game-manager-jk-agent
nano pom.xml
# <packaging>pom</packaging>

cd game-manager-jk-agent
mvn archetype:generate -DgroupId=com.gamemanager  -DartifactId=game-manager-jk-web &&
mvn archetype:generate -DgroupId=com.gamemanager  -DartifactId=game-manager-jk-cli &&
mvn archetype:generate -DgroupId=com.gamemanager  -DartifactId=game-manager-jk-server-manager &&
mvn archetype:generate -DgroupId=com.gamemanager  -DartifactId=game-manager-jk-server-connector

```

## Curl

### Enviar arquivo form data

```
curl --location --request POST 'http://192.168.1.1:8081/api/form' \
--header 'token: 123456' \
--header 'Content-Type: multipart/form-data' \
--form 'file=@/home/user/downloads/arquivo.pdf'
```

## GCP

init
```
gcloud init
```

logoff
```
gcloud auth revoke
```

## JVM

Remote debug

Intelij Run > Remote > Debug (localhost:8001)
```
#gradle
#jvmFlags = ['-Xdebug', '-Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8000']

-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:8000
```

## Audio


### Record and play

```
arecord -f cd - | aplay -

#If you wanna play while saving:

arecord -f cd - | tee output.wav | aplay -
```
### Microphone clean

```
#https://medium.com/@tim_73574/ubuntu-microphone-noise-cancellation-and-volume-auto-adjusted-issue-2c79678542bb

>> sudo vim /etc/pulse/default.pa
### Enable Echo/Noise-Cancelation
load-module module-echo-cancel aec_method=webrtc aec_args="analog_gain_control=0 digital_gain_control=1" source_name=echoCancel_source sink_name=echoCancel_sink
set-default-source echoCancel_source
set-default-sink echoCancel_sink
>> pulseaudio -k
>> pulseaudio --start

```
### Pulseaudio control

```
apt install pavucontrol
pavucontrol
```

### Restart pulseaudio
```
pulseaudio -k ; pulseaudio --start
```

## Containers

### Geral
Log:
`
docker logs --follow --tail 10 --timestamps [containerId]
`

Bash:

`
docker exec -it [containerId] /bin/bash
`

Attach:

`
docker container attach [containerId] 
`

Remove container

`
docker container rm [containerId]
`

Buildar imagem de um container via Dockerfile
```
docker image build [pathToDockerfile]
# Output o id da imagem
```

Iniciando uma imagem de um container
```
docker container run [imageId]
```

Iniciando uma imagem de um container com bash
```
docker container run -it [imageId]  bash
```

Buildar todos containers de um docker-compose.yml

`
docker-compose build
`

Iniciar todos containers de um docker-compose.yml

`
docker-compose up
`

docker log file location
`
/var/lib/docker/containers/[container-id]/[container-id]-json.log
/var/lib/docker/containers/[container-id]/local-logs
`

`
docker inspect --format='{{.LogPath}}'
`

run docker with local log
`
#https://docs.docker.com/config/containers/logging/local/
docker run \
      --log-driver local --log-opt max-size=10m \
      alpine echo hello world
`

Remover imagem docker
`
docke rmi [imageId]
`

Remover:
        - all stopped containers
        - all networks not used by at least one container
        - all dangling images
        - all build cache
```
docker system prune 
docker volume prune

docker image prune

#docker-compose rm -f 
```

Criar volume
docker volume create postgres-dev

Executar compose
```
docker-compose exec -T eats-application-mysql-db mysql -uroot -pcaelum123 eats -N -B -e "select r.id, r.cep, r.tipo_de_cozinha_id from restaurante r where r.aprovado = true;" |
sed -E 's/[[:space:]]+/,/g' | 
docker-compose exec -T eats-distancia-mongo-db mongoimport --db eats_distancia --collection restaurantes --type csv  --fields=_id,cep,tipoDeCozinhaId
```

Referencias:
```
# https://docs.docker.com/compose/gettingstarted/
# https://docs.docker.com/get-started/part2/
```

### MySQL

```
# https://hub.docker.com/_/mysql

docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=default -p 127.0.0.1:3306:3306/tcp -d mysql:latest --default-authentication-plugin=mysql_native_password
```

### Postgresql

```
docker run --name postgres-db -e POSTGRES_PASSWORD=postgres -p 127.0.0.1:5432:5432/tcp -v postgres-dev:/var/lib/postgresql/data -d postgres:9.6
```

```
pg_restore --create --host=localhost --port=5432 --username=postgres --password --dbname=docket --format=c /home/admin/Docket/dump/backup_saasv2_saasv2_20200709100310.backup
```

### Rabbitmq

Iniciando container e bindando portas
```
# https://hub.docker.com/_/rabbitmq

docker run -d --hostname my-rabbit --name some-rabbit -p 127.0.0.1:5672:5672/tcp -p 127.0.0.1:4369:4369/tcp -p 127.0.0.1:5671:5671/tcp -p 127.0.0.1:15672:15672/tcp  -p 127.0.0.1:25672:25672/tcp -e RABBITMQ_DEFAULT_USER=guest -e RABBITMQ_DEFAULT_PASS=guest rabbitmq:3 
```

Ativando acesso remoto via web

`
docker exec -it some-rabbit rabbitmq-plugins enable --offline rabbitmq_mqtt rabbitmq_federation_management rabbitmq_stomp
`

Reiniciando o container após ativar o acesso web (necessário)

`
docker restart some-rabbit
`

Removendo o container

`
docker stop some-rabbit && docker rm some-rabbit
`



```
