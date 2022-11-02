# DynamoDB Serverless

## Bancos de dados NoSQL

- NoSQL significa Not Only SQL.
- São banco de dados não relacionais.
- São distribuídos.
- Não suporta Join.
- Todos os dados usados no Query ficam em uma linha.
- Não fazem Aggregation, como por exemplo **SUM**.
- Escalonamento horizontal.

## DynamoDB

- Gerenciado pela AWS com réplicas em 3 AZ's garantindo alta disponibilidade.
- É um banco de dados NoSQL.
- Escalonável para quantidade muito grande de dados pois é um banco de dados distribuído.
- Milhões de requisições por segundo, trilhões de linhas, centenas de terabytes de armazenamento.
- Rápido e consistente em performance (baixa latência).
- Integrado com IAM para segurança, autorização e administração.
- Event Driven Programming com DynamoDB Streams.
- Baixo custo.
- Escalonamento automático.
- DynamoDB é feito de tabelas.
- Primary Key é obrigatório e deve ser definido no momento em que criamos a tabela.
- Uma tabela pode ter infinitos itens (equivalente a rows nos bancos relacionais).
- Cada item tem Attributes, podendo ser adicionados a qualquer momento ou ser nulo.
- Tamanho máximo do item é de 400KB.
- Tipos de dados suportados:
  - Scalar types: String, Number, Binary, Boolean, null.
  - Document type: List, Map.
  - Set types: String set, Number set, Binary set.

## DynamoDB - Primary Key

- Opção 1: Somente Partition Key (hash):
  - Partition Key deve ser única para cada item.
  - Partition Key deve ser diversificada para os dados serem distribuídos.
- Opção 2: Partition Key + Sort Key:
  - A combinação deve ser única.
  - Dados são agrupados pela Partition Key.
  - Sort Key == Range Key.

## DynamoDB - Provisioned Throughput

- Tabela deve ter leitura e escrita provisionadas.
- Read Capacity Units (RCU).
- White Capacity Units (WCU).
- Opção de configurar auto-scaling para throughput.
- Throughput pode temporariamente exceder o limite estabelecido usando 'burst credit'.
- Se o burst credit acabar, você receberá um **ProvisionedThroughputException**.
- É recomendado fazer um Exponential Back-Off Retry.

## DynamoDB - Write Capacity Units (WCU)

- 1 Write Capacity Unit (WCU) representa uma escrita por segundo para 1 item até 1KB em tamanho.
- Se o item for maior que 1KB, mais WCU será consumido.
- Exemplos de cálculos:
  - Escrever 10 itens de 2KB por segundo = 20 WCU (10 \* 2 = 20).
  - Escrever 6 itens de 4.5KB por segundo = 30 WCU (6 \* 5 = 30).
  - Escrever 120 itens de 2KB por minuto = 4 WCU ((2 \* 120) / 60 = 4).

## DynamoDB - Read Capacity Units (RCU)

- 1 Read Capacity Unit (RCU) representa 1 Strongly Consistent Read por segundo ou 2 Eventually Consistent Reads por segundo para um item até 4KB.
- Se o item for maior que 4KB, mais RCU serão consumidos.
- Exemplos de Cálculos:
  - 10 Strongly Consistent Read por segundo de 4KB = 10 RCU ((10 \* 4) / 4 = 10).
  - 16 Eventually Consistent Read por segundo de 12KB = 24 RCU ((16/2) \* (12/4) = 24).
  - 10 Strongly Consistent Read por segundo de 6KB = 20 RCU ((10 \* 8)/4 = 20).

## DynamoDB - Strongly Consistent Read VS Eventually Consistent Read

- Eventually Consistent Read: Se lermos um item logo após uma escrita, podemos receber seu valor anterior devido a replicação do dado.
- Strongly Consistence Read: Se lermos um item depois de uma escrita, leremos o valor atualizado.
- Por padrão o DynamoDB usa Eventually Consistence Reads, porém os métodos GetItem, Query e Scan possuem um parâmetro chamado ConsistentRead que pode ser modificado para True.

