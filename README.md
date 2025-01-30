### SistemaContainer
#### Versão separada em repositórios

##### Após fazer o clone do repositório é necessário carregar os sub-módulos do __git__.
```shell script
git submodule update --init --recursive
```

##### E então construa o container através de:
```shell script
docker-compose up --build
```
