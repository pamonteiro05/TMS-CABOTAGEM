# TMS Cabotagem · Dashboard executivo

Dashboard estático em português para Aliança Navegação e Maersk Truck. Identidade visual inspirada nos prints fornecidos: laranja nstech, navegação escura e apresentação limpa. Não utiliza bibliotecas externas nem precisa de instalação ou compilação.

**A versão atual utiliza 319 chamados reais de `glpi 1.csv`: 202 de Aliança Navegação e 117 de Maersk Truck.** Aberturas entre 03/06 e 07/10/2026. Referência: 08/10/2026, última atualização observada às 10:29. A data de extração não é explicitada no CSV; a referência adotada é a última atualização registrada. Q1 não possui cobertura; Q2 e Q4 têm cobertura parcial. Veja `IMPORTACAO.md` para mapeamento e reconciliação.

## Visualizar

Abra `index.html` no navegador. Todos os arquivos usam caminhos relativos e funcionam em um repositório GitHub Pages. Se preferir, sirva esta pasta com um servidor estático local. O botão “Apresentar / PDF” abre a impressão do navegador, onde é possível salvar em PDF.

## Publicar no GitHub Pages

1. Crie um repositório e envie **o conteúdo desta pasta** para a raiz, incluindo `.github/workflows/pages.yml`.
2. Use a branch `main` ou ajuste o nome no arquivo do workflow.
3. Em Settings → Pages → Build and deployment, selecione **GitHub Actions**.
4. Aguarde a execução de “Publicar dashboard” em Actions. O endereço aparecerá em Settings → Pages.

Alternativa sem Actions: em Pages selecione “Deploy from a branch”, branch `main`, pasta `/ (root)`, e salve.

Nenhum repositório foi criado nem publicado automaticamente. Antes de publicar dados reais, confirme que o conteúdo pode ser público: o site e seu arquivo de dados ficam acessíveis aos visitantes.

## Substituir os dados

Edite `real.js`, atribuindo somente o objeto abaixo a `window.DASHBOARD_DATA`. Não misture registros fictícios e reais. O arquivo é carregado diretamente para funcionar inclusive sem servidor. CSV/XLSX devem ser normalizados para este contrato antes da substituição; envie o relatório para adaptação.

```js
window.DASHBOARD_DATA = {
  mode: 'real',
  referenceDate: '2026-09-30',
  tickets: [{
    id: '12345',
    company: 'Aliança Navegação', // ou 'Maersk Truck'
    openedAt: '2026-07-01',
    resolvedAt: '2026-07-05', // null se ainda aberto
    status: 'Resolvido',
    technician: 'Nome do técnico', // null se não atribuído
    module: 'Integração / EDI / SAP',
    priority: 'Normal', // 'Crítica' para o KPI de críticos
    aiHandled: null, // true, false ou null se desconhecido
    slaDueAt: '2026-07-06', // null se não informado
    slaMet: true, // comparar horário da resolução com prazo real
    theme: 'N3 – Falha na aplicação' // categoria do GLPI
  }]
};
```

Use datas válidas no formato ISO `AAAA-MM-DD`, IDs únicos, nomes exatos das duas empresas e resolução posterior à abertura. Campos sem informação devem ser `null`. A data de referência é a data de extração/fechamento do relatório. A classificação “Crítica” deve ser mapeada a partir da definição adotada pela equipe. O prazo de SLA deve vir do relatório ou de regra aprovada; não é inferido no dashboard real. Na base real, o prazo é o campo “Tempo para solução”. A comparação usa horários; a solução está disponível com precisão de minuto. O campo `slaMet` é booleano, ou null se não calculável.

## Regras dos indicadores

- **Total:** abertura no trimestre selecionado; período cortado na data de referência.
- **Média por atendente:** registros atribuídos / técnicos distintos com atribuição nesse recorte; não representa média de produtividade ou resolução.
- **IA:** true / registros com booleano conhecido. Sem cobertura, apresenta N/D.
- **SLA:** resolvidos no recorte dentro do prazo / resolvidos com prazo conhecido. Não mede SLA de primeira resposta.
- **Backlog:** todos os chamados abertos até o fim do recorte, sem resolução até essa data, inclusive trimestres anteriores. Sua idade é medida nessa data.
- **Críticos e >30 dias:** subconjuntos do backlog. Aging é a mediana das idades.
- **Evolução:** abertura e resolução contadas por suas próprias datas. Resoluções podem vir de trimestres anteriores.
- **Status, técnico, tema e módulo:** distribuição dos chamados abertos no recorte. Para status, a data de resolução permite reconstruir aberto/resolvido; sem histórico de mudanças, não é possível reconstruir o status intermediário exato de trimestres antigos.
- **Trimestres:** volume de aberturas do ano selecionado, para a empresa filtrada. Meses futuros são sinalizados no gráfico mensal.

Os insights são regras descritivas calculadas sobre os registros, sem inferência de causa. Para comparar backlog histórico corretamente, a base precisa conter todos os chamados ainda ativos, mesmo os abertos antes do ano analisado.

## Estrutura

`index.html`: interface · `styles.css`: visual responsivo e impressão · `app.js`: filtros e indicadores · `real.js`: base substituível · `.github/workflows/pages.yml`: deploy.

## Limitações

Fotografia estática, sem conexão automática ao GLPI, autenticação ou histórico de transferências entre técnicos. A tabela usa a atribuição informada no relatório. CSAT e FCR foram omitidos conforme as marcações dos prints. Para adotar o dashboard em produção, validar colunas, cobertura, prioridades e critérios de SLA com o relatório real.

## Atualização com relatório real

A entidade GLPI é Maersk em todos os registros e **não** foi usada para separar empresas. A separação usa a Categoria; quando ausente, usa a instância declarada. Módulo é a rota da URL da ocorrência, sem domínio, parâmetros ou descrição. A publicação inclui somente campos necessários ao dashboard: os textos integrais, e-mails, requerentes e URLs completas não integram a base publicada. Registros com status Fechado ou Solucionado são contabilizados como resolvidos. Sem histórico de reabertura, o backlog passado é uma reconstrução pela data da solução final, sujeita a essa limitação. O CSV exportado pode representar uma seleção: os totais referem-se exclusivamente à base recebida.
