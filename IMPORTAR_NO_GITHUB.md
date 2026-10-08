# Importar o dashboard no GitHub

Este pacote contém o dashboard TMS Cabotagem pronto para publicação com GitHub Pages.

## Conteúdo

- `index.html`: página principal; carrega a base em `real.js`.
- `real.js`: base tratada com 319 chamados e somente os campos necessários aos indicadores.
- `app.js` e `styles.css`: cálculos, filtros, gráficos e apresentação.
- `favicon.svg`: ícone do site.
- `README.md` e `IMPORTACAO.md`: documentação e regras de tratamento dos dados.
- `pages.yml`: arquivo de configuração incluído no projeto original.

## Subir os arquivos

1. Baixe e extraia o ZIP.
2. No repositório `TMS-CABOTAGEM`, escolha **Add file → Upload files**.
3. Envie o conteúdo extraído para a raiz do repositório — não envie uma pasta externa contendo os arquivos.
4. Faça o commit na branch `main`.
5. Em **Settings → Pages**, selecione **Deploy from a branch**, `main` e `/(root)` (ou mantenha a configuração de Pages já ativa).
6. Aguarde o GitHub Pages atualizar o site.

O `index.html` deste pacote aponta para `real.js` na raiz; isso corrige o caminho quebrado que impedia o carregamento dos dados.

## Privacidade e limites dos dados

O ZIP **não inclui o CSV bruto**. O relatório original contém e-mails, descrições de chamados e outros dados pessoais. A base `real.js` contém apenas os campos necessários ao dashboard. Como o repositório e o site são públicos, confirme que a publicação desses indicadores e nomes de técnicos está autorizada pela sua organização.

Os 319 chamados refletem apenas a seleção contida no relatório, com abertura entre 03/06 e 07/10/2026; Q1 não tem cobertura e Q2/Q4 são parciais. O dashboard é uma fotografia estática, sem integração automática com o GLPI.
