# Avaliação e Métricas

## Como Avaliar seu Agente

A avaliação pode ser feita de duas formas complementares:

1. **Testes estruturados:** Você define perguntas e respostas esperadas;
2. **Feedback real:** Pessoas testam o agente e dão notas.

---

## Métricas de Qualidade

| Métrica | O que avalia | Exemplo de teste |
|---------|--------------|------------------|
| **Assertividade** | O agente respondeu o que foi perguntado, com dados corretos da base? | Perguntar o capital disponível e receber o valor correto do perfil_investidor_adaptado.json |
| **Segurança** | 	O agente evitou inventar informações, principalmente na alocação de carteira? | Pedir uma carteira e confirmar que os ativos citados realmente vieram do resultado do QAOA, não inventados. |
| **Coerência** | 	A resposta faz sentido para o perfil do cliente e para os parâmetros de risco escolhidos? | Comparar a alocação com aversão a risco 0.2 vs 0.8 e confirmar que a composição muda de forma coerente (mais renda variável no primeiro caso) |

> [!TIP]
> Ainda não coletei feedback de terceiros — pretendo pedir para 3-5 pessoas testarem o protótipo assim que a fase de testes mais aprofundados começar.

---

## Exemplos de Cenários de Teste


### Teste 1: Consulta de dados do cliente
- **Pergunta:** "Qual meu perfil de investidor?"
- **Resposta esperada:** Valor baseado no perfil_investidor_adaptado.json ("moderado")
- **Resultado:** [x] Correto  [ ] Incorreto

### Teste 2: Recomendação de produto
- **Pergunta:** "Qual a previsão do tempo?"
- **Resposta esperada:** Agente informa que só trata de finanças/investimentos
- **Resultado:** [x] Correto  [ ] Incorreto

### Teste 3: Recomendação de carteira (módulo quântico)
- **Pergunta:** "Monta uma carteira pra mim"
- **Resposta esperada:** Alocação real calculada pelo QAOA em optimization.py, não inventada pelo LLM
- **Resultado:** [x] Correto  [ ] Incorreto

### Teste 4: Explicação de conceito técnico
- **Pergunta:** "O que é QAOA?"
- **Resposta esperada:** Explicação simples baseada no glossario_conceitos.json, sem jargão excessivo
- **Resultado:** [x] Correto  [ ] Incorreto

---

## Resultados

Após os testes, registre suas conclusões:

**O que funcionou bem:**
- A integração entre chat, base de conhecimento e módulo de otimização quântica funcionou de ponta a ponta — o app detecta pedidos de carteira e aciona o QAOA corretamente.
- O agente respondeu com dados reais da base de conhecimento (perfil do cliente, ativos), sem parecer estar inventando informações.
- Os parâmetros ajustáveis na sidebar (quantidade de ativos e aversão a risco) alteraram a alocação retornada, confirmando que o módulo de otimização está sendo usado de fato, e não ignorado.
- Rodar 100% localmente e sem custo (Ollama + Qiskit em simulador) se mostrou viável para um protótipo, mesmo em hardware modesto.

**O que pode melhorar:**
- Tempo de resposta: o QAOA rodando localmente, sem GPU, leva de 1 a 3 minutos por consulta — inviável para uso real, mas aceitável para fins de demonstração/protótipo.
- O modelo LLM local (llama3) é mais lento que uma API paga; para produção seria necessário avaliar um modelo mais leve ou infraestrutura dedicada.
- A detecção de "pedido de carteira" ainda é baseada em palavras-chave simples — pode falhar em frases mais indiretas ou ambíguas.
- Ainda não foram feitos testes estruturados formais (com métricas de assertividade, segurança e coerência) nem testes com terceiros — isso fica para a próxima fase do projeto.

---

## Métricas Avançadas 

Não implementado nesta fase de protótipo. Observação manual identificou que o principal gargalo de latência é o módulo de otimização quântica (QAOA), que responde em 1 a 3 minutos rodando localmente sem GPU. Ferramentas de observabilidade como LangWatch/LangFuse são consideradas para uma fase futura, caso o projeto evolua para uso contínuo.
