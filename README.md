# Automação de Testes E2E — Hub de Leitura

Projeto de automação de testes end-to-end desenvolvido com Cypress para validar os principais fluxos da aplicação **Hub de Leitura**.

A suíte está integrada ao Jenkins para execução contínua dos testes, registro das execuções no Cypress Cloud e publicação de relatórios com Allure.

## Objetivo do projeto

Este projeto tem como objetivo demonstrar a aplicação de práticas de qualidade de software em uma suíte de testes automatizados, incluindo:

- automação de testes de interface;
- organização dos testes com fixtures, comandos customizados e Page Objects;
- geração de dados dinâmicos;
- execução automatizada com Jenkins;
- registro das execuções no Cypress Cloud;
- geração de evidências e relatórios com Allure.

## Cenários automatizados

A suíte valida os seguintes fluxos:

- cadastro de usuário;
- cadastro com dados gerados pelo Faker;
- cadastro utilizando comando customizado;
- cadastro utilizando Page Object;
- validação de campos obrigatórios;
- login de usuário;
- login administrativo;
- consulta ao catálogo;
- busca de livros;
- adição de livros à cesta;
- acesso aos detalhes de um livro;
- envio do formulário de contato;
- validações do formulário de contato.

## Tecnologias utilizadas

- JavaScript
- Node.js
- Cypress
- Faker
- Jenkins
- Cypress Cloud
- Allure Report
- Git e GitHub

## Estrutura do projeto

```text
.
├── cypress
│   ├── e2e
│   │   ├── cadastro.cy.js
│   │   ├── catalogo-busca.cy.js
│   │   ├── catalogo.cy.js
│   │   ├── contato.cy.js
│   │   └── login.cy.js
│   ├── fixtures
│   │   ├── livros.json
│   │   └── usuario.json
│   └── support
│       ├── commands.js
│       ├── e2e.js
│       └── pages
│           └── cadastro-page.js
├── cypress.config.js
├── Jenkinsfile
├── package.json
├── package-lock.json
└── README.md
