# Full Cycle — Clean Architecture

Projeto baseado no template [fc-clean-architecture](https://github.com/devfullcycle/fc-clean-architecture). O fluxo de Customer (use cases, notification, validador yup e API Express) foi usado como referência para implementar Product.

## Como rodar

```bash
npm install
npm test
npm run dev
```

- `npm test` executa `tsc --noEmit` e o Jest.
- `npm run dev` sobe a API com nodemon em `src/infrastructure/api/server.ts` (porta `3000`, ou `PORT`).
- O banco da API é SQLite em memória, iniciado no `express.ts`.

## API

| Método | Rota | Descrição |
| --- | --- | --- |
| POST | `/customer` | Cria um customer |
| GET | `/customer` | Lista customers (JSON ou XML com `Accept: application/xml`) |
| POST | `/product` | Cria um product (`name`, `price`) |
| GET | `/product` | Lista products (JSON ou XML com `Accept: application/xml`) |

Exemplo:

```bash
curl -X POST http://localhost:3000/product \
  -H "Content-Type: application/json" \
  -d '{"name":"Product 1","price":100}'

curl http://localhost:3000/product
curl http://localhost:3000/product -H "Accept: application/xml"
```

## Testes

Os testes de integração e o E2E usam Sequelize com SQLite em memória e supertest.

```bash
npm test
```

Cobertura de Product:

- unitário com repositório mockado e integração com banco para create, find, list e update
- E2E de `POST /product` e `GET /product` em JSON e XML

## Desafios de Product

1. **Use cases** — Create, Find, List e Update em `src/usecase/product`, com DTO, teste unitário e teste de integração em cada um.
2. **API** — rota `/product` registrada no Express, com `ProductModel` e presenter XML.
3. **Notification** — a entidade Product acumula os erros (id vazio, nome vazio e preço negativo) na notification em vez de lançar no primeiro erro. Nome vazio e preço negativo geram os dois erros juntos.
4. **Validação desacoplada** — `ProductYupValidator` e `ProductValidatorFactory`. A entidade delega a validação e o validador preenche a notification.
