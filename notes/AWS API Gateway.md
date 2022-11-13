# API Gateway

- AWS Lambda + API Gateway: Sem infraestrutura para gerenciar.
- Há versionamento da API.
- Há possibilidade de criação de vários ambientes. Exemplo: dev, prod, test.
- Gerencia segurança: autenticação e autorização.
- Cria API Key e gerencia Request Throttling.
- Importação do Swagger/Open API para definição rápida de APIs.
- Transforma e valida Requests e Responses.
- Gera SDK e especificação da API.
- É possível criar cache de Responses da API.

## API Gateway - Integração

- Fora do VPC:
  - AWS Lambda (mais comum).
  - Endpoint na EC2.
  - Load Balancers.
  - Qualquer outro serviço AWS.
  - Acesso publico através de HTTP.
- Dentro do VPC:
  - AWS Lambda no mesmo VPC.
  - EC2 endpoint no mesmo VPC.

## API Gateway - Deployment Stages

- Alterar uma API e salvar não significa que as alterações entrarão em vigor.
- É preciso fazer deploy para alterações terem efeito.
- Mudanças são deployed para Stages (quantos Stages forem necessários e alguns exemplos são Stages de dev, test e prod).
- Cada Stage tem sua própria configuração.
- É possível usar Rollback nos stages.

## API Gateway - Stage Variables

- Stage Variables são como environment variables para API Gateway.
- Use para mudanças frequentes na configuração da API.
- Podem ser usados para:
  - Lambda Function ARN.
  - HTTP endpoint.
  - Parameter Mapping Templates.

## API Gateway - Canary Deployment

- Possível habilitar Canary Deployment para qualquer Stage.
- Escolha o percentual de tráfego para cada API.

## API Gateway - Mapping Template

- Mapping Template pode ser usado para modificar Request e Response.
- Renomear parâmetros.
- Modificar body content.
- Adicionar headers.
- Mapear JSON para XML para enviar para o backend ou para o cliente.
- Use **Velocity Template Language (VTL)** para realizar loops, ifs e etc.
- Filtrar resultado da saída e remover dados desnecessários.

## API Gateway - Swagger e Open API spec

- Forma comum de definir REST API usando definição da API como código.
- Importa Swagger/OpenAPI 3.0 spec para API Gateway:
  - Method.
  - Method Request.
  - Integration Request.
  - Method Response.
  - +AWS extensions para API Gateway e configuração de cada opção.
- Pode exportar API como Swagger/OpenAPI spec.
- usando Swagger podemos gerar SDK para nossa aplicação.

## API Gateway - Caching API Response

- Caching reduz o numero de chamadas feitas para o backend.
- Default time TTL (time to live) é de 300 segundos. Sendo o ,mínimo 0s e máximo de 3.600s.
- Cache são definidos por Stage.
- Opção de criptografia.
- Capacidade do cache entre 0.5GB e 237GB.
- Possibilidade de alterar a configuração do cache para um método especifico.
- Habilidade de limpar o cache inteiro rapidamente.
- Clientes com autorização correta no IAM podem invalidar o cache com o header: `CacheControl:max-age=0`.

## API Gateway - Logging, Monitoring e Tracing

- CloudWatch Logs:
  - Habilita CloudWatch logging no nível Stage com Log Level.
  - Capacidade de alterar as configurações por API. Exemplo: ERROR, DEBUG, INFO.
  - Log contém informações sobre Request/Response body.
- CloudWatch Metrics:
  - Por Stage.
  - Possibilidade de habilitar Detailed Metrics.
- X-Ray:
  - Habilitar Tracing para pegar informação extra sobre Requests no API Gateway.
  - X-Ray API Gateway + AWS Lambda oferecem uma solução completa para monitoramento.

## API Gateway - CORS

- CORS (Cross Origin Resource Sharing) deve ser habilitado quando API recebe chamadas de outro domínio.
- OPTIONS Pre-Flight Request deve conter os seguintes headers:
  - Access-Control-Allow-Method.
  - Access-Control-Allow-Headers.
  - Access-Control-Allow-Origin.
- CORS pode ser habilitado pelo console AWS.

## API Gateway - Usage Plans e API Keys

- Possibilidade de limitar o usa da API.
- Usage Plans:
  - Throttling: Configura a capacidade normal e Burst.
  - Quotas: Número de requests por dia/semana/mês.
  - Associado com API Stages.
- API Keys:
  - Gera uma por cliente.
  - Associado com Usage Plans.
- Possibilidade de rastrear o uso da API Key.

## API Gateway - Segurança

- Permissão IAM:
  - Criar IAM Policy com a devida autorização e anexar ao Role/User.
  - API Gateway verifica permissão IAM passada durante a chamada.
  - Ideal para fornecer acesso dentro da infraestrutura AWS.
  - Utiliza a capacidade **Sig v4** onde as credenciais IAM estão no Header.
  - Gerencia autenticação e autorização.
- Lambda Authorizer, conhecido também como Custom Authorizers:
  - Usa AWS Lambda para validar o token no header que está sendo passado.
  - Opção para Cache do resultado da autenticação.
  - Lambda deve retornar uma IAM Policy para o usuário.
  - Ideal para token de aplicações de terceiros.
  - Gerencia autenticação e autorização.
  - Cobrado por chamada da Lambda Function.
- Cognito User Pool:
  - Cognito gerencia User Lifecycle.
  - API Gateway verifica a identidade automaticamente do AWS Cognito.
  - Gerenciado e podendo usar conta do Facebook, Google, Amazon, etc para autenticação.
  - Não é necessário nenhuma implementação.
  - Deve-se implementar autorização no backend.
  - Cognito só trabalha na autenticação, não na autorização.
