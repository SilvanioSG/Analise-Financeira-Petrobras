<p align="center">
  <img src="LogotipoSG.png" alt="Logotipo SG" height="90">
</p>

<h1 align="center">Viabilidade e Valuation — Petrobras (PETR4)</h1>

<p align="center">
  <strong>Saúde Financeira, Viabilidade de Investimento e Diagnóstico de Preço Justo</strong><br>
  Base: DFP CVM (2019–2024) + Cotação B3 · Data-base: 31/12/2024
</p>

<p align="center">
  <a href="https://silvaniosg.github.io/Analise-Financeira-Petrobras/"><strong>▶ Acessar o relatório interativo (Plotly dinâmico)</strong></a>
</p>

---

## Objeto e Intuito

Este projeto aplica técnicas de **análise de dados financeiros** e conceitos de **Business Intelligence** sobre os demonstrativos contábeis consolidados da Petrobras — DRE, BPA, BPP e DFC — com o objetivo de responder, com base empírica, a três perguntas de negócio essenciais à decisão de um gestor:

1. A empresa está saudável financeiramente?
2. É viável investir?
3. Qual o diagnóstico sobre o preço justo?

Foi concebido como estudo de caso de um **Gestor de Operações e Negócios Orientado a Dados**, integrando extração, tratamento, modelagem dimensional, cálculo de indicadores, reconciliação com fontes oficiais e valuation por múltiplos. Todo o pipeline é reproduzível e auditável.

---

## Perguntas e Respostas de Negócio

| Pergunta | Resposta Sintética | Evidência |
|---|---|---|
| A empresa está saudável financeiramente? | **Sim, com ressalva de liquidez.** Alavancagem controlada e margem EBITDA robusta; liquidez corrente abaixo de 1,0 exige monitoramento. | Dív.Líq./EBITDA 1,60x · Margem EBITDA 41,6% · Liquidez Corrente 0,69x |
| É viável investir? | **Sim, com compreensão do ruído contábil.** O lucro reportado é deprimido por efeito cambial e impairment; o lucro normalizado sustenta múltiplos atrativos. | Lucro reportado R$ 37,0 bi → normalizado R$ 84,1 bi · P/L normalizado 4,74x |
| Qual o diagnóstico sobre o preço justo? | **Desconto relevante frente a pares e à média histórica.** EV/EBITDA e P/VP sinalizam precificação conservadora. | EV/EBITDA 3,55x · P/VP 1,08x · P/L 10,77x |

---

## Origem dos Dados

- **Fonte primária:** DFP (Demonstrações Financeiras Padronizadas) da **CVM** — arquivos anuais `dfp_cia_aberta_{DRE,BPA,BPP,DFC_MI}_con_{ano}.csv`, 2019–2024, download idempotente com cache local.
- **Fonte de mercado:** cotação de fechamento PETR4 em 31/12/2024 (R$ 30,56) e Dividend Yield via **yfinance/B3**.
- **Reconciliação:** valores-chave confrontados com o release de resultados do **RI Petrobras** (EBITDA, Dívida Líquida Ajustada, PL) para validação de escopo.

---

## Metodologia Técnica

### 1. Ingestão e Tratamento
Download idempotente dos ZIPs DFP/CVM (2019–2024). Filtro por razão social e consolidação por exercício (`DT_FIM_EXERC`). **Deduplicação ordenada por `[ANO_ARQUIVO, ORDEM_EXERC]`** para priorizar a versão mais recente em casos de *restatement*. Conversão de escala (milhares → BRL) e tipagem explícita.

### 2. Extração de Contas
Mapeamento pelo plano de contas CVM: Receita (3.01), Lucro Bruto (3.03), EBIT (3.05), Lucro Líquido (3.11). D&A operacional via conta **6.01.01.04** com fallback por descrição — excluindo `6.03.*` (financiamentos). Dívida Bruta = Empréstimos CP (2.01.04) + LP (2.02.01); auditoria empírica da conta 2.01.05 confirmou ausência de arrendamentos embutidos. Dívida Líquida = Dívida Bruta − (Caixa 1.01.01 + Aplicações 1.01.02).

### 3. Indicadores Calculados
Margens (bruta, EBIT, EBITDA, líquida), retorno (ROE, ROA), liquidez (corrente), alavancagem (Dív.Líq./EBITDA e Dív.Líq./PL) e múltiplos de valuation (P/L, P/VP, EV/EBITDA) na base de 31/12/2024.

### 4. Normalização e Análise de Anomalias
Identificação de eventos não recorrentes (impairment, variação cambial, crédito de imposto diferido) e cálculo do **lucro normalizado**, removendo o efeito real do câmbio — com nota metodológica explícita sobre a premissa (câmbio é recorrente em empresa com dívida em USD, mas o *efeito* é ajustado para leitura do resultado operacional).

### 5. Modelagem Dimensional para BI
Exportação em **Star Schema** para consumo em Power BI: 4 tabelas fato (fDRE, fBPA, fBPP, fDFC), 2 dimensões (dConta, dCalendario) e fIndicadores. Sufixos de unidade (`_pct`, `_x`) eliminam ambiguidade semântica no BI.

