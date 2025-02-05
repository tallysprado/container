# SistemaContainer
# Configuração para rodar todos os projetos dentro de um único container.
## OBS: Neste container, faz-se referências à branch docker/card01 do repositório do projeto __sistema-backend__ e __sistema_frontend__.
#### Se for acessar os repositórios por outra referência fora este contexto, certifique-se utilizar a _branch_ correta.

### Siga os passos para criar o container:
- Faça o clone da aplicação
```shell script
git clone https://github.com/tallysprado/container.git
```
- Entre no repositório clonado
```shell script
cd container
```
- Faça o clone dos submódulos do projeto sistema-frontend e sistema-backend
```shell script
git submodule update --init --recursive
```

### E então construa o container através de:
```shell script
docker-compose up --build
```

## A aplicação pode ser acessada em <http://127.0.0.1:4200> utilizando as credenciais:
### Usuário: __universidade__
### Senha: __123__
#### Faça login na aplicação através da matrícula dos usuários criados. Utilize a senha __123__.
___
## Dentro de cada repositório há a documentação de como rodar a aplicação localmente em ambiente de desenvolvimento.
## Esta configuração usa o perfil de testes para construção do container. Em cada repositório há os passos de como executar o perfil de desenvolvimento (local).
## O __compose__ deste repositório irá criar quatro containers:
- Container Angular, utilizando a imagem do Node
- Container Quarkus, utilizando imagem Quarkus
- Container Keycloak, utilizando imagem Keycloak
- Container PSQL, Utilizando imagem PostgreSQL
## Para executar a aplicação localmente fora dos containers, deve-se garantir que este __compose__ não esteja em execução:
```shell script
docker-compose down -v
```
## e acessar a documentação de cada repositório separadamente.
#### O backend possui algumas rotas seguras (/protected), sendo necessário __token__ autenticado para fazer requisicões.
### Cenários de teste:
- Menu "Usuários -> Criar": (criar usuários para acessar a aplicação através da matrícula e senha 123)
    - Nome, Cargo e CPF são dados obrigatórios e exclusivos (validação backend para exception de constraint)
    - A depender do cargo, será criado uma entidade Aluno, Professor ou Coordenador e será associada 
    à entidade usuário.
    - Enter salva os dados.
    - Responsividade dos campos (4 colunas telas largas, 1 coluna disp. mobile)
    - Usuários criados nessa tela podem acessar a aplicação de acordo com a role definida em 'cargo'. Utilizar matrícula (ver tela de consulta) e senha "123".
- Menu "Usuários -> Consultar": 
    - Testar login na aplicação utilizando matrícula e senha "123"  
    - Expandir a linha mostra disciplinas matriculadas.
    - Responsividade mobile para filtros e tabela (é exibido cards com as colunas no lugar das linhas na visualização
    em dispositivos móveis).
    - A edição do usuário reaproveita a tela de criação.
    - O filtro da matrícula é um inner join a depender do tipo se é Aluno, Professor ou Coordenador
    - Possibilita excluir o usuário da base e do Keycloak (futuramente deve-se habilitar este botão para roles específicas apenas)
- Menu "Matrícula - Aluno": (relacionar disciplina x usuário)
    - É listado apenas entidade Aluno.
    - Esta tela apresenta itens (disciplinas) inseridos na base de dados durante a inicializacão do Quarkus, no import.sql.
    - Nome é auto-complete com os alunos cadastrados, ao selecionar deve aparecer a lista de disciplinas disponíveis
    e as matriculadas. Selecionar e salvar vai atualizar os dados da linha expandível da tela de filtros.
    - Campo de filtro "matrícula" ainda está em construção :construction:
    - Responsividade da tabela, para dispositivos mobile, ainda em construção :construction:
- Menu da Topbar abrirá "por cima" do conteúdo em dispositivos mobile, e "empurrará" o conteúdo telas maiores.
- Tela de Login foi customizada direto no .ftl e não exibe logo marca em dispositivos móveis, apenas em telas maiores.
- As rotas estão protegidas pelo AuthGuard do Keycloak. Existe duas _roles_ importadas na configuração inicial, 
ROLE_COORDENADOR e ROLE_ALUNO. Deve-se considerar no teste, atribuir diferentes papéis ao usuário e verificar a página
de acesso negado ao tentar acessar uma rota não autorizada. O papel de coordenador tem mais funcionalidades implementadas,
porém vale testar o acesso à rota <http://localhost:4200/periodo/create> sem ter o papel 'aluno' atribuído e verificar a página de _forbidden access_.
- Todas as rotas da aplicacão (Frontend e backend) estão seguras e é necessário TOKEN do usuário logado, para enviar requisicões ao backend ou acessar telas do frontend de acordo com papel cadastrado.
###### Dúvidas entre em contato no Whatsapp ou E-mail:
- (88) 9 9651 - 0001
- tallys.prado@gmail.com

#### Rotas do Backend:
![Rotas backend](https://i.ibb.co/Q2BN8sy/rotas.png)
