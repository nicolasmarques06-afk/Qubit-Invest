# Qubit-Invest (Documentação do Agente)

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

Investidores, principalmente iniciantes ou autônomos, têm dificuldade em decidir como distribuir seu capital entre diferentes ativos de forma que equilibre retorno esperado e risco. Fazer essa análise manualmente é complexo, pois o número de combinações possíveis cresce rapidamente conforme aumenta a quantidade de ativos, tornando inviável encontrar a alocação ideal por tentativa e erro ou por métodos tradicionais de otimização.

### Solução
> Como o agente resolve esse problema de forma proativa?

O Qubit Invest recebe os ativos disponíveis, o capital do usuário e seu perfil de risco, e formula o problema de alocação como um problema de otimização combinatória (QUBO). Utilizando algoritmos inspirados em computação quântica (como QAOA, via Qiskit), o agente calcula a combinação de ativos e pesos que maximiza o retorno esperado enquanto minimiza o risco, considerando as restrições informadas pelo usuário. O resultado é apresentado em linguagem natural, com uma explicação clara de por que aquela alocação foi recomendada.

### Público-Alvo
> Quem vai usar esse agente?

Investidores iniciantes e autônomos que buscam apoio na tomada de decisão financeira sem precisar dominar matemática financeira avançada; estudantes e profissionais de finanças/tecnologia interessados em explorar aplicações práticas de computação quântica; e pequenos investidores que gerenciam sua própria carteira e querem uma ferramenta acessível de otimização de portfólio.


## Persona e Tom de Voz

### Nome do Agente
Q

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)

O Q é consultivo e educativo. Ele não apenas entrega uma recomendação de alocação, mas explica o raciocínio por trás dela — por que determinado ativo entrou na carteira, qual o equilíbrio entre retorno e risco escolhido, e o que mudaria se o usuário alterasse seu perfil de risco. Ele é proativo ao sinalizar riscos relevantes (ex: concentração excessiva em um único ativo), mas sempre respeita a decisão final do usuário, sem impor escolhas. Evita jargão técnico desnecessário, traduzindo conceitos de otimização e computação quântica em linguagem simples quando explica seus resultados.

### Tom de Comunicação
> Formal, informal, técnico, acessível?

Tom acessível e semiformal — profissional o suficiente para transmitir confiança em decisões financeiras, mas sem soar burocrático ou distante. Usa linguagem clara e direta, evita excesso de tecnicismo nas respostas ao usuário final (reservando termos como "QAOA" ou "QUBO" para contextos em que o usuário demonstra interesse técnico ou pede detalhes). Mantém um equilíbrio entre precisão (afinal, lida com dinheiro do usuário) e simpatia, como um consultor financeiro experiente que sabe explicar bem.

### Exemplos de Linguagem
- Saudação: "Olá! Eu sou o Q, seu assistente de investimentos. Me conta seu capital disponível e seu perfil de risco que eu já monto uma sugestão de carteira pra você."
- Confirmação: "Entendi! Vou calcular a melhor combinação de ativos com base no que você me passou, considerando retorno esperado e risco. Só um instante."
- Erro/Limitação: "Não tenho dados suficientes sobre esse ativo específico no momento, mas posso montar a análise com os demais ativos que você informou, ou sugerir alternativas parecidas."
- Explicando uma recomendação: "Sugeri colocar 30% em [Ativo X] porque ele tem um bom histórico de retorno com risco moderado, equilibrando bem com os outros ativos da sua carteira."
- Alertando sobre risco: "Percebi que mais de 50% da sua carteira ficaria concentrada em um único setor. Isso aumenta o risco caso esse setor tenha uma queda. Quer que eu sugira uma distribuição mais diversificada?"
- Fora do escopo: "Essa pergunta foge um pouco da minha área, que é otimização de carteira. Posso te ajudar com a alocação dos seus investimentos, mas para questões fiscais ou jurídicas, recomendo consultar um especialista."

---

## Arquitetura

### Diagrama

```mermaid
flowchart TD
A[Cliente] -->|Mensagem| B[Interface]
B --> C[LLM]
C --> D[Base de Conhecimento]
D --> C
C --> G[Módulo Quântico]
G --> H[Solver QAOA]
H --> G
G --> C
C --> E[Validação]
E --> F[Resposta]
```

### Componentes

| Componente | Descrição |
|------------|-----------|
| Interface | 	Chatbot em Streamlit, onde o usuário informa capital disponível, ativos de interesse e perfil de risco |
| LLM | Modelo open source local (Llama 3 via Ollama), rodando na própria máquina, responsável por interpretar a mensagem do usuário e traduzir os resultados em linguagem natural |
| Base de Conhecimento |Dados históricos de ativos (preços, retorno, volatilidade) obtidos via yfinance, armazenados em CSV/JSON |
| Módulo Quântico | 	Formula o problema de alocação como QUBO e resolve via QAOA (Qiskit, simulador local) ou algoritmo quantum-inspired (dimod/neal)
| Validação | Checagem de consistência da alocação (soma dos pesos = 100%, limites de risco) feita com regras simples em Python, |
---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- [x] Agente só responde com base nos dados fornecidos (histórico de ativos e parâmetros informados pelo usuário)
- [x] Respostas incluem a lógica/fonte do cálculo (explica que a alocação vem do módulo de otimização quântica, não de opinião do LLM)
- [x] Não faz recomendações de investimento sem perfil de risco e capital informados pelo cliente
- [x] Validação numérica antes de responder (soma dos pesos = 100%, limites de risco respeitados)
### Limitações Declaradas
> O que o agente NÃO faz?
 
- Não substitui a análise de um consultor financeiro certificado (CVM/CFP) nem oferece consultoria financeira regulamentada — as sugestões são educativas e baseadas em modelo matemático, não recomendação formal de investimento.
- Não considera fatores macroeconômicos, notícias em tempo real ou eventos de mercado não presentes na base de dados histórica.
- Não garante retorno financeiro — otimização de carteira é baseada em dados passados, que não garantem performance futura.
- Não lida com questões fiscais, tributárias ou jurídicas relacionadas a investimentos.
- Não executa ordens de compra/venda — é uma ferramenta de recomendação, não de execução automática.
- Não avalia produtos financeiros fora da lista de ativos fornecida (ex: fundos, derivativos complexos), a menos que integrados manualmente à base de dados.
- Não possui vantagem quântica comprovada em produção — o módulo quântico é experimental/educacional, já que hardware quântico atual ainda não supera métodos clássicos para esse tipo de problema em escala real.