### 6. Reconciliação com Fonte Oficial
Divergência calculada vs. reportado do RI: **+1,3% na Dívida Líquida** (R$ 326,8 bi vs R$ 322,6 bi), explicitada como diferença de escopo da "Dívida Líquida Ajustada". EBITDA calculado R$ 204,2 bi vs reportado R$ 208,1 bi (~2% de gap, compatível com escopo de D&A).

---

## Indicadores-Chave — 2024

| Indicador | Valor |
|---|---|
| Receita Líquida | R$ 490,8 bi |
| Lucro Líquido | R$ 37,0 bi |
| EBITDA | R$ 204,2 bi |
| Margem EBITDA | 41,6% |
| ROE | 10,1% |
| Liquidez Corrente | 0,69x |
| Dív.Líq./EBITDA | 1,60x |
| Dívida Líquida | R$ 326,8 bi |
| Patrimônio Líquido | R$ 367,5 bi |

---

## Valuation — 31/12/2024

| Múltiplo | Valor |
|---|---|
| Market Cap | R$ 398,6 bi |
| P/L reportado | 10,77x |
| P/L normalizado | 4,74x |
| P/VP | 1,08x |
| EV | R$ 725,5 bi |
| EV/EBITDA | 3,55x |
| Dividend Yield | 6,24% |

---

## Visualizações do Relatório

### 1. Evolução de Receita, EBIT e Lucro Líquido (2018–2024)

![Receita, EBIT e Lucro Líquido](1-Petrobras%20%E2%80%94%20Receita%2C%20EBIT%20e%20Lucro%20L%C3%ADquido.PNG)

### 2. Decomposição da DRE — 2024

![Decomposição da DRE](2-Decomposi%C3%A7%C3%A3o%20da%20DRE%20%E2%80%94%20Petrobras.png)

### 3. Margens e ROE (2018–2024)

![Margens e ROE](3-Margens%20e%20ROE%20%E2%80%94%20Petrobras.png)

### 4. Liquidez × Alavancagem

![Liquidez e Alavancagem](Liquidez%20%C3%97%20Alavancagem%20%E2%80%94%20Petrobras.png)

---

## Insights Analíticos

1. **Ciclo de preço do petróleo é o motor da receita.** A receita saltou 66% em 2021 e 42% em 2022 com Brent alto, e caiu 20% em 2023 e 4% em 2024 com a normalização — a alavanca operacional é altíssima.
2. **Resultado 2024 é contaminado por itens não recorrentes.** Impairment (R$ 9,4 bi) e variação cambial negativa (R$ 60,9 bi) deprimem o lucro reportado; o lucro normalizado (R$ 84,1 bi) evidencia operação substancialmente mais rentável do que o número GAAP sugere.
3. **Alavancagem sob controle após desalavancagem estrutural.** Dív.Líq./EBITDA de 1,60x em 2024 está bem abaixo do covenant típico de 2,5x, com trajetória de queda desde 3,04x em 2020.
4. **Liquidez corrente é o ponto de atenção.** 0,69x em 2024 rompe o piso de 1,0x pela primeira vez na série — reflexo de maior concentração de dívida no curto prazo e distribuição robusta de dividendos.
5. **Preço de mercado embute desconto expressivo.** EV/EBITDA de 3,55x e P/L normalizado de 4,74x são múltiplos de ciclo baixo para uma major integrada de porte — o mercado precifica risco regulatório, transição energética e incerteza sobre política de preços.
6. **Reconciliação com o RI valida a metodologia.** Gap de 1,3% na Dívida Líquida é compatível com a definição de "Dívida Líquida Ajustada" do relatório oficial, atestando robustez do pipeline.

---

## Estrutura de Arquivos
.
├── AnaliseFinanceiraPetrobras.ipynb Pipeline completo (ETL → BI → valuation), reprodutível
├── AnaliseFinanceiraPetrobras.html Relatório final interativo (Plotly embutido)
├── requirements.txt Dependências Python do projeto
├── LogotipoSG.png Identidade visual do autor
└── *.png Quatro visualizações-chave do relatório


**Dependências principais:** `pandas`, `numpy`, `plotly`, `requests`, `yfinance`.

---

## Acesso Online

**Relatório interativo (Plotly dinâmico):**
[https://silvaniosg.github.io/Analise-Financeira-Petrobras/](https://silvaniosg.github.io/Analise-Financeira-Petrobras/)

---

## Autor

**Silvanio Gois** — Gestor de Operações e Negócios Orientado a Dados

- Site: [https://www.silvaniogois.com.br](https://www.silvaniogois.com.br)
- LinkedIn: [https://www.linkedin.com/in/silvanio-gois/](https://www.linkedin.com/in/silvanio-gois/)
- GitHub: [https://github.com/SilvanioSG](https://github.com/SilvanioSG)

---

## Limitações e Próximos Passos

- FCD (Fluxo de Caixa Descontado) não implementado nesta versão.
- Múltiplos calculados no fechamento de 2024, não em tempo real.
- Tratamento do câmbio como não recorrente é hipótese simplificadora, explicitada no código.
- Dividend Yield é da data corrente da análise, não de 31/12/2024.
- Roadmap: (i) ingestão de ITRs para atualização trimestral; (ii) sensibilidade Monte Carlo Brent × câmbio; (iii) benchmarking com Shell,
- TotalEnergies e Equinor; (iv) implementação de FCD com WACC documentado; (v) dashboard Power BI em quatro páginas (Executivo, DRE Waterfall, Saúde, Valuation).
