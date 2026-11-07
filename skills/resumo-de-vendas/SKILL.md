---
name: resumo-de-vendas
description: >-
  Sales briefing from Imans for a day, week, month or custom period: headline number, comparison,
  channels, top products and what deserves attention. Use when the user asks "como estão as vendas?",
  "resumo do dia/da semana/do mês", "fechamento de vendas do mês", "quanto vendemos ontem?",
  "relatório de vendas para a diretoria", "how are sales going", "daily/weekly sales brief".
  Do NOT use to explain a specific drop or spike (use diagnostico-de-vendas), for margin or
  profitability (use margem-e-mix) or for finance/cash (use fechamento-financeiro).
---

# Resumo de vendas

Responda no idioma do usuário (padrão: português do Brasil). Valores em R$ com separador brasileiro.

## 1. Contexto (no máximo uma pergunta)

- Workspace: se não estiver claro, chame `list_my_workspaces` (veja a skill primeiros-passos).
- Período: use o que o usuário disse. "Hoje/ontem/esta semana/este mês" são relativos à data atual no
  fuso do workspace. Se for totalmente ambíguo, assuma **mês corrente até ontem** e diga que assumiu.
- Empresa (CNPJ): em workspace com várias empresas, só filtre se o usuário pedir.

## 2. Busca (2–4 chamadas, em paralelo quando possível)

1. `get_sales_insights` para o período — KPIs, comparação com a janela anterior, cobertura de lucro e
   top produtos. É a chamada principal.
2. `get_sales_by_channel` para a divisão por canal/classificação (marketplace, e-commerce, atacado…).
3. Só se o período tiver mais de ~10 dias: `get_sales_by_period` com bucket `day` ou `week` para ver
   a forma da curva.
4. Se `read_insights_reporting` estiver disponível: leia as Opportunities/Highlights abertas para o
   período (verifique `freshness` antes de chamá-las de atuais).

Se o usuário quiser um card visual e `show_sales_overview_card` existir, use-o além do texto.

## 3. Passo cético (antes de escrever)

- **Dia parcial:** se o período inclui hoje, o número é parcial. Compare com o mesmo horário/dia ou
  exclua hoje, e diga qual escolha fez.
- **Dia da semana:** compare semana com semana e dia com o mesmo dia da semana anterior, nunca
  segunda contra domingo.
- **Sazonalidade BR:** Black Friday, Natal, Dia das Mães, dia do pagamento (5º dia útil e dia 20),
  campanhas de marketplace (11.11, 12.12). Uma variação nessas datas não é tendência.
- **Sincronização:** pedidos recentes do ERP/marketplace podem ainda não ter chegado. Se os últimos
  1–2 dias estiverem muito abaixo do normal, sugira atraso de sincronização antes de afirmar queda.
- **Faturamento e devoluções:** os totais seguem as configurações do workspace (exigir NF, política
  de devoluções). Não some nem subtraia devoluções de novo.
- Descarte variações menores que ~5% como ruído, a não ser que o usuário peça detalhe.

## 4. Formato da resposta

1. **Manchete** em uma frase: faturamento do período, variação contra a referência e o principal motivo.
2. Tabela curta: faturamento, pedidos, ticket médio — atual × anterior × variação.
3. Canais: os 3–5 maiores com participação e variação.
4. Produtos: top 5 e qualquer produto que mudou muito.
5. **Atenção:** no máximo 3 pontos acionáveis, cada um marcado como *(leitura)* ou *(requer ação no Imans)*.
6. Uma linha sobre as premissas (período, eixo de data, dia parcial).

Não dê conselho de preço não solicitado. Para causas de uma queda, ofereça a skill diagnostico-de-vendas.
