# AWS Monitoramento, auditoria e solução de problemas

- Os serviços para realizar monitoramento, fazer auditoria e identificar problemas são:
  - CloudTrail.
  - X-Ray.
  - CloudWatch.

## Monitoramento

- Já foi visto como distribuir aplicações:
  - De forma segura.
  - Automaticamente.
  - Usando infraestrutura como código.
  - Usando os melhores componentes da AWS.
- Monitoramento interno:
  - Podemos antecipar uma falha.
  - Performance e custo.
  - Trends.
  - Aprender e otimizar.

## Monitoramento na AWS

- AWS CloudWatch:
  - Metrics: Coleta e rastreia Key Metrics.
  - Logs: Coleta, monitora, analisa e armazena arquivos de log.
  - Events: Envia notificações quando certos eventos acontecem no ambiente AWS.
  - Alarmes: Reagem em tempo real a Metrics e Eventos.
- AWS X-Ray:
  - Troubleshooting da aplicação para performance e erros.
  - Rastreamento distribuído de microsserviços.
- AWS CloudTrail:
  - Monitora API Calls.
  - Registra todas as mudanças nos recursos AWS feitas pelos usuários.

## AWS CloudWatch

### AWS CloudWatch - Metrics

- CloudWatch disponibiliza Metrics para todos os serviços AWS.
- Metric é uma variável monitorada. Por exemplo: utilização da CPU, tráfego de rede e etc.
- Metrics pertencem ao namespaces.
- Dimension é um atributo do Metric e pode ser, por exemplo, ID da instância, environment e etc.
- Cada Metric pode ter até 10 Dimension.
- Metrics possuem timestamp.
- Possibilidade de criar um CloudWatch Dashboard com o metrics.

### AWS CloudWatch - EC2 monitoramento detalhado

- Metrics da instância EC2 são capturados por padrão a cada 5 minutos.
- Detailed Monitoring captura os dados a cada minuto.
- Use Detailed Monitoring se quiser uma resposta mais rápida do ASG.
- AWS Free Tier permite que você tenha até 10 Detailed Monitoring Metrics.
- Uso de memória RAM não é enviado para o CloudWatch. Uso da memória RAM deve ser enviado de dentro da instância usando Custom Metric.

### AWS CloudWatch - Custom Metrics

- Possibilidade de definir e enviar seu próprio Custom Metric para o CloudWatch.
- Habilidade de usar Dimensions (attributes) para segmentar Metrics.
- Metrics Resolution:
  - Standard: 1 minuto.
  - High Resolution: Até 1 segundo (utiliza StorageResolution API parameter e tem alto custo).
- Uso de API Call PutMetricData.
- Uso de Exponential Back Off em caso de Throttle Errors.

### AWS CloudWatch - Logs

- Aplicação pode enviar logs para o CloudWatch usando SDK.
- CloudWatch pode coletar logs de serviços como:
  - Elastic BeanStalk.
  - ECS.
  - AWS Lambda.
  - VPC Flow Logs.
  - API Gateway.
  - CloudTrail.
  - CloudWatch.
  - Route53.
- CloudWatch Logs podem ser enviados para:
  - Batch Exporter para S3 Archival.
  - Stream para ElasticSearch cluster para futura análise.
- CloudWatch Logs podem usar Filter Expressions.
- Arquitetura de armazenamento de logs:
  - Log Groups: Normalmente representa uma aplicação.
  - log Stream: Instância dentro da aplicação, log files, container e etc.
- Pode-se definir Log Expiration Policies com expiração em 30 dias ou nunca.
- Usando AWS CLI podemos filtrar CloudWatch Logs.
- Para enviar logs para o CloudWatch, verifique as permissões IAM estão corretas.
- Segurança: Criptografia dos logs ocorre utilizando KMS a nível de Group Logs.

### AWS CloudWatch - Events

- Schedule: Cron job.
- Event Pattern: Event Rules para reagir para um serviço fazendo algo.
- Trigger para Lamba Function e mensagens SQS, SNS e Kinesis.
- CloudWatch Event cria um arquivo JSON para dar informações sobre a mudança.

### AWS CloudWatch - Alarms

- Alarmes são usados para acionar notificações para qualquer Metrics.
- Alarmes podem ser enviados para Auto Scaling, EC2 Actions, SNS notifications.
- Várias opções para alarmar, como por exemplo: sampling, min, max e etc.
- Alarm States:
  - OK.
  - INSUFFICIENT_DATA.
  - ALARM.
- Period:
  - Length of Time em segundos para capturar Metric.
  - High Resolution Custom Metric: Pode escolher somente entre 10 segundos ou 30 segundos.

## AWS X-Ray

- Debugging em produção:
  - Teste localmente.
  - Adiciona log statements em todos os lugares.
  - Re-deploy em produção.
- Formato dos logs é diferente entre as plataformas usando CloudWatch dificultando a análise.
- Debugging.
- Sem visão comum de toda a arquitetura.
- Vantagens:
  - Troubleshooting Performance para identificar gargalos.
  - Facilita entendimento de dependencies em arquitetura de microsserviços.
  - Identificar problemas com PinPoint Service.
  - Revisar comportamento das requisições.
  - Encontrar error e exceções.
  - Identificar usuários impactados.
- Compatibilidade:
  - AWS Lambda.
  - Elastic BeanStalk.
  - ECS.
  - ELB.
  - API Gateway.
  - Instâncias EC2 ou qualquer outro servidor, inclusive On Premise.
- Utiliza Tracing:
  - Tracing é uma forma de monitorar um Request de um ponto ao outro.
  - Cada componente que trata um Request adiciona seu próprio Trace.
  - Trace é feito de segmentos e sub segmentos.
  - Annotations pode adicionar Traces para incluir informações extras.
  - Capacidade Trace:
    - A cada Request.
    - Sample Request podendo ser por porcentagem, taxa por minuto entre outros.
  - Segurança no X-Ray:
    - Autorização IAM.
    - KMS para criptografia em repouso.
- Como habilitar o X-Ray:
  - No código da aplicação deve-se importar o AWS X-Ray SDK.
    - Haverá pouca mudança no código.
    - O SDK irá capturar na aplicação:
      - Chamadas para serviços AWS.
      - Request HTTP/HTTPS.
      - Database Calls.
      - Queue Calls.
- X-Ray troubleshooting:
  - Se o X-Ray não estiver funcionando na instância EC2:
    - Verificar se a EC2 IAM Role tem a permissão correta.
    - Verificar se a EC2 está rodando o X-Ray Daemon.
  - Se for problema de funcionamento com AWS Lambda:
    - Verifique se tem IAM execution role com a Policy correta (AWSX-RayWriteOnlyAccess).
    - Verifique se X-Ray foi importado no código.

## AWS CloudTrail

- Governança, Conformidade e Auditoria para sua conta AWS.
- CloudTrail é habilitado por padrão.
- Histórico de eventos e API Calls feitos na conta AWS podem ser recuperados utilizando:
  - Console.
  - SDK.
  - CLI.
  - AWS Services.
- CloudTrail pode enviar logs para CloudWatch logs.
- Se um recurso é deletado na AWS, procure o motivo no CloudTrail primeiro.
