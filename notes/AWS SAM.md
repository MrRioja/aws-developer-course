# AWS SAM (Serverless Application Model)

- Framework para desenvolver e publicar aplicações serverless.
- Configuração no formato YAML.
- Gera complexos CloudFormation de um simples arquivo SAM YAML.
- Suporta qualquer coisa do CloudFormation como por exemplo: Outputs, Mappings, Parameters, Resources, entre outros.
- Somente dois comando para publicar na AWS.
- SAM pode usar CodeDeploy para fazer deploy de Lambda Functions.
- SAM pode ajudar você a rodar Lambda, API Gateway, DynamoDB localmente.
- Transform Header indica que é um template SAM. Possui o formato do exemplo abaixo:
  - Transform: 'AWS::Serverless-2022-11-20'.
- Write Code:
  - AWS::Serverless::Function (Lambda).
  - AWS::Serverless::Api (API Gateway).
  - AWS::Serverless::SimpleTable (DynamoDB).
- Package e Deploy:
  - Para criar packages podemos utilizar aws cloudformation package ou sam package.
  - Para criar packages podemos utilizar aws cloudformation deploy ou sam deploy.
