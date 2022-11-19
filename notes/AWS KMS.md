# AWS KMS (Key Management Service)

- KMS é a forma mais popular de criptografar dados na AWS.
- Forma fácil de controlar acesso aos dados.
- Keys gerenciadas pela AWS.
- Totalmente integrado com autorização IAM.
- Funciona nos bastidores para os seguintes serviços AWS:
  - Amazon EBS para criptografia de volumes.
  - Amazon S3 para criptografia de objetos do lado do servidor.
  - Amazon Redshift e RDS para criptografar dados.
  - Amazon SSM para Parameter Store.
- É possível utilizar KMS via CLI e SDK.
- No KMS, a CMK é usada para criptografar dados e nunca pode ser lida pelo usuário. A CMK pode ser alternada para aumentar a segurança.
- KMS pode criptografar no máximo 4KB por chamada.
- Se os dados ultrapassarem o limite deverá ser usado o Envelope Encryption.
- Para dar acesso ao KMS para outras pessoas:
  - Key Policy deve permitir acesso do usuário.
  - IAM Policy deve permitir chamadas a API.
- Gerencia Keys e Policies:
  - Create.
  - Rotation Policies.
  - Disable.
  - Enable.
- Possibilidade de auditar o uso da Key usando CloudTrail.
- Existem 3 tipos de Customer Master Key (CMK):
  - AWS Managed Service Default CMK: Grátis.
  - User Key criada no KMS: $1 por mês.
  - User Key importada (deve ser 256-bit symmetric key): $1 por mês.
  - \+ Custo por API call no KMS: $0.03 por 10.000 calls.