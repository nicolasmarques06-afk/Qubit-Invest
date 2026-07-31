# Código da Aplicação

Esta pasta contém o código do agente financeiro Qubit Invest.

## Estrutura Sugerida

```
qubit_invest/
├── app.py                      # Aplicação principal (Streamlit) - interface de chat
├── knowledge_base.py           # Carregamento da base de conhecimento (JSON/CSV)
├── llm_client.py                # Cliente do LLM (Ollama, rodando localmente)
├── optimization.py              # Motor de otimização quântica (QUBO + QAOA via Qiskit)
├── requirements.txt              # Dependências
└── base_conhecimento/
    ├── perfil_investidor_adaptado.json
    ├── produtos_financeiros_quantico.json
    ├── matriz_correlacao.json
    ├── historico_precos_simulado.csv
    ├── taxas_referencia.json
    ├── tabela_ir_regressiva.json
    ├── glossario_conceitos.json
    ├── disclaimer_compliance.json
    ├── transacoes.csv
    └── historico_atendimento.csv
```

## requirements.txt

```
streamlit
requests
pandas
numpy
qiskit
qiskit-optimization
qiskit-algorithms
```

## Como Rodar

```bash
# 1. Instalar dependências
pip install -r requirements.txt

# 2. Instalar e rodar o Ollama (LLM local, gratuito)
# Baixe em https://ollama.com e depois rode:
ollama pull llama3

# 3. Rodar a aplicação
streamlit run app.py



O app abre automaticamente no navegador (http://localhost:8501). Na sidebar é possível ajustar os parâmetros da otimização (quantidade de ativos e aversão a risco) antes de pedir uma recomendação de carteira no chat.
'''
