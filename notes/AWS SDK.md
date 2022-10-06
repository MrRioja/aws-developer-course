# AWS SDK

- Usado para acessar a CLI de dentro de uma aplicação.
- SDK significa Software Development Kit.
- AWS oferece SDK's para:
  - Java.
  - .NET.
  - PHP.
  - NodeJS.
  - Javascript.
  - Python (boto3/botocore).
  - GO.
  - Ruby.
  - C++.
- Precisamos usar SDK quando programamos serviços AWS.
- AWS CLI usa Python SDK (boto3).
- Se não especificarmos Region a SDK usará us-east-1 como padrão.

## AWS SDK - Segurança da credential

- É recomendado usar Default Credential Provider Chain.
- Default Credential Provider Chain nos bastidores com:
  - Credenciais AWS locais em computadores pessoais ou servidores próprios (`~/.aws/credentials`).
  - Para EC2 usar Instance Profile Credentials usando IAM Role.
  - Environment Variables:
    - AWS_ACCESS_KEY_ID.
    - AWS_SECRET_ACCESS_KEY.
- Melhores práticas recomendam o uso de IAM Role quando estiver trabalhando com serviços AWS.

## AWS SDK - Exponential Backoff

- Quando chamadas a API falham devido a excesso de chamadas, devemos usar Exponential Backoff.
- Isso aplica-se para Rate Limited API.
- Pode ser feito usando chamadas a API com as SDKs.
- As tentativas ocorrem da seguinte maneira:
  - 1 falhou espera 1 UT (Unidade de tempo).
  - 2 falhou espera 2 UT (Unidade de tempo).
  - 3 falhou espera 4 UT (Unidade de tempo).
  - 4 falhou espera 8 UT (Unidade de tempo).
