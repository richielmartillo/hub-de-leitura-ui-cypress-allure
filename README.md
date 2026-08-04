# Automação de Testes E2E — Hub de Leitura

## Sobre o projeto

Este projeto automatiza fluxos de interface da aplicação **Hub de Leitura** com Cypress. A suíte exercita jornadas de usuário em navegador, gera evidências de execução e pode ser executada localmente ou em uma pipeline Jenkins no Windows.

O repositório integra Cypress Cloud para o acompanhamento de execuções gravadas e Allure Report para publicação de resultados no Jenkins.

## Objetivos

- Validar os principais fluxos de interface da aplicação em uma perspectiva end-to-end.
- Organizar os testes com comandos reutilizáveis, fixtures e Page Object.
- Executar a suíte de forma reproduzível com Node.js e npm.
- Gerar evidências e relatórios para apoiar a análise de falhas em integração contínua.

## Cenários automatizados

Os cenários existentes na suíte abrangem:

- Cadastro de usuário com dados estáticos, dados dinâmicos gerados pelo Faker, comando customizado e Page Object.
- Validação de cadastro sem preenchimento do nome.
- Login com credenciais diretas, comando customizado e fixture de usuário, incluindo conta administrativa.
- Consulta ao catálogo, adição de livros à cesta e acesso ao detalhe de um livro.
- Busca de livros por texto e por dados carregados de fixture.
- Envio do formulário de contato e validações de campos obrigatórios.

> Observação: há um `it.only` em `cypress/e2e/catalogo-busca.cy.js`. Enquanto ele permanecer, o Cypress limitará a execução desse spec ao cenário marcado. A remoção deve ser considerada antes de uma execução completa da suíte.

## Tecnologias utilizadas

- JavaScript
- Node.js e npm
- Cypress
- Faker (`@faker-js/faker`)
- Jenkins
- Cypress Cloud
- Allure Report (`allure-cypress`)
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
│       └── pages/cadastro-page.js
├── allure-results/                 # Gerado durante a execução
├── cypress.config.js
├── Jenkinsfile
├── Jenkinsfile-win
├── package.json
├── package-lock.json
└── README.md
```

## Pré-requisitos

- Node.js em versão compatível com as dependências do projeto.
- npm.
- Git.
- Google Chrome para a execução pelo script `npm test`.
- Jenkins, somente para execução pela pipeline.
- Plugin Allure configurado no Jenkins, quando a publicação do relatório for necessária.

## Instalação

Clone o repositório e instale as dependências bloqueadas no lockfile:

```bash
git clone https://github.com/richielmartillo/hub-de-leitura-ui-cypress-allure.git
cd hub-de-leitura-ui-cypress-allure
npm ci
```

## Execução dos testes

A aplicação Hub de Leitura deve estar disponível em `http://localhost:3000` antes da execução local.

### Modo interativo

```bash
npx cypress open
```

### Modo headless

```bash
npx cypress run
```

### Script do projeto

O script abaixo executa o Cypress em Google Chrome:

```bash
npm test
```

## Jenkins

A configuração destinada ao Jenkins em Windows está no arquivo `Jenkinsfile-win`.

No job Jenkins, utilize:

- **Branch:** `main`
- **Script Path:** `Jenkinsfile-win`

A pipeline Windows executa as seguintes etapas:

1. Clona a aplicação Hub de Leitura.
2. Instala as dependências da aplicação com `npm ci`.
3. Inicia a aplicação em segundo plano.
4. Clona o projeto de testes e instala suas dependências com `npm ci`.
5. Aguarda `http://localhost:3000` com `wait-on`.
6. Executa o Cypress pelo script `npm test`.
7. Arquiva screenshots e vídeos, e publica os resultados do Allure.
8. Encerra os processos Node ao final da execução.

## Cypress Cloud

O projeto possui `projectId` configurado no Cypress e disponibiliza o script abaixo para gravação no Cypress Cloud:

```bash
npm run cy:report
```

Esse comando requer uma Record Key fornecida pelo ambiente. Em uma execução Jenkins que grave resultados no Cloud, a chave deve ser armazenada como credencial protegida com o identificador `cypress-record-key`; ela não deve ser registrada no repositório, em logs ou na documentação.

O `Jenkinsfile-win` atual executa `npm test` e não chama o comando com `--record`. Para publicar no Cypress Cloud pela pipeline Windows, essa etapa deve ser incluída de forma controlada no pipeline.

## Allure Report

A integração do `allure-cypress` está configurada em `cypress.config.js`. Os resultados são gerados em:

```text
allure-results/
```

No Jenkins, o passo `allure` lê esse diretório e publica o relatório no job, desde que o plugin Allure esteja disponível na instância.

## Artefatos

O Cypress pode gerar screenshots e vídeos durante a execução, e o Allure gera resultados em `allure-results/`. Esses artefatos são temporários, servem como evidência e não devem ser versionados. As regras do `.gitignore` mantêm esses arquivos fora do controle de versão.

## Resultados esperados

Em uma execução bem-sucedida, espera-se:

- Resultado da suíte exibido no terminal, no Cypress ou no console do Jenkins.
- Evidências de falhas disponíveis nos artefatos do job quando aplicável.
- Resultados processados pelo Allure no Jenkins.
- Execução registrada no Cypress Cloud quando o comando com `--record` for utilizado com uma credencial válida.

## Boas práticas aplicadas

- Page Object para o fluxo de cadastro.
- Comandos customizados para login e preenchimento de cadastro.
- Fixtures para dados de usuário e catálogo.
- Dados dinâmicos gerados com Faker.
- Separação entre specs, suporte, fixtures e Page Objects.
- Integração contínua com Jenkins, geração de evidências e relatórios Allure.

## Melhorias futuras

- Remover o `it.only` para garantir a execução integral dos specs em CI.
- Incluir a etapa de gravação no Cypress Cloud no `Jenkinsfile-win`, usando a credencial Jenkins protegida.
- Adicionar validações de qualidade e linting ao pipeline.
- Ampliar os Page Objects para outros fluxos da aplicação.

## Autor

Richard Marlon Balestrim

- GitHub: https://github.com/richielmartillo
- LinkedIn: https://www.linkedin.com/in/richard-marlon-balestrim

## Créditos

Projeto desenvolvido durante o curso de Engenharia de Qualidade de Software da EBAC e posteriormente adaptado, organizado e documentado para o portfólio pessoal.
