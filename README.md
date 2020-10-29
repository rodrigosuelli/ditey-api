# Ditey-Server

Esse é o back-end da aplicação [ditey-web](https://github.com/rodrigosuelli/ditey-web).

É uma API RESTful feita com [Node.js](https://nodejs.org/en/) + [Express](http://expressjs.com/) + [PostgreSLQ](https://www.postgresql.org/) que recebe os textos e as informações do usuário relacionadas à autenticação, e armazena/fornece esses dados para o cliente através de uma REST API.

O principal propósito dessa API é armazenar os textos do usuário, para que eles não sejam perdidos quando o usuário desligar a máquina ou acessar a aplicação [ditey-web](https://github.com/rodrigosuelli/ditey-web) de outro dispositivo.

## 📌 Índice

- [Instalação](#-instalação)
- [Rotas](#-rotas)
- [Como contribuir](#-como-contribuir)
- [Licença](#memo-licença)

## 🚀 Instalação

### Pré-requisitos

- Ter [**Git**](https://git-scm.com/) para clonar o projeto.
- Ter [**Node.js**](https://nodejs.org/en/) instalado.
- Ter um gerenciador de pacotes como [**NPM**](https://www.npmjs.com/get-npm) ou [**Yarn**](https://classic.yarnpkg.com/en/) para instalar as dependências do projeto.
- Ter o [**PostgreSLQ**](https://www.postgresql.org/) instalado.

### Clonando o Repositório

```bash
$ git clone https://github.com/rodrigosuelli/ditey-api.git

$ cd ditey-api
```

### Instalando as dependências

```bash
$ npm install

# ou

$ yarn
```

### Criando o banco de dados

Crie um banco de dados com o nome `diteyApi`. Para cria-lo você pode usar a cli do PostgreSQL `psql` no seu terminal e executar os comandos abaixo. Ou, se preferir, você pode usar um cliente PostgreSQL como [PgAdmin](https://www.pgadmin.org/) e replicar os comandos abaixo na interface gráfica. Ambos PgAdmin e psql que já vem instalado com o PostgreSQL por padrão.

Para usar a cli `psql` execute o comando abaixo no terminal:

```bash
# psql --username=[USERNAME], o username padrão é postgres
$ psql --username=postgres
```

Após inserir sua senha de usuário que será requisitada pelo `psql`, você pode executar os comandos abaixo para criar o banco de dados e as tabelas de texto e usuário:

```sql
# Cria o banco de dados diteyAPi
$ CREATE DATABASE diteyApi;

# Se conecta ao banco diteyApi
$ \c diteyApi

# Instala a extensão uuid-ossp no banco, necessária para gerar o UUID do usuário e criar a tabela users
$ CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

# Cria tabela de usuários
$ CREATE TABLE users(
  id uuid DEFAULT uuid_generate_v4(),
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255) NOT NULL UNIQUE,
  password VARCHAR(255) NOT NULL,
  PRIMARY KEY(id)
);

# Cria tabela de textos
$ CREATE TABLE texts(
  id SERIAL PRIMARY KEY,
  title VARCHAR(255),
  content TEXT,
  user_id UUID,
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

Ainda conectado ao banco diteyApi, execute o comando `\dt` para listar as tabelas do banco e ter certeza de que elas foram criadas:

Saia do psql com o comando `\q` .

### Executando o projeto

```bash
# Faz uma copia do arquivo .env.example e renomeia para .env
$ cp .env.example .env
```

No arquivo `.env` insira sua senha de usuário do postgres na variável PGPASSWORD e verifique se as outras variáveis de ambiente estão de acordo com o seu banco de dados.

Finalmente, com todas as dependências instaladas, o banco de dados criado e rodando na porta padrão(5432) e as variáveis de ambiente configuradas, você já pode executar o servidor no modo de desenvolvimento na porta(3333):

```bash
$ npm run dev

# ou

$ yarn dev
```

## 📃 Rotas

URL base: http://localhost:3333/api/

### Rotas de autenticação

- **post('/auth/login')** - Faz log in em uma conta
- **post('/auth/register')** - Cria uma conta
- **get('/auth/verify')** - Verifica se o usuário está autenticado
- **post('/auth/refresh-token')** - Retorna um novo accessToken e refreshToken para o usuário autenticado.

### Rotas privadas (autenticação necessária)

- **get('/texts')** - Lista todos os textos do usuário autenticado
- **post('/texts')** - Cria um texto com título e conteúdo vazios
- **put('/texts/:id')** - Modifica o título e conteúdo de um texto
- **delete('/texts/:id')** - Deleta um texto do banco de dados

## 🤔 Como contribuir

1. Faça um fork desse repositório
2. Faça um clone do seu fork (`git clone url-do-seu-fork && cd ditey-api`)
3. Crie uma branch com sua feature ou correção de bugs (`git checkout -b minha-branch`)
4. Faça commit das suas alterações (`git commit -m 'feature/bugfix: minhas alterações'`)
5. Faça push para a sua branch (`git push origin minha-branch`)
6. Abra sua Pull Request no repositório que você fez o fork

## 📝 Licença

Este projeto está licenciado sob a licença [MIT](./LICENSE).
