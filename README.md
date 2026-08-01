# 💠 Qubit Invest
 
**Assistente virtual de investimentos com IA que aplica computação quântica (QUBO/QAOA) para otimização de carteiras.**
 
Projeto desenvolvido por [Nicolas Marques](https://github.com/nicolasmarques06-afk) no curso de Data Science da [DIO](https://www.dio.me/), em parceria com a Accenture.
 
---
 
## O Problema
 
Investidores iniciantes têm dificuldade em decidir como distribuir seu capital entre diferentes ativos de forma que equilibre retorno e risco. Fazer essa análise manualmente é complexo — o número de combinações possíveis cresce rapidamente conforme aumenta a quantidade de ativos, tornando inviável encontrar a alocação ideal por tentativa e erro.
 
## A Solução
 
O **Qubit Invest** une três camadas para resolver esse problema:
 
1. **Chat conversacional** — um agente (o **Q**) que interpreta o perfil, capital e objetivos do investidor em linguagem natural, usando um LLM local (Ollama), sem custo.
2. **Base de conhecimento estruturada** — 10 arquivos JSON/CSV com dados do cliente, ativos, correlações, taxas de mercado e regras de tributação.
3. **Módulo de otimização quântica** — a escolha de ativos é formulada como um problema **QUBO** e resolvida com **QAOA**, rodando em simulador local via [Qiskit](https://www.ibm.com/quantum/qiskit).
O LLM nunca inventa a alocação de carteira — ele só explica, em linguagem simples, o resultado real calculado pelo módulo quântico.
 
> **Escopo:** projeto educacional/experimental. Não é recomendação financeira formal nem substitui um consultor certificado.
 
---
 
## Como Funciona
 
```
Cliente → Interface (Streamlit) → LLM (Q) → Base de Conhecimento
                                       ↓
                            Módulo de Otimização Quântica
                                  (QUBO + QAOA)
                                       ↓
                                  Validação → Resposta
```
 
O módulo quântico é o único componente que executa cálculo matemático real — os demais orquestram a conversa e traduzem o resultado.
 
---
 
## Tecnologias
 
| Camada | Ferramenta |
|---|---|
| Interface | [Streamlit](https://streamlit.io/) |
| LLM | [Ollama](https://ollama.ai/) (local, gratuito) |
| Otimização quântica | [Qiskit](https://www.ibm.com/quantum/qiskit) + `qiskit-optimization` (QUBO/QAOA) |
| Dados | `pandas`, `numpy` |
 
Todo o projeto roda **100% local, sem custo** — sem API paga, sem hardware quântico real.
 
---
 
## Como Rodar
 
```bash
# 1. Instalar dependências
pip install -r requirements.txt
 
# 2. Instalar e rodar o Ollama (LLM local, gratuito)
# Baixe em https://ollama.com e depois rode:
ollama pull llama3
 
# 3. Rodar a aplicação
streamlit run src/app.py
```
 
O app abre em `http://localhost:8501`. Na sidebar é possível ajustar os parâmetros da otimização (quantidade de ativos e aversão a risco) antes de pedir uma recomendação de carteira no chat.
 
---
 
## Estrutura do Projeto
 
```
qubit-invest/
├── src/
│   ├── app.py                  # Interface de chat (Streamlit)
│   ├── knowledge_base.py       # Carregamento da base de conhecimento
│   ├── llm_client.py           # Cliente do LLM (Ollama)
│   ├── optimization.py         # Motor de otimização quântica (QUBO + QAOA)
│   └── requirements.txt
├── data/                       # Base de conhecimento (10 arquivos JSON/CSV)
├── docs/
│   ├── 01-documentacao-agente.md
│   ├── 02-base-conhecimento.md
│   ├── 03-prompts.md
│   ├── 04-metricas.md
│   └── 05-pitch.md
└── README.md
```
 
---
 
## Documentação
 
| Documento | Conteúdo |
|---|---|
| [`docs/01-documentacao-agente.md`](docs/01-documentacao-agente.md) | Caso de uso, persona, tom de voz e arquitetura |
| [`docs/02-base-conhecimento.md`](docs/02-base-conhecimento.md) | Descrição e estratégia de integração dos dados |
| [`docs/03-prompts.md`](docs/03-prompts.md) | System prompt, exemplos de interação e edge cases |
| [`docs/04-metricas.md`](docs/04-metricas.md) | Métricas de avaliação e cenários de teste |
| [`docs/05-pitch.md`](docs/05-pitch.md) | Roteiro do pitch de apresentação |
 
---
 
## Base de Conhecimento
 
| Arquivo | Descrição |
|---|---|
| `perfil_investidor_adaptado.json` | Dados do cliente (capital, perfil de risco, metas) |
| `produtos_financeiros_quantico.json` | Ativos disponíveis, com retorno e volatilidade |
| `matriz_correlacao.json` | Correlação entre ativos, para cálculo de risco combinado |
| `historico_precos_simulado.csv` | Série de preços usada no cálculo de retorno/risco |
| `taxas_referencia.json` | Selic, CDI e IPCA |
| `tabela_ir_regressiva.json` | Alíquotas de Imposto de Renda por prazo |
| `glossario_conceitos.json` | Termos técnicos explicados em linguagem simples |
| `disclaimer_compliance.json` | Aviso legal exibido junto às recomendações |
| `transacoes.csv` | Fluxo de caixa do cliente |
| `historico_atendimento.csv` | Histórico de atendimento / FAQ |
 
---
 
## Limitações Conhecidas
 
- O QAOA rodando localmente, sem GPU, leva de 1 a 3 minutos por consulta.
- Não existe vantagem quântica comprovada para problemas financeiros em produção — este projeto é uma prova de conceito, não uma alternativa performática a métodos clássicos.
- Detecção de pedidos de carteira é baseada em palavras-chave simples.
- Ainda não foram realizados testes estruturados formais com terceiros.
## Próximos Passos
 
- [ ] Testes estruturados com métricas de assertividade, segurança e coerência
- [ ] Otimizar latência do módulo quântico
- [ ] Expandir a base com dados macroeconômicos em tempo real (API do Banco Central)
---
 
## Autor
 
**Nicolas Marques**
Projeto desenvolvido no curso de Data Science da [DIO](https://www.dio.me/), em parceria com a Accenture.
 
*Este repositório é um fork do desafio [`dio-lab-bia-do-futuro`](https://github.com/digitalinnovationone/dio-lab-bia-do-futuro).*
