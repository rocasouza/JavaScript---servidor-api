# Manual de Desenvolvimento - EasyCar API

## 1. Introdução

Bem-vindo(a) à equipe de desenvolvimento da EasyCar API!

Este manual serve como um guia central para todos os desenvolvedores que trabalham no projeto. Ele descreve nossas práticas de codificação, ferramentas, processos e a arquitetura geral do sistema. Seguir este manual é crucial para manter a qualidade, consistência e colaboração eficiente em nossa equipe.

### 1.1. Objetivo do Projeto

A API EasyCar gerencia o processo de solicitação e atribuição de caronas entre passageiros e motoristas. Ela permite que usuários solicitem caronas, motoristas aceitem e gerenciem essas caronas, e que o sistema rastreie o status de cada corrida.

Para uma descrição detalhada dos endpoints e regras de negócio da API, consulte o [README.md](./README.md).

### 1.2. Público-Alvo

Este manual é destinado a todos os engenheiros de software, incluindo desenvolvedores backend, frontend (se aplicável à API), e QAs que interagem com o projeto EasyCar API.

## 2. Primeiros Passos

Esta seção orienta você na configuração do seu ambiente de desenvolvimento local.

### 2.1. Pré-requisitos

Antes de começar, certifique-se de ter instalado:

*   **Node.js**: Versão LTS recomendada (ex: v18.x ou superior). [Download Node.js](https://nodejs.org/)
*   **NPM** (geralmente vem com o Node.js) ou **Yarn**: (Definir qual gerenciador de pacotes é o padrão do projeto).
*   **Git**: Para controle de versão. [Download Git](https://git-scm.com/)
*   **Docker** (Opcional, mas recomendado): Se o projeto utilizar containers para banco de dados ou outros serviços. [Download Docker](https://www.docker.com/products/docker-desktop)
*   **Um Editor de Código**: VS Code (recomendado com extensões como ESLint, Prettier), WebStorm, etc.
*   **Cliente API REST**: Postman, Insomnia, ou similar para testar os endpoints.

### 2.2. Configurando o Ambiente

1.  **Clonar o Repositório:**
    ```bash
    git clone https://github.com/rocasouza/JavaScript---servidor-api.git
    cd JavaScript---servidor-api
    ```

2.  **Instalar Dependências:**
    (Se usar NPM)
    ```bash
    npm install
    ```
    (Se usar Yarn)
    ```bash
    yarn install
    ```

3.  **Configuração de Variáveis de Ambiente:**
    *   Copie o arquivo de exemplo `.env.example` para `.env`:
        ```bash
        cp .env.example .env
        ```
    *   Edite o arquivo `.env` com as configurações necessárias para o seu ambiente local (ex: credenciais de banco de dados, chaves de API, porta do servidor).
        ```
        # Exemplo de .env
        PORT=3000
        DATABASE_URL="postgresql://user:password@localhost:5432/easycar_dev"
        # Outras variáveis...
        ```
    *   **Importante:** O arquivo `.env` não deve ser versionado no Git. Ele já está incluído no `.gitignore`.

4.  **Configurar o Banco de Dados:**
    *   (Descrever os passos para configurar o banco de dados local. Ex: Usar Docker Compose para subir uma instância PostgreSQL, rodar migrações, popular com dados iniciais).
    *   Exemplo com Docker (se aplicável):
        ```bash
        docker-compose up -d db # Supondo um serviço 'db' no docker-compose.yml
        ```
    *   Rodar Migrações (se usar um ORM/ferramenta de migração):
        ```bash
        npm run db:migrate # Exemplo de comando
        ```

5.  **Rodar o Projeto Localmente:**
    ```bash
    npm run dev # Para modo de desenvolvimento com hot-reload (se configurado)
    ```
    ou
    ```bash
    npm start # Para iniciar o servidor
    ```
    A API deverá estar acessível em `http://localhost:PORTA_CONFIGURADA` (ex: `http://localhost:3000`).

## 3. Arquitetura do Projeto

### 3.1. Tecnologias Principais

*   **Backend:** Node.js
*   **Framework:** (Ex: Express.js, NestJS, Fastify - *Especificar qual é usado*)
*   **Linguagem:** JavaScript (ES6+) ou TypeScript (*Especificar*)
*   **Banco de Dados:** (Ex: PostgreSQL, MongoDB - *Especificar qual é usado e por quê*)
*   **ORM/Query Builder:** (Ex: Sequelize, Prisma, Knex.js - *Especificar se usado*)
*   **Testes:** (Ex: Jest, Mocha, Chai - *Especificar*)
*   **Autenticação/Autorização:** (Ex: JWT, OAuth2 - *Especificar*)

### 3.2. Estrutura de Diretórios (Exemplo)

Uma estrutura de diretórios organizada é fundamental. Abaixo um exemplo comum, que deve ser adaptado à realidade do projeto:

📁 Projeto \
├─ 📁 .git \
├─ 📁 .github        # Workflows de CI/CD, templates de PR/Issue \
├─ 📁 config         # Arquivos de configuração (banco de dados, etc.) \
├─ 📁 dist           # Arquivos compilados (TypeScript, bundler, etc.) \
├─ 📁 docs           # Documentação adicional  \
├─ 📁 node_modules  \
├─ 📁 src \
│   ├── 📁 api                     # Lógica relacionada à API \ 
|   |   ├── 📁 routes              # Definição das rotas da API \
│   │   ├── 📁 controllers         # Lógica de manipulação das requisições \
│   │   ├── 📁 middlewares         # Middlewares da aplicação \
│   │   └── 📁 validators          # Esquemas de validação \
│   ├── 📁 core                     # Lógica de negócio principal \
│   │   ├── 📁 services            # Regras de negócio \
│   │   ├── 📁 models              # Modelos de dados \
│   │   └── 📁 enums               # Enumerações \
│   ├── 📁 infra                    # Infraestrutura (BD, clientes externos) \
│   │   ├── 📁 database            # Configurações, migrações, seeds \
│   │   └── 📁 clients             # Clientes para APIs externas \
│   ├── 📁 shared                   # Utilitários compartilhados \
│   │   ├── 📁 utils \
│   │   └── 📁 errors              # Classes de erro customizadas \
│   └── 📄 app.js                  # Ponto de entrada da aplicação \
├── 📁 tests \
│   ├── 📁 unit \
│   ├── 📁 integration \
│   └── 📁 e2e \
├── 📄 .env.example                # Exemplo de variáveis de ambiente \
├── 📄 .eslintignore \
├── 📄 .eslintrc.js                # Configuração do ESLint \
├── 📄 .gitignore \
├── 📄 .prettierrc.js              # Configuração do Prettier \
├── 📄 docker-compose.yml         # Arquivo de orquestração Docker \
├── 📄 Dockerfile \
├── 📄 jest.config.js             # Configuração do Jest \
├── 📄 package.json \
├── 📄 package-lock.json \
└── 📄 README.md 

- Projeto
  - .git
  - .github  # Workflows de CI/CD, templates de PR/Issue
  - config   # Arquivos de configuração (banco de dados, etc.)
  - dist     # Arquivos compilados (TypeScript, bundler, etc.)
  - docs     # Documentação adicional
  - node_modules
  - src
    - api  # Lógica relacionada à API
      - routes        # Definição das rotas da API
      - controllers   # Lógica de manipulação das requisições
      - middlewares   # Middlewares da aplicação
      - validators    # Esquemas de validação
    - core  # Lógica de negócio principal
      - services       # Regras de negócio
      - models         # Modelos de dados
      - enums          # Enumerações
    - infra  # Infraestrutura
      - database       # Configurações, migrações, seeds
      - clients        # APIs externas
    - shared  # Utilitários compartilhados
      - utils
      - errors         # Erros customizados
    - app.js           # Ponto de entrada
  - tests
    - unit
    - integration
    - e2e
  - .env.example       # Exemplo de variáveis de ambiente
  - .eslintignore
  - .eslintrc.js       # Configuração do ESLint
  - .gitignore
  - .prettierrc.js     # Configuração do Prettier
  - docker-compose.yml
  - Dockerfile
  - jest.config.js
  - package.json
  - package-lock.json
  - README.md
  - Manual.md

*Adapte esta estrutura para refletir a organização real do projeto EasyCar API.*

## 4. Padrões de Código e Convenções

### 4.1. Linguagem

*   O projeto utiliza **JavaScript (ES6+)** ou **TypeScript** (*Definir qual*).
*   Siga as funcionalidades mais recentes e estáveis da linguagem.

### 4.2. Estilo de Código (Linting e Formatting)

*   **ESLint**: Utilizamos ESLint para análise estática de código e para garantir um padrão de codificação. As regras estão definidas no arquivo `.eslintrc.js`.
    *   Rode o linter com: `npm run lint`
    *   Para corrigir problemas automaticamente: `npm run lint:fix`
*   **Prettier** (Opcional, mas recomendado): Para formatação automática de código. Se usado, as configurações estarão em `.prettierrc.js`.
    *   Integre com seu editor para formatar ao salvar.
    *   Rode o formatador: `npm run format` (se configurado)

### 4.3. Convenções de Nomenclatura

*   **Variáveis e Funções:** `camelCase` (ex: `minhaVariavel`, `calcularTotal`)
*   **Classes e Interfaces (TypeScript):** `PascalCase` (ex: `UserService`, `IRide`)
*   **Arquivos:** `kebab-case.js` (ex: `user-service.js`) ou `PascalCase.ts` para classes/interfaces em TypeScript. (*Definir um padrão consistente*).
*   **Constantes:** `UPPER_SNAKE_CASE` (ex: `MAX_USERS`, `API_TIMEOUT`)
*   **Endpoints de API:** `kebab-case` e no plural para coleções (ex: `/pending-rides`, `/user-profiles`).

### 4.4. Comentários

*   Comente código complexo, decisões de design não óbvias e lógica de negócio crítica.
*   Use JSDoc para documentar funções públicas, seus parâmetros e retornos, especialmente em módulos reutilizáveis e na camada de serviço.
    ```javascript
    /**
     * Calcula o preço final de uma corrida.
     * @param {string} rideId - O ID da corrida.
     * @param {object} options - Opções adicionais para cálculo.
     * @returns {Promise<number>} O preço final da corrida.
     * @throws {Error} Se a corrida não for encontrada.
     */
    async function calculateRidePrice(rideId, options) {
      // ...lógica
    }
    ```

### 4.5. Módulos

*   Utilize módulos ES6 (`import`/`export`).
*   Evite importações circulares.

## 5. Controle de Versão com Git

### 5.1. Branching Model

Adotamos um fluxo baseado no **GitFlow simplificado** ou **GitHub Flow**:

*   **`main`**: Contém o código de produção estável. Somente merges de `develop` (ou branches de release) são permitidos, geralmente após um processo de QA e aprovação.
*   **`develop`**: Branch principal de desenvolvimento. Novas features e fixes são integrados aqui antes de irem para `main`. Deve estar sempre em um estado "buildável".
*   **Branches de Feature (`feature/nome-da-feature`):**
    *   Criadas a partir de `develop`.
    *   Ex: `feature/user-authentication`, `feature/list-available-drivers`.
    *   Mergeadas de volta em `develop` via Pull Request.
*   **Branches de Correção (`fix/nome-da-correcao`):**
    *   Criadas a partir de `develop` para correções não urgentes.
    *   Ex: `fix/ride-status-update-bug`.
    *   Mergeadas de volta em `develop` via Pull Request.
*   **Branches de Hotfix (`hotfix/descricao-do-hotfix`):**
    *   Criadas a partir de `main` para correções críticas em produção.
    *   Mergeadas de volta em `main` E `develop`.

### 5.2. Mensagens de Commit

Siga o padrão **Conventional Commits** (https://www.conventionalcommits.org/). Isso ajuda na automação de changelogs e na clareza do histórico.

Formato: `<tipo>(<escopo_opcional>): <descrição>`

Exemplos:

*   `feat: adiciona endpoint para cadastro de motoristas`
*   `fix(rides): corrige cálculo de distância para corridas`
*   `docs: atualiza documentação da API de usuários`
*   `style: formata código com Prettier`
*   `refactor: melhora performance da consulta de corridas pendentes`
*   `test: adiciona testes unitários para o serviço de pagamento`
*   `chore: atualiza dependências do projeto`

### 5.3. Pull Requests (PRs)

*   Todo código novo ou alterado deve ser submetido através de um Pull Request (PR) para o branch `develop` (ou `main` para hotfixes).
*   **Criação do PR:**
    *   Use um título claro e descritivo.
    *   Na descrição, explique o que foi feito, por que foi feito, e como testar. Se relacionado a uma issue, mencione-a (ex: "Closes #123").
*   **Revisão de Código (Code Review):**
    *   Pelo menos **um** outro desenvolvedor deve revisar e aprovar o PR.
    *   Foco da revisão: lógica, clareza, performance, segurança, cobertura de testes, aderência aos padrões.
    *   Seja construtivo nos comentários.
*   **Verificações Automáticas (CI):**
    *   O PR deve passar em todas as verificações de Integração Contínua (CI), como linting, testes automatizados e build.
*   **Merge:**
    *   Após aprovação e CI verde, o PR pode ser mergeado. Prefira "Squash and merge" ou "Rebase and merge" para manter o histórico de `develop` limpo (*Definir o método padrão*).
    *   Delete o branch após o merge.

## 6. Testes

A qualidade é responsabilidade de todos. Testes automatizados são essenciais.

### 6.1. Filosofia de Testes

*   **Pirâmide de Testes:** Foco em testes unitários, complementados por testes de integração e alguns testes E2E.
*   Escreva testes para todas as novas funcionalidades e correções de bugs.
*   Testes devem ser rápidos, independentes e repetíveis.

### 6.2. Tipos de Testes e Ferramentas

*   **Testes Unitários:**
    *   Testam a menor unidade de código (funções, métodos) de forma isolada.
    *   Ferramenta: (Ex: **Jest**, Mocha, Jasmine - *Especificar*)
    *   Localização: `tests/unit`
*   **Testes de Integração:**
    *   Testam a interação entre diferentes módulos ou serviços (ex: controller + service + model, interação com banco de dados mockado ou real).
    *   Ferramenta: (Ex: **Jest**, Supertest - *Especificar*)
    *   Localização: `tests/integration`
*   **Testes End-to-End (E2E):**
    *   Testam o fluxo completo da aplicação sob a perspectiva do usuário, interagindo com a API como um cliente real.
    *   Ferramenta: (Ex: **Jest + Supertest**, Cypress, Playwright - *Especificar*)
    *   Localização: `tests/e2e`

### 6.3. Como Rodar os Testes

*   Rodar todos os testes:
    ```bash
    npm test
    ```
*   Rodar testes com cobertura de código:
    ```bash
    npm run test:coverage
    ```
*   Rodar um arquivo de teste específico (exemplo com Jest):
    ```bash
    npm test -- tests/unit/user-service.test.js
    ```

### 6.4. Cobertura de Código

*   Monitore a cobertura de código.
*   Objetivo: Manter uma cobertura de (Ex: > 80%) para novas funcionalidades.
*   Relatórios de cobertura são gerados em (Ex: `/coverage`).

## 7. Documentação da API

*   A documentação principal dos endpoints da API, incluindo exemplos de requisição/resposta e regras de negócio, está no arquivo README.md.
*   **É responsabilidade de cada desenvolvedor manter esta documentação atualizada** ao adicionar ou modificar funcionalidades da API.
*   Para documentação mais complexa ou diagramas, utilize a pasta `/docs`.

## 8. Gerenciamento de Dependências

*   Utilizamos **NPM** (ou **Yarn** - *especificar*) para gerenciar as dependências do projeto.
*   **Adicionar uma nova dependência:**
    ```bash
    npm install <nome-do-pacote>
    # ou para dependências de desenvolvimento:
    npm install --save-dev <nome-do-pacote>
    ```
*   **Atualizar dependências:**
    ```bash
    npm update <nome-do-pacote> # Para um pacote específico
    npm outdated # Para verificar pacotes desatualizados
    # Avalie cuidadosamente antes de atualizar todas as dependências de uma vez.
    ```
*   **Remover uma dependência:**
    ```bash
    npm uninstall <nome-do-pacote>
    ```
*   Sempre verifique as licenças de novas dependências para garantir compatibilidade.
*   O arquivo `package-lock.json` (ou `yarn.lock`) **DEVE** ser versionado.

## 9. Tratamento de Erros

*   Utilize códigos de status HTTP apropriados para indicar o resultado das requisições.
*   Para erros do cliente (4xx), forneça mensagens claras no corpo da resposta JSON. Exemplo:
    ```json
    {
      "error": {
        "code": "VALIDATION_ERROR",
        "message": "O campo 'email' é obrigatório.",
        "details": [
          { "field": "email", "issue": "required" }
        ]
      }
    }
    ```
*   Para erros do servidor (5xx), evite expor detalhes internos. Logue o erro detalhadamente no servidor.
*   Crie classes de erro customizadas (ex: `NotFoundError`, `ValidationError`, `UnauthorizedError`) que herdem de `Error` para um tratamento mais granular.
*   Utilize um middleware global de tratamento de erros para capturar exceções não tratadas e formatar a resposta de erro.

## 10. Segurança

*   **NUNCA** versione dados sensíveis (senhas, chaves de API, tokens) no código. Utilize variáveis de ambiente (`.env`).
*   **Validação de Entrada:** Valide todos os dados recebidos de clientes (body, query params, path params). Use bibliotecas como Joi, Zod, ou express-validator.
*   **Autenticação e Autorização:** Implemente mecanismos robustos (ex: JWT). Proteja rotas sensíveis.
*   **Prevenção contra Ataques Comuns:**
    *   SQL Injection (se usar SQL direto, ORMs geralmente ajudam).
    *   XSS (menos comum em APIs JSON, mas cuidado se a API servir HTML).
    *   CSRF (geralmente não é um problema para APIs stateless com tokens, mas entenda o contexto).
*   **HTTPS:** Use HTTPS em produção.
*   **Rate Limiting:** Implemente para prevenir abuso.
*   **Logging de Segurança:** Registre tentativas de login falhas, acessos não autorizados, etc.
*   Mantenha as dependências atualizadas para corrigir vulnerabilidades conhecidas.

## 11. Banco de Dados

*   **Banco Utilizado:** (Ex: PostgreSQL 14)
*   **ORM/Query Builder:** (Ex: Prisma, Sequelize, Knex.js - *Especificar*)
*   **Migrações:**
    *   Utilize a ferramenta de migração do ORM (Ex: `npx prisma migrate dev`, `npx sequelize-cli db:migrate`).
    *   Toda alteração no schema do banco DEVE ser feita através de um arquivo de migração.
    *   As migrações são versionadas no Git.
*   **Seeds (Dados Iniciais):**
    *   Crie scripts para popular o banco com dados essenciais para desenvolvimento e testes (Ex: `npx prisma db seed`, `npx sequelize-cli db:seed:all`).
*   **Conexões:** As configurações de conexão estão no arquivo `.env`.

## 12. Processo de Deploy (Implantação)

*(Esta seção deve ser preenchida com os detalhes específicos do processo de deploy da EasyCar API).*

*   **Ambientes:**
    *   `development` (local)
    *   `staging` (ambiente de testes e homologação, similar à produção)
    *   `production` (ambiente real dos usuários)
*   **Ferramentas de CI/CD:** (Ex: GitHub Actions, Jenkins, GitLab CI - *Especificar*)
    *   Descrever os pipelines (build, test, deploy).
*   **Plataforma de Hospedagem:** (Ex: AWS, Google Cloud, Heroku, Vercel - *Especificar*)
*   **Estratégia de Deploy:** (Ex: Blue/Green, Canary - *Especificar*)
*   **Rollbacks:** Como proceder em caso de falha no deploy.

## 13. Comunicação e Ferramentas da Equipe

*(Esta seção deve ser preenchida com as ferramentas e rituais da equipe).*

*   **Comunicação:**
    *   Canal principal: (Ex: Slack #easycar-api-dev, Microsoft Teams)
    *   Reuniões:
        *   Daily Stand-ups (Horário, objetivo)
        *   Sprint Planning (se usar Scrum)
        *   Sprint Review/Demo (se usar Scrum)
        *   Retrospectivas (se usar Scrum)
*   **Gerenciamento de Tarefas:**
    *   Ferramenta: (Ex: Jira, Trello, Asana, GitHub Issues - *Especificar*)
    *   Fluxo de trabalho das tarefas (To Do, In Progress, In Review, Done).
*   **Documentação Interna:** (Ex: Confluence, Notion, Wiki do GitHub - *Especificar*)

## 14. Como Contribuir

1.  Certifique-se de que seu ambiente local está configurado (Seção 2).
2.  Pegue uma tarefa do backlog (Jira, Trello, etc.) ou crie uma issue para um bug/feature.
3.  Crie um novo branch a partir de `develop` seguindo o padrão de nomenclatura (Seção 5.1).
    ```bash
    git checkout develop
    git pull origin develop
    git checkout -b feature/minha-nova-feature
    ```
4.  Implemente a funcionalidade ou correção.
5.  Escreva testes unitários e/ou de integração para cobrir suas alterações.
6.  Certifique-se de que todos os testes passam (`npm test`).
7.  Certifique-se de que o código está de acordo com os padrões de linting (`npm run lint`).
8.  Faça commits seguindo o padrão de mensagens (Seção 5.2).
9.  Envie seu branch para o repositório remoto:
    ```bash
    git push origin feature/minha-nova-feature
    ```
10. Abra um Pull Request para `develop` no GitHub (ou plataforma similar).
11. Aguarde a revisão de código e responda aos feedbacks.
12. Após aprovação e CI verde, seu PR será mergeado.

---

*Este manual é um documento vivo e deve ser atualizado conforme o projeto e a equipe evoluem. Sinta-se à vontade para sugerir melhorias!*

