nODE_aMBIENTE
-
Criando um Backend completo em [NodeJs com ES6](https://medium.com/@flavio.ever/criando-um-backend-completo-em-nodejs-com-es6-e-configurando-o-ambiente-parte-1-8c271a010c9c) e configurando o Ambiente - Parte 1

***Requisitos***

- Yarn — Para gerenciamento dos pacotes;
- NodeJs — Ambiente da aplicação;
- Docker — Para criar uma camada de abstração dos nossos recursos;
- Insomnia — Para trabalhar com o protocolo HTTP;
- Postbird — Para poder consultar nossa base de dados relacional;
- VSCode — P

***Docker***

- Instalar Docker;

        sudo snap install docker 
        docker run -name database -e POSTGRES_PASSWORD=docker -p 5432:5432 -d postgre

- Banco de dados; 

Banco | Senha | Porta | Versão PostgreSQL 
---|---|---|---|
database |  docker | 5432 | 11

- Comandos úteis:

        docker -v |versao 20.10.8
        docker ps |Lista os serviços abertos
        docker stop database |Pausa serviço database
        docker ps -a |Histórico
        docker logs database |Lista logs do service

***cd*** [node_ambiente]()

        yarn init -y |cria package.json

***mkdir*** node_ambiente / [src]()

***touch*** node_ambiente / src / [app.js]() , [routes.js](), [server.js]()

- cd node_ambiente

        yarn add express  |possibilita manipular dados e criar rotas 
        yarn add nodemon sucrase -D |refresh aplicação automaticamente.

***app.js***

        /**
        Importo a biblioteca express;
        Importo as rotas externas e seus demais verbos (POST, GET, PUT, DELETE);
        **/
        import express from 'express';
        import routes from './routes';

        class App {
        // O construtor irá invocar minha instância e os middlewares;
        constructor() {
            this.server = express();
            this.middleware();
            this.routes();
        }

        middleware() {
            /** 
            Em this.server.use(express.json()), estou dizendo à aplicação
            para entender quando meu corpo de requisição for um JSON; 
            **/
            this.server.use(express.json());
        }

        routes() {
            /**
            Em this.server.use(routes), estou dizendo à aplicação
            para aplicar minhas rotas;
            **/
            this.server.use(routes);
        }
        }

***server.js***

        import App from './App';
        // Inicio o servidor na porta 3333;
        App.listen(3333);

***routes.js***

        // Importo apenas Router do pacote Express;
        import { Router } from "express";
        // Crio uma instância do método Router;
        const router = new Router();

        /**
        Nosso primeiro verbo é o GET;
        Em req (require) será tudo aquilo que se envia pra rota;
        Em res (response) será tudo aquilo que se retorna ao cliente;
        **/
        router.get('/', (req, res) => {
            return res.status(200).json({ message: 'Hello, Medium! ;-)'});
        });

        export default router;

***package.json***

        "scripts": {
        "dev": "nodemon src/server",
        "dev:debug": "nodemon --inspect src/server"
        },

***mkdir*** node_ambiente / [nodemon.json]()

        {
        "execMap": {
            "js": "node -r sucrase/register"
        }
        }

***Start servidor***

        yarn dev