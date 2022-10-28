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

## AWS SQS - Simple Queue Service

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

## AWS SNS - Simple Notification Service

- Producer somente envia mensagem para 1 SNS Topic.
- Podem existir até 10.000.000 de subscribers por Topic.
- Os subscribers irão receber todas as mensagens enviadas.
- Limite de 100.000 Topics.
- Subscribers podem ser:
  - SQS.
  - Lambda.
  - HTTP/HTTPS.
  - Emails.
  - SMS.
  - Mobile notifications.

## AWS SNS - Como publicar

- Topic Publish (com seu servidor AWS, usando SDK):
  - Criar Topic.
  - Criar Subscription.
  - Publish para o Topic.
- Direct Publish (para mobile apps SDK):
- Criar a plataforma da aplicação.
- Criar a plataforma endpoint.
- Publicar para a plataforma EndPoint.
- Funciona com Google GCM, Apple APNS, Amazon ADM e outros.

## AWS SNS - Fan Out (SNS + SQS)

- Publique uma vez usando SNS e receba em vários SQS.
- Totalmente dissociado.
- Sem perda de dados.
- Possibilidade de adicionar subscriber a qualquer momento.
- SQS permite processamento retardado.
- SQS permite retries of work.
- Podem ter vários workers em um queue e só um worker em outra queue.

## Kinesis

- Kinesis é uma alternativa ao Apache Kafka.
- Ideal para log de aplicação, metrics, IoT, clickStream.
- Ideal para real-time big data.
- Ideal para Streaming Processing Frameworks (spark, NiFi).
- Dados são automaticamente replicados para 3 AZs.
- Kinesis Streams: Baixa latência, escalonável.
  - Stream são divididos em Shards/Partitions.
  - Retenção dos dados é de 1 dia e pode ser estendido até 7 dias.
  - Habilidade de reprocessar / replay data.
  - Múltiplas aplicações podem acessar o mesmo stream.
  - Processamento em tempo real e throughput escalonável.
  - Uma vez que os dados são inseridos no Kinesis, eles não podem ser apagados (Immutability).
  - Shards:
    - 1 stream pode ser constituído por 1 ou mais shards.
    - Capacidade por shard: 1MB/s ou 1.000 mensagens/s para escrita e 2MB/s para leitura.
    - Cobrado por Shard provisionado. Pode ter quantos Shards forem necessários.
    - Bathing ou por mensagem.
    - Número de Shards pode ser ajustável conforme necessidade.
    - Records são ordenados por Shard.
- Kinesis Analytics: Para análise em tempo real de streams usando SQL.
- Kinesis Firehose: Carrega o stream no S3, Redshift, ElasticSearch.

## AWS Kinesis API - Put Records

- PutRecord API + Partition Key é hashed para determinar Shard ID.
- Mesma key vai para a mesma partition.
- Mensagens enviadas recebem um número sequencial.
- Escolha um Partition Key que seja distribuída para prevenir hot partition.
- Use user_id se tiver vários usuários.
- Não use estado_id se 90% dos usuários são do mesmo estado.
- Use bathing com PutRecords para reduzir custo e aumentar throughput.
- ProvisionedThroughoutExceeded é disparado se consumir acima do limite.
- Podemos usar CLI, AWS SDK ou Producer Libraries de vários frameworks.

## AWS Kinesis API - Consumers

- Pode ser um consumidor normal (CLI, SDK, etc).
- Pode usar Kinesis Client Library:
  - KCL usa DynamoDB para fazer checkpoint offsets.
  - KCL usa DynamoDB para rastrear outros workers e dividir o trabalho entre os Shards.

## AWS Kinesis API - Security

- Controle de acesso/autorização usando IAM Policies.
- Criptografia em trânsito usando HTTPS endpoints.
- Criptografia em repouso usando KMS.
- Possibilidade de criptografar e descriptografar dados usando client side (mais complicado).
- VPC endpoints disponível para Kinesis para acesso dentro do VPC.

## AWS Kinesis Data Analytics

- Análise em tempo real no Kinesis Streams usando SQL.
- Kinesis Data Analytics:
  - Auto scaling.
  - Managed: Não é necessário provisionar servidores.
  - Continuos: Tempo real.
- Cobrado somente pelo que é usado.
- Pode ciar Streams a partir de queries em tempo real.

## AWS Kinesis Firehose

- Gerenciado pela AWS.
- Quase em tempo real (60ms de latência).
- Carrega dados dentro do Redshift, Amazon S3, ElasticSearch, Splunk.
- Escalonamento automático.
- Suporta vários formatos de dados (cobrado por conversão).
- Cobrado pela quantidade de dados que passam pelo Firehose.
