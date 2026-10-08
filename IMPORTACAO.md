# Importação do relatório GLPI

Fonte: `glpi 1.csv`, recebido do usuário. 319 chamados únicos, sem IDs duplicados. Referência adotada: **08/10/2026 às 10:29**, última atualização encontrada no arquivo; o CSV não informa a hora da extração.

| Empresa | Total exportado | Q2 (parcial) | Q3 | Q4 até a referência | Em aberto na referência |
|---|---:|---:|---:|---:|---:|
| Aliança Navegação | 202 | 36 | 154 | 12 | 19 |
| Maersk Truck | 117 | 26 | 79 | 12 | 15 |
| Total | 319 | 62 | 233 | 24 | 34 |

## Cobertura e regras

- Aberturas entre 03/06 e 07/10/2026. Q1 está sem cobertura; Q2 e Q4 são parciais. Totais refletem a seleção exportada, não comprovam a totalidade do GLPI.
- Empresas extraídas da Categoria. Todos os registros têm entidade “Maersk”, portanto a entidade não foi usada para essa separação. O chamado 2610060827 tem categoria vazia e foi atribuído a Maersk Truck a partir do campo “SELECIONE A INSTÂNCIA” na descrição.
- 266 Fechados e 19 Solucionados são resolvidos, todos com Data da solução. Os outros 34 são abertos. Não houve conflito entre status terminal e presença de solução.
- Técnicos: nome do campo “Atribuído - Técnico”, com espaços removidos. Dez chamados não têm técnico atribuído.
- “Critica” foi normalizada para “Crítica”. Prioridades Alta e Muito alta continuam distintas e não entram no KPI de críticos.
- Temas: categoria depois da empresa, preservando N1, N2 e N3. Módulos: rota do campo URL da ocorrência, sem URL completa. Seis chamados não têm rota identificável e aparecem como “Não informado”.
- SLA: prazo do campo “Tempo para solução”, não a barra de progresso. Há 315 prazos informados. A data da solução tem precisão de minuto, enquanto o prazo tem segundos; uma solução no mesmo minuto do prazo sem certeza sobre os segundos seria indeterminada. Não houve casos indeterminados nesta base.
- IA: campo ausente, mantido como N/D. Não foi inferido atendimento automatizado a partir de autor ou técnico.
- Backlog histórico é reconstruído pela data da solução final. Sem eventos de reabertura, não é possível reproduzir todas as mudanças passadas. Status intermediário histórico é exibido como “Em aberto (histórico)”.

## Conferência de Q3

233 aberturas: 154 Aliança Navegação e 79 Maersk Truck. Fila reconstruída em 30/09: 44 chamados (27 Aliança e 17 Truck), dos quais 17 acima de 30 dias. SLA no recorte: 41,4%, sobre os resolvidos com prazo calculável, considerando também chamados abertos antes de Q3.

## Tratamento do arquivo

O CSV usa ponto e vírgula, aspas e campos com múltiplas linhas. O trecho de script HTML encontrado após os registros foi descartado como conteúdo externo à tabela e não foi executado. Os registros foram tratados como dados. A base do site não contém descrição integral, e-mails, requerentes, observadores ou URLs completas.

## Verificações realizadas

Contagem e unicidade de IDs; datas de abertura/solução; coerência entre status e solução; separação de empresas; filtros de quatro trimestres e duas empresas; ausência de cobertura apresentada como N/D; ausência de IA preservada. O relatório original não foi alterado.
