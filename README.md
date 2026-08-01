# 🤖 Qubit Invest

## Contexto

Contexto

O mercado financeiro brasileiro tem crescido em investidores pessoa física acessando plataformas digitais, mas a maioria — especialmente iniciantes — não sabe como montar uma carteira que equilibre retorno e risco de forma eficiente.

Ao mesmo tempo, otimização de portfólio é um dos casos de uso mais maduros de computação quântica aplicada a finanças: o problema pode ser formulado como QUBO e resolvido com algoritmos como QAOA (via Qiskit), rodando em simuladores gratuitos, sem precisar de hardware quântico real.

O Qubit Invest nasceu como projeto do curso de Data Science da DIO (parceria Accenture), unindo três partes: um agente conversacional (LLM) que interpreta pedidos do usuário e explica resultados em linguagem simples, um módulo de otimização quântica que resolve a alocação de ativos, e uma base de dados com perfil do investidor, ativos, correlações e taxas de mercado.

- **Escopo:** projeto educacional/experimental, não é recomendação financeira formal nem substitui um consultor certificado.


> [!TIP]
> Na pasta [`examples/`](./examples/) você encontra referências de implementação para cada etapa deste desafio.

---

## O Que Você Deve Entregar

### 1. Documentação do Agente

 **O que** o Q faz e **como** ele funciona:

- **Caso de Uso:** Qual problema financeiro ele resolve? 
- **Persona e Tom de Voz:** Como o agente se comporta e se comunica?
- **Arquitetura:** Fluxo de dados e integração com a base de conhecimento
- **Segurança:** Como evitar alucinações e garantir respostas confiáveis?

📄 **Template:** [`docs/01-documentacao-agente.md`](./docs/01-documentacao-agente.md)

---

### 2. Base de Conhecimento

Utilize os **dados mockados** disponíveis na pasta [`data/`](./data/) para alimentar seu agente:

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


Você pode adaptar ou expandir esses dados conforme seu caso de uso.

📄 **Template:** [`docs/02-base-conhecimento.md`](./docs/02-base-conhecimento.md)

---

### 3. Prompts do Agente

Documente os prompts que definem o comportamento do seu agente:

- **System Prompt:** Instruções gerais de comportamento e restrições
- **Exemplos de Interação:** Cenários de uso com entrada e saída esperada
- **Tratamento de Edge Cases:** Como o agente lida com situações limite

📄 **Template:** [`docs/03-prompts.md`](./docs/03-prompts.md)

---

### 4. Aplicação Funcional

Desenvolva um **protótipo funcional** do seu agente:

- Chatbot interativo (sugestão: Streamlit, Gradio ou similar)
- Integração com LLM (via API ou modelo local)
- Conexão com a base de conhecimento

📁 **Pasta:** [`src/`](./src/)

---

### 5. Avaliação e Métricas

Descreva como você avalia a qualidade do seu agente:

**Métricas Sugeridas:**
- Precisão/assertividade das respostas
- Taxa de respostas seguras (sem alucinações)
- Coerência com o perfil do cliente

📄 **Template:** [`docs/04-metricas.md`](./docs/04-metricas.md)

---

### 6. Pitch

Grave um **pitch de 3 minutos** (estilo elevador) apresentando:

- Qual problema seu agente resolve?
- Como ele funciona na prática?
- Por que essa solução é inovadora?

📄 **Template:** [`docs/05-pitch.md`](./docs/05-pitch.md)

---

## Ferramentas Sugeridas

Todas as ferramentas abaixo possuem versões gratuitas:

| Categoria | Ferramentas |
|-----------|-------------|
| **LLMs** | [ChatGPT](https://chat.openai.com/), [Copilot](https://copilot.microsoft.com/), [Gemini](https://gemini.google.com/), [Claude](https://claude.ai/), [Ollama](https://ollama.ai/) |
| **Desenvolvimento** | [Streamlit](https://streamlit.io/), [Gradio](https://www.gradio.app/), [Google Colab](https://colab.research.google.com/) |
| **Orquestração** | [LangChain](https://www.langchain.com/), [LangFlow](https://www.langflow.org/), [CrewAI](https://www.crewai.com/) |
| **Diagramas** | [Mermaid](https://mermaid.js.org/), [Draw.io](https://app.diagrams.net/), [Excalidraw](https://excalidraw.com/) |

---