## DynamoDB - Partitions Internal

- Dados são divididos em partitions.
- Partition Key passa por um algoritmo para saber para qual Partition deve ir.
- Para computar o número de partições:
  - Capacidade: (Total RCU / 3000) + (Total de WCU / 1000).
  - Tamanho: Tamanho total / 10GB.
  - Número de partições: CEILING(MAX(Capacity, Size)).
- WCU e RCU são distribuídos igualmente entre as partições.

## DynamoDB - Throttling

- Se excedemos RCU ou WCU recebemos um ProvisionedThroughputExceededException.
- Possíveis razões:
  - Hot Key: Um Partition Key está sendo acessado muitas vezes.
  - Hot Partition.
  - Itens muito grandes: Pois RCU e WCU dependem do tamanho do item.
- Soluções possíveis:
  - Exponential Back-Off quando uma exceção é encontrada (recurso incluso no SDK).
  - Distribuir Partition Keys o máximo possível.
  - Se for problema com RCU, podemos usar o DynamoDB Accelerator (DAX).

## DynamoDB - Escrevendo dados

- PutItem:
  - Escreve dados no DynamoDB, criando ou substituindo.
  - Consome WCU.
- UpdateItem:
  - Atualiza dados no DynamoDB, atualizando parte dos atributos.
  - Possibilidade de usar Atomic Counters e incrementá-los.
- Conditional Writes:
  - Escreve ou atualiza somente se a condição for satisfeita.
  - Ajuda com acesso simultâneo aos itens.
  - Sem impacto na performance.

## DynamoDB - Apagando dados

- DeleteItem:
  - Deleta um item.
  - É possível fazer um DeleteItem usando condicionais.
- DeleteTable:
  - Apaga toda a tabela e seu conteúdo.
  - Mais rápido que DeleteItem para apagar todos os itens.

## DynamoDB - Batching writes

- BatchWhiteItem:
  - Máximo de 25 PutItem ou DeleteItem por chamada.
  - Máximo de 16MB de dados escritos.
  - Máximo de 400KB de dados por item.
- Batching permite reduzir a latência, reduzindo o número de API Calls ao DynamoDB.
- Operações são feitas em paralelo para melhor eficiência.
- É possível que alguns itens do Batch falhem, nesse caso será preciso tentar novamente utilizando o algoritmo Exponential Back-off.

## DynamoDB - Lendo dados

- GetItem:
  - Leitura baseado no Primary Key.
  - Primary Key = Hash ou hash-range.
  - Eventually Consistent Read é o padrão.
  - Opção de usar o Strongly Consistent Read (consome mais RCU e pode demorar mais).
  - ProjectionExpression pode ser especificado para incluir somente certos atributos.
- BatchGetItem:
  - Até 100 itens.
  - Até 16MB.
  - itens são lidos em paralelo para minimizar a latência.

## DynamoDB - Query

- Query retorna itens com base em:
  - Valor do Partition Key usando o operador `=`.
  - Valor do SortKey usando um dos operadores a seguir: `=`, `<`,`<=`,`>`,`>=`,`Between`,`Begin`.
  - FilterExpression pra filtros no lado do cliente.
- Retorna:
  - Até 1MB.
  - Ou número de itens especificado em `Limit`.
- Possível fazer paginação com o resultado.
- Pode-se usar query com Table, Local Secondary Index ou Global Secondary Index.

## DynamoDB - Scan

- Utilizar o Scan com filtros é ineficiente.
- Retorno de até 1MB. Usar paginação.
- Consome muito RCU.
- Pode-se diminuir o impacto usando Limit ou reduzir o tamanho do resultado.
- Para melhor performance, use Parallel Scans:
  - Múltiplas instâncias varrem múltiplas partições ao mesmo tempo.
  - Aumenta o throughput e o RCU consumido.
  - Limitar o impacto do Parallel Scans do mesmo jeito que é feito com Scans.
- Pode-se usar ProjectionExpression + FilterExpression.
