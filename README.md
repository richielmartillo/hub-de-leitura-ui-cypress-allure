# Hub de Leitura - Testes UI

Projeto de automação de testes de interface desenvolvido no **Módulo 18** do curso de **Engenharia de Qualidade de Software - EBAC**.

Este repositório tem como objetivo automatizar cenários da aplicação **Hub de Leitura** utilizando **Cypress**, com integração ao **Jenkins** para execução em pipeline, ao **Cypress Cloud** para acompanhamento das execuções e ao **Allure Report** para geração de relatórios.

## Objetivo do projeto

Automatizar os principais fluxos da aplicação **Hub de Leitura**, aplicando conceitos de testes end-to-end e integração contínua vistos durante o módulo.

## Tecnologias utilizadas

- JavaScript
- Node.js
- Cypress
- Jenkins
- Cypress Cloud
- Allure Report
- Git
- GitHub

## Funcionalidades testadas

Os testes automatizados cobrem cenários como:

- Cadastro de usuário
- Login
- Contato
- Catálogo
- Busca no catálogo

## Estrutura do projeto

```bash
.
├── cypress
│   ├── e2e
│   │   ├── cadastro.cy.js
│   │   ├── catalogo-busca.cy.js
│   │   ├── catalogo.cy.js
│   │   ├── contato.cy.js
│   │   └── login.cy.js
│   ├── fixtures
│   ├── screenshots
│   └── support
│       ├── commands.js
│       ├── e2e.js
│       └── pages
├── allure-results
├── cypress.config.js
├── package.json
├── package-lock.json
├── Jenkinsfile
├── Jenkinsfile-win
└── README.md
```

## Pré-requisitos

Antes de executar o projeto, é necessário ter instalado:

- Node.js
- npm
- Git

## Instalação

Clone o repositório:

```bash
git clone https://github.com/richielmartillo/hub-de-leitura-teste-ui.git
```

Acesse a pasta do projeto:

```bash
cd hub-de-leitura-teste-ui
```

Instale as dependências:

```bash
npm install
```

## Como executar os testes

### Modo interativo

```bash
npx cypress open
```

### Modo headless

```bash
npx cypress run
```

## Execução com Cypress Cloud

O projeto está configurado para integração com o **Cypress Cloud**, permitindo gravar e acompanhar a execução dos testes.

Exemplo de execução:

```bash
npx cypress run --record
```

> Para esse tipo de execução, é necessário configurar corretamente o `projectId` e a `record key` no ambiente.

## Integração com Jenkins

Este projeto possui configuração para execução automatizada no Jenkins.

Durante a pipeline, são realizadas etapas como:

- clonagem do repositório da aplicação **hub-de-leitura-integrado2**
- instalação das dependências da aplicação
- inicialização da aplicação localmente
- clonagem do repositório de testes **hub-de-leitura-teste-ui**
- instalação das dependências do projeto de testes
- espera da aplicação estar disponível em `http://localhost:3000`
- execução dos testes no **Cypress Cloud**
- arquivamento de evidências, como screenshots e vídeos
- geração de relatório com **Allure**

## Observação importante

Os testes automatizados dependem da aplicação **Hub de Leitura** em execução.

Por isso, no pipeline Jenkins, o repositório **hub-de-leitura-integrado2** é clonado e iniciado antes da execução dos testes, preparando automaticamente o ambiente necessário para os testes UI.

## Relatórios

Os resultados dos testes podem ser armazenados na pasta:

```bash
allure-results
```

Além disso, o pipeline também pode arquivar evidências de execução, como:

- screenshots
- vídeos

## Aprendizados aplicados no módulo

Durante o desenvolvimento deste projeto, foram praticados conceitos como:

- automação de testes UI com Cypress
- organização da estrutura de testes
- integração contínua com Jenkins
- execução de testes em pipeline
- preparação automática do ambiente de execução
- integração com Cypress Cloud
- geração de evidências e relatórios
- versionamento com Git e GitHub

## Autor

Desenvolvido por **Richard Marlon Balestrim** como atividade prática do **Módulo 18** do curso de **Engenhario de Qualidade de Software - EBAC**.
