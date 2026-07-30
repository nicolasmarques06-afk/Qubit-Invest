# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`:

| Arquivo | Formato | Descrição |
|---------|---------|-----------|
| `transacoes.csv` | CSV | Receitas e despesas mensais do cliente, usadas para estimar capacidade de investimento|
| `historico_atendimento.csv` | CSV | Registros de dúvidas e atendimentos anteriores, usados como base de FAQ do agente |
| `historico_precos_simulado.csv` | CSV | Série histórica de preços por ativo, usada para calcular retorno e risco na otimização |
| `perfil_investidor_adaptado.json` | JSON | Dados do cliente (capital, perfil de risco, metas), usados para personalizar a recomendação |
| `produtos_financeiros_quantico.json` | JSON | Ativos disponíveis, com retorno esperado e volatilidade anualizados |
| `disclaimer_compliance.json` | JSON | 	Texto de aviso legal exibido junto às recomendações do agente |
| `glossario_conceitos.json` | JSON | 	Termos técnicos (QUBO, QAOA, volatilidade etc.) explicados em linguagem simples |
| `tabela_ir_regressiva.json` | JSON |	Alíquotas de Imposto de Renda por prazo, usadas para calcular retorno líquido |
| `taxas_referencia.json` | JSON | Selic, CDI e IPCA, usados para converter rentabilidade percentual em valores reais|
| `matriz_correlacao.json` | JSON | Correlação entre os ativos, usada para calcular o risco combinado da carteira|
> [!TIP]

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Sim. Os arquivos perfil_investidor.json e produtos_financeiros.json, fornecidos originalmente pela DIO, foram adaptados com campos numéricos (capital disponível, tolerância a risco, retorno esperado e volatilidade anualizados), necessários para alimentar o módulo de otimização quântica (QUBO/QAOA). Além disso, foram criados 6 novos arquivos que não existiam na base original — matriz_correlacao.json, historico_precos_simulado.csv, taxas_referencia.json, tabela_ir_regressiva.json, glossario_conceitos.json e disclaimer_compliance.json — necessários porque os dados fornecidos eram voltados a um agente de atendimento genérico, e não continham as informações matemáticas (correlação, séries de preços) exigidas para formular e resolver o problema de otimização de carteira. Os arquivos transacoes.csv e historico_atendimento.csv foram mantidos sem alterações.
---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Os arquivos JSON e CSV são carregados no início da sessão a partir do diretório local da base de conhecimento. Os dados fixos (perfil do cliente, produtos, taxas, glossário, disclaimer) são lidos uma vez e mantidos em memória durante a conversa. Os dados numéricos usados no cálculo (histórico de preços, matriz de correlação) são carregados sob demanda, no momento em que o módulo de otimização quântica é acionado.
### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Uma combinação dos dois. Informações estáveis e de baixo volume (perfil do cliente, disclaimer, glossário) são incluídas diretamente no system prompt do LLM, para que o agente sempre tenha esse contexto disponível. Já os dados de maior volume (histórico de preços, matriz de correlação) não vão para o prompt — são consultados dinamicamente pelo módulo de otimização quântica, que retorna apenas o resultado (alocação de ativos) para o LLM incluir na resposta. Isso evita estourar o limite de contexto do prompt com dados numéricos que o LLM não precisa interpretar diretamente.

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil de risco: Moderado
- Capital disponível para investimento: R$ 5.000,00
- Objetivo principal: Construir reserva de emergência

Ativos disponíveis (resumo):
- Tesouro Selic: retorno 10,5% a.a. | risco baixo
- Fundo Multimercado: retorno 12,5% a.a. | risco médio
- ETF BOVA11: retorno 12,0% a.a. | risco alto

Resultado do Módulo de Otimização Quântica:
- Tesouro Selic: 40%
- Fundo Multimercado: 35%
- ETF BOVA11: 25%

Disclaimer: Esta é uma sugestão educativa baseada em modelo matemático, não uma recomendação financeira formal.
...
```
