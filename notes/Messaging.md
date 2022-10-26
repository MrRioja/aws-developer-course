# Messaging

- Aplicações precisam comunicar com outras aplicações.
- Existem duas formas de realizar essa comunicação:
  - Síncrona:
    - Pode ser problemática entre aplicações quando ocorrem picos de tráfego.
  - Assíncrona.
- Métodos para dissociar a aplicação, sendo que todos podem escalonar independente da aplicação:
  - Usando SQS: Modelo Queue.
  - Usando SNS: Modelo pub/sub.
  - Usando Kinesis: Modelo de streaming em tempo real.

## AWS SQS

## AWS SQS - Standard Queue

- Gerenciado pela AWS.
- Escalona de 1 à 10.000 mensagens por segundo.
- Default Retention of Message é de 4 dias e extensível à 14 dias.
- Sem limites de quantas mensagens podem ficar na queue.
- Latência menor que 10ms.
- Escalonamento horizontal de consumidores.
- Podem haver mensagens duplicadas.
- Pode haver consumo de mensagens fora de ordem (Best effort ordering).
- Tamanho máximo da mensagem é de 256KB.

## AWS SQS - Delay Queue

- Adiciona delay a uma mensagem para que os consumidores não a vejam imediatamente.
- O delay máximo é de 15 minutos.
- Default é 0 segundos.
- Default pode ser ajustado no queue level.
- Default pode ser alterado usando DelaySeconds Parameters.

## AWS SQS - Produzindo mensagens

- Definir um body.
- Adicionar Messages Attributes (metadata é opcional).
- Opção de Delay Delivery.
- Ao produzir uma mensagem será retornado:
  - Message Identified.
  - MD5 hash do body.

## AWS SQS - Consumindo mensagens

- Poll SQS para mensagens aonde o consumidor pode receber no máximo 10 mensagens por vez.
- Consumidor deve processar as mensagens dentro do Visibility Timeout.
- Após o consumo, as mensagens devem ser deletadas usando Message ID e Receipt Handle.

## AWS SQS - Visibility Timeout

- Quando o consumidor requisita uma mensagem da fila, a mensagem ficará invisível para outros consumidores por um período de tempo definido, o chamado Visibility Timeout, que é:
  - Ajustável de 0 segundos a 12 horas, sendo o padrão 30 segundos.
  - Se o valor for muito alto (10 minutos) e o consumidor falhar ao processar a mensagem, será necessário esperar muito tempo até que ela esteja disponível novamente para um reprocessamento.
  - Se o valor for muito baixo (1 segundo) e o consumidor precisar de mais tempo para processar a mensagem, outro consumidor irá receber a mesma mensagem fazendo com que ela possa ser processada duas vezes.
- Temos o ChangeVisibility API para mudar a visibilidade enquanto processa a mensagem.
- Temos o DeleteMessage API para avisar para o SQS que a mensagem foi processada.

## AWS SQS - Dead Letter Queue

- Se o consumidor falhar ao processar a mensagem dentro do Visibility Timeout, a mensagem volta para a fila.
- Podemos escolher quantas vezes uma mensagem retorna para a fila, a chamada Redrive policy.
- Quando esse número é atingido, a mensagem vai para a Dead Letter Queue (DQL).
- Devemos criar a DQL e apontar para o SQS queue para ela.
- Devemos processar as mensagens na DQL antes que ela expire.

## AWS SQS - Long Polling

- Quando o consumidor fizer uma requisição para a fila, podemos configurar uma espera por mensagens caso ela esteja vazia. Essa técnica é chamada de Long Pooling.
- O Long Pooling diminui o número de chamadas a SQS API e aumenta a eficiência da aplicação.
- O Wait Time pode ser ajustado entre 1 e 20 segundos, que é a recomendação na maioria dos casos.
- Long Pooling pode ser habilitado no nível Queue ou nível API usando WaitTimeSeconds.

## AWS SQS - FIFO Queue

- First In - First Out, primeiro a entrar é o primeiro a sair, disponível a depender da região.
- Nome do Queue deve terminar com `.fifo`.
- Baixo Throughout (até 3.000 por segundo com Batching e 300 por segundo sem Batching).
- Mensagens são processadas em ordem pelos consumidores.
- Mensagens são enviadas somente uma vez.
- Sem Delay por mensagem, somente por queue.
- Possibilidade de fazer de-duplication usando "Duplication ID".
- Message Groups:
  - Possibilita agrupar mensagens por FIFO ordering usando Message GroupID.
  - Somente um Worker pode ser designado por Message Group para que as mensagens sejam processadas em ordem.
  - Message Group é somente uma Tag extra na mensagem.

## AWS SQS - Extended Client

- Limite da mensagem 256KB.
- Usando SQS Extended Client (Java Library).

## AWS SQS - Segurança

- Criptografia em trânsito usando HTTPS endpoint.
- Pode usar Server Side Encryption (SSE) usando KMS.
- Pode usar CMK (Custom Master Key).
- Pode ajustar Data Key Reuse Period entre 1 minuto e 24 horas.
  - Com o Data Key Reuse Period baixo a KMS API será usada com frequência.
  - Com o Data Key Reuse Period alto a KMS API será usada com menor frequência.
- O Server Side Encryption criptografa somente o body da mensagem e não seu metadata (message ID, timestamp, attributes).
- IAM Policy deve permitir o uso do SQS.
- SQS Queue Access Policy:
  - Controle granular sobre IP.
  - Controle sobre o tempo de requisição.
- Nenhum VPC endpoint deve ter acesso a internet para acessar o SQS.

## AWS SQS API - Importante saber

- CreateQueue.
- DeleteQueue.
- PurgeQueue.
- SendMessage.
- ReceiveMessage.
- DeleteMessage.
- ChangeMessageVisibility.
- Batch API ajuda a reduzir custos e podemos usar para:
  - SendMessage.
  - DeleteMessage.
  - ChangeMessageVisibility.
