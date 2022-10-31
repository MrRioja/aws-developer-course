# AWS Serverless

- É um novo paradigma em que os desenvolvedores não precisam gerenciar servidores.
- Serverless começou com Lambda Function mas hoje possui serviços para banco de dados, mensagens, armazenamento e outros.

## Lambda

### Benefícios de usar Lambda

- Preço simplificado:
  - Cobrado por requisição e tempo de processamento.
  - Free tier inclui 1 milhão de requisições e 400 mil GB/s de tempo de processamento.
  - Integração com diversos serviços.
  - Integração com diversas linguagens de programação.
  - Monitoramento simplificado usando CloudWatch.
  - Até 3GB de RAM para executar suas funções.
  - Aumento da RAM também aumenta performance da CPU e rede.

### Lambda - Configuração

- Timeout: Default é 3 segundos e o máximo 15 minutos.
- Environment Variables.
- Memória alocada é de 128MB até 3GB.
- Pode ser colocada dentro de um VPC + Security Groups.
- Deve ser associada a um IAM Role.

### Lambda - Concurrency e Throttling

- Concurrency: Até 1.000 execuções podem acontecer ao mesmo tempo. Pode ser aumentado através de ticket de suporte AWS.
- Possível ajustar "reserved concurrency".
- Cada chamada além do Concurrency Limit dispara um Throttle.
- Comportamento do Throttle:
  - Synchronous Invocation: Retorna ThrottleError 429.
  - Asynchronous Invocation: Tenta novamente e depois envia para DLQ.

### Lambda - Retries e DLQ

- Se a função invocada de forma assíncrona falhar, será feito uma nova tentativa.
- Depois da segunda tentativa, será enviado para Dead Letter Queue.
- DLQ pode ser SNS Topic ou SQS Queue.
- O payload original é enviado para DLQ.
- Essa é a forma eficiente de detectar erros com sua função em produção sem alterar o código.
- Verifique se IAM execution Role está correta.

### Lambda - Logging, Monitoring e Tracing

- CloudWatch:
  - AWS Lambda logs são armazenadas no AWS CloudWatch Logs.
  - AWS Lambda Metrics são exibidos no AWS CloudWatch Metrics.
  - Verificar se a função Lambda tem IAM Role com Policy que permite escrever no CloudWatch.
- X-Ray:
  - É possível usar Trace com X-Ray.
  - Habilitado na configuração Lambda (Executa X-Ray Daemon).
  - Utilize AWS SDK no seu código.
  - Certifique-se que a Lambda Function tem a correta IAM execution Role.

### Lambda - Limites

- Execução:
  - Memory Allocation: De 128MB à 3008MB com incrementos de 64MB.
  - Tempo máximo de execução: 15 minutos.
  - Capacidade do disco (pasta `/temp`) 512MB.
  - Execuções simultâneas: 1.000.
- Deployment:
  - Tamanho do arquivo Lambda Function: 50MB no formato `.zip`.
  - Código + dependências não podem ultrapassar 250MB.
  - Pode utilizar a pasta `/temp` para carregar outros arquivos no startup.
  - Tamanho das Environment Variables: 4KB.
