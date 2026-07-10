# LH Games — Consumo de API (Service, Login, Guard)

Este pacote contém o projeto completo (código-fonte + arquivos de configuração) e o arquivo `dbgames.json`, banco de dados fake usado pelo `json-server`.

Testei o projeto de ponta a ponta (npm install + ng build, development e production) antes de te enviar, e corrigi 2 problemas reais que encontrei:

1. **`src/app/app.component.css` estava faltando** no repositório (o componente referenciava o arquivo, mas ele não existia). Sem ele, o build quebra. Já criei o arquivo (vazio, pronto para você estilizar se quiser).
2. **`@types/node` sem versão fixada**: sem um `package-lock.json` no repositório, o `npm install` puxa a versão mais recente do `@types/node` como dependência transitiva, o que conflita com o TypeScript 4.8 usado pelo Angular 15 e quebra o build com dezenas de erros de tipo. Corrigi fixando a versão (`18.16.9`, compatível com Angular 15) no `package.json` e incluí um `package-lock.json` gerado — **importante: suba esse `package-lock.json` para o GitHub também**, ele garante que todo mundo (você, o tutor) instale exatamente as mesmas versões.

## Como aplicar no seu projeto local

Copie estes arquivos/pastas para a raiz do seu projeto, substituindo os existentes:
- `src/app` (código-fonte completo)
- `src/main.ts`
- `tsconfig.json`, `tsconfig.app.json`, `tsconfig.spec.json`
- `package.json`
- `package-lock.json`
- `dbgames.json` (raiz do projeto, mesmo nível do `angular.json`)

## Passo a passo para rodar

1. Apague `node_modules` (se existir) e rode:
   ```
   npm install
   ```
2. Instale o `json-server` globalmente (se ainda não tiver):
   ```
   npm install -g json-server
   ```
3. Na raiz do projeto, suba o servidor fake da API:
   ```
   json-server --watch dbgames.json
   ```
   API disponível em `http://localhost:3000/produtos`.
4. Em outro terminal:
   ```
   ng serve
   ```
5. Acesse `http://localhost:4200`.

## Usuário para testes de login

- **Usuário:** aluno
- **Senha:** 1234

## O que foi implementado

- `src/app/models/Produto.model.ts` e `Usuario.model.ts`
- `src/app/produto.service.ts` — Service com GET, POST, PUT e DELETE para `http://localhost:3000/produtos`
- `src/app/login.service.ts` — autenticação simples e controle de exibição do menu
- `src/app/guard.guard.ts` — `CanActivate` que bloqueia `/restrito` sem token
- `src/app/restrito/*` — área restrita completa (menu-restrito, lista-produto, cadastro-produto, atualiza-produto)
- `src/app/inicio` — tela inicial agora lista os produtos vindos da API (antes eram fixos)
- `src/app/login` — tela de login conectada ao `LoginService`
- `src/app/app.module.ts` — módulos `HttpClientModule`, `FormsModule`, `ReactiveFormsModule` e `RestritoRoutingModule` adicionados
- `src/app/app-routing.module.ts` — rota `/restrito` protegida pelo Guard
- `src/app/app.component.ts` / `.html` — menu principal escondido automaticamente ao entrar na área restrita

## Testando a API isoladamente (Postman)

Antes de testar no Angular, você pode validar a API com o Postman, seguindo os passos do material do SENAI (GET, POST, PUT `/produtos/:id`, DELETE `/produtos/:id`), usando a URL `http://localhost:3000/produtos`.

