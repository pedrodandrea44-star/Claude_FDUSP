# Graduação - FDUSP

App web para acompanhar a graduação na Faculdade de Direito da USP: leituras
(com progresso por páginas e prazos), horário de aulas, agenda de provas e
entregas, e um guia das salas do prédio.

**App no ar:** https://fdusp-483f8.web.app/

## Como funciona

- **Um único arquivo:** todo o app (HTML, CSS e JavaScript) vive em
  [`public/index.html`](public/index.html), incluindo a fonte Montserrat
  embutida em base64.
- **Backend:** Firebase (projeto `fdusp-483f8`)
  - **Autenticação:** login com Google (Firebase Auth).
  - **Dados:** Firestore, documento `usuarios/{uid}` com os campos
    `leituras`, `aulas`, `agenda`, `colunasExtra`, `colunasOcultas` e
    `ordemColunas`. Cache local persistente: funciona offline e sincroniza
    ao reconectar.
  - **Hospedagem:** Firebase Hosting, servindo a pasta `public/`.

## Como publicar uma alteração

**Automático:** todo push nos branches `main` ou `claude/**` dispara o
workflow [`deploy.yml`](.github/workflows/deploy.yml), que publica a pasta
`public/` no Firebase Hosting. Ele exige o segredo `FIREBASE_SERVICE_ACCOUNT`
no repositório (Settings → Secrets and variables → Actions), contendo o JSON
de uma chave de conta de serviço do projeto (console do Firebase →
Configurações do projeto → Contas de serviço → Gerar nova chave privada).
Sem o segredo, o workflow apenas avisa e pula o deploy.

**Manual (alternativa):** com login no Google dono do projeto:

```bash
npx firebase-tools login
npx firebase-tools deploy --only hosting
```

O `.firebaserc` já aponta para o projeto `fdusp-483f8`, e o `firebase.json`
já configura a pasta `public/` como raiz do site.

Há também o workflow [`resgatar-originais.yml`](.github/workflows/resgatar-originais.yml)
(disparo manual), que baixa do site publicado o `index.html`, os ícones e o
manifest e os grava no repositório — útil como conferência de que o
repositório bate com o que está no ar. Atenção: ele sobrescreve os arquivos
do repositório com a versão publicada; não o rode com alterações ainda não
publicadas.

## Histórico da recuperação (ago/2026)

A conversa original de desenvolvimento foi apagada por engano; o app em si
nunca saiu do ar. Este repositório foi reconstruído a partir do código-fonte
da versão publicada em `fdusp-483f8.web.app`. A reconstrução do
`index.html` foi conferida contra o site no ar e ficou **byte a byte
idêntica** ao original (a fonte Montserrat embutida veio do pacote oficial
`@fontsource-variable/montserrat`, o mesmo usado na versão publicada). Os
ícones e o `manifest.webmanifest` originais foram baixados do site pelo
workflow de resgate.

Os dados dos usuários ficam no Firestore e **não** são afetados por deploys
do Hosting.
