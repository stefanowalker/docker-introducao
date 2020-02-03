# Introdução ao Docker em 22 minutos

Projeto gerado na videoaula do canal no YouTube Programador a Bordo.

Link da aula:
https://www.youtube.com/watch?v=Kzcz-EVKBEQ

## Como rodar

### Instalando dependências
Acesse a pasta `./api` no terminal e execute:
```
npm install
```

Com isso instalamos as dependências Node que precisamos. Estou utilizando Node 10.

### Construindo as imagens

Acesse a pasta raíz do projeto e construa cada imagem (MySQL, Node API e PHP):

```
docker build -t mysql-image -f api/db/Dockerfile .
```
```
docker build -t node-image -f api/Dockerfile .
```
```
docker build -t php-image -f website/Dockerfile .
```
// -t significa tag e nome em seguida é o nome escolhido por mim.

// -f significa que o file que eu devo usar para construir o docker

// . o ponto ao final significa que o contexto da execução é a pasta atual (do projeto)


### Ver as imagens das libs/modulos criados
```
docker image ls
```

### Criando os containers
```
docker run -d --rm --name mysql-container mysql-image
```

// com o comando docker ps dá pra ver os containers criados, rodando

### Rodando os containers
Na pasta raíz do projeto, execute um de cada vez:

```
docker run -d -v $(pwd)/api/db/data:/var/lib/mysql --rm --name mysql-container mysql-image
```
```

docker run -d -v $(pwd)/api:/home/node/app -p 9001:9001 --link mysql-container --rm --name node-container node-image
```
```
docker run -d -v "$(pwd)/website":/var/www/html -p 8888:80 --link node-container --rm --name php-container php-image
```

// -d significa wque o terminal pode ser usado, depois do comando. O terminal nao fica preso na exibicao de informaçoes do container. Sem isso nao é possível utilzar o terminal enquanto o container estiver de pé.

//-rm significa que se o container existir, ele será removido para que outro possa ser criado.



### Agora faça o restore do banco:
```
docker exec -i mysql-container mysql -uroot -pprogramadorabordo < api/db/script.sql
```


Para entender melhor sobre cada comando utilizado, assita a videoaula ;)