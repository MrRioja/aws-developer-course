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
