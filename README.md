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

1. Edite `public/index.html`.
2. Faça o deploy (requer login no Google dono do projeto):

   ```bash
   npx firebase-tools login
   npx firebase-tools deploy --only hosting
   ```

O `.firebaserc` já aponta para o projeto `fdusp-483f8`, e o `firebase.json`
já configura a pasta `public/` como raiz do site.

## Histórico da recuperação (ago/2026)

A conversa original de desenvolvimento foi apagada por engano; o app em si
nunca saiu do ar. Este repositório foi reconstruído a partir do código-fonte
da versão publicada em `fdusp-483f8.web.app`, com duas ressalvas:

- A fonte Montserrat embutida foi reconstituída a partir do pacote oficial
  `@fontsource-variable/montserrat` (mesma fonte, pesos 400–700, subset
  latino) — visualmente idêntica à original.
- Os ícones (`icone.svg`, `icone-32.png`, `icone-192.png`, `icone-512.png`,
  `apple-touch-icon.png`) e o `manifest.webmanifest` foram **recriados**
  (capelo de formatura nas cores do app), pois os arquivos originais não
  puderam ser baixados. Os originais continuam no ar no Hosting até o
  próximo deploy — se quiser preservá-los, baixe-os do site publicado
  antes de fazer um novo deploy.

Os dados dos usuários ficam no Firestore e **não** são afetados por deploys
do Hosting.
