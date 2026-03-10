# Bloco de Reflexão — Roteiro 03

## 1. Stubs e Skeletons

Na Tarefa 2, o stub (cliente) serializa os argumentos em JSON, envia pela rede, e deserializa o resultado. O skeleton (servidor) recebe esses bytes, desserializa, executa a função certa e envia de volta. Eles existem porque sem eles voce teria que fazer manualmente toda essa conversa de rede — serializar, descobrir qual função chamar, tratamento de erro. Seria um caos. Com eles, parece que voce tá chamando uma função local.

## 2. REST não é RPC

Na Tarefa 1, eu chamava proxy.calcular("soma", 10, 3) — parecia uma função. Na Tarefa 3, não chamava função; criava um recurso via POST /calculos e depois buscava com GET /calculos/{id}. REST pensa em recursos que voce manipula; RPC pensa em funções que voce invoca. Fielding critica RPC sobre HTTP porque quebra a regra do REST: voce tá fingindo que é uma chamada local quando na verdade tá fazendo rede, escondendo a verdade.

## 3. Evolução de Contrato

Se eu adicionar unidade: string ao RespostaCalculo no .proto, o Protobuf v3 é esperto: clientes antigos ignoram o novo campo automaticamente. No REST não tem schema, então se o servidor muda a resposta, o cliente velho não quebra, mas também não sabe que existe unidade — fica cego. Por isso gRPC é melhor pra evolução em sistemas internos.

## 4. Escolha de Tecnologia

Pra startup com API pública + 10 microserviços internos: REST para a API pública e gRPC internamente.

## 5. Conexão com Labs Anteriores

RPC esconde a rede, que é exatamente o problema. voce pode escrever um loop que chama proxy.calcular() 1 milhão de vezes achando que é rápido, mas tá fazendo 1 milhão de requisições HTTP. Transparência excessiva faz voce tomar decisão de design errada porque não tá vendo o custo real da rede.
