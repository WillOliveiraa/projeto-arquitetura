# Projeto Arquitetura

Aplicação de gestão de contas e cartões, desenvolvida em TypeScript para demonstrar uma arquitetura modular com separação entre regras de negócio, interface e infraestrutura.

## Arquitetura

- **Monorepo com npm Workspaces e Turborepo:** aplicações, módulos de negócio e pacotes compartilhados no mesmo repositório, com execução coordenada de builds e testes.
- **Clean Architecture e princípios de DDD:** entidades, Value Objects e casos de uso ficam em `modules/`, independentes de NestJS, Next.js e Prisma. As validações do domínio são encapsuladas nos modelos.
- **Inversão de dependência:** os módulos definem contratos de repositórios e consultas; o backend fornece as implementações com Prisma.
- **Separação de escrita e leitura (CQRS simplificado):** casos de uso coordenam alterações, enquanto contratos de consulta atendem às leituras e listagens paginadas.
- **Frontend organizado por funcionalidade:** cada módulo reúne páginas, componentes, hooks e clientes HTTP, separando apresentação, comportamento e comunicação com a API.
- **Contratos e resultados compartilhados:** DTOs, abstrações de entidades, Value Objects e o padrão `Result` padronizam dados, validações e tratamento de falhas.
- **Testabilidade:** regras de negócio podem ser verificadas com Jest e repositórios em memória, sem depender do banco de dados.

## Estrutura

```text
apps/
  web/                 # Next.js, React e Tailwind CSS
  backend/             # API NestJS e adaptadores Prisma
modules/
  contas/              # Domínio e casos de uso de contas
  cartao/              # Domínio e casos de uso de cartões
  auth/                # Modelos de perfis e permissões
packages/
  shared/              # Abstrações, validações e contratos comuns
  eslint-config/       # Configuração compartilhada de lint
  typescript-config/   # Configuração compartilhada de TypeScript
```

Nas operações de escrita, o fluxo é: **interface → API → caso de uso → domínio → contrato de repositório → Prisma → PostgreSQL**. As leituras utilizam contratos de consulta implementados no backend.

O núcleo de negócio depende dos contratos compartilhados; os detalhes de framework e persistência ficam nas aplicações.

## Tecnologias

TypeScript · Next.js · React · Tailwind CSS · NestJS · Prisma · PostgreSQL · Docker Compose · Jest · Turborepo

## Comandos principais

Na raiz do repositório, após configurar os arquivos `.env` das aplicações e preparar o banco de dados:

```bash
npm install
npm run dev     # Inicia frontend e backend
npm run build   # Compila aplicações e pacotes
npm test        # Executa os testes disponíveis
npm run lint    # Executa o lint
```

Os exemplos de variáveis estão em `apps/backend/.env.example` e `apps/web/.env.example`. Ajuste `DATABASE_URL` ao banco definido em `apps/backend/docker-compose.yml` e configure `JWT_SECRET`. O frontend utiliza a porta `3000` e a API, por padrão, a porta `4000`.
