# AWS Cognito

- Serve para dar uma identidade para nossos usuários, dessa forma eles podem interagir com nossa aplicação.
- Cognito User Pools:
  - SignIn para usuários da aplicação.
  - Integrado com API Gateway.
- Cognito Identity Pools:
  - Oferece credenciais AWS para usuários para acesso a recursos AWS diretamente.
  - Integrado com Cognito User Pools com o Identity Provider.
- Cognito Sync:
  - Sincroniza dados do dispositivo com Cognito.

## AWS Cognito - Cognito User Pools (CUP)

- Cria um banco de dados gerenciado pela AWS para ser usado pelo seu aplicativo móvel.
- Simple login: Username ou email e senha.
- Possibilidade de verificar email/telefone e adicionar MFA.
- Pode-se habilitar Federated Identities (Facebook, Google, SAML...).
- Retorna JSON Web Tokens (JWT).
- Pode ser integrado com API Gateway para autenticação.

## AWS Cognito - Federated Identity Pools

- Objetivo:
  - Fornecer ao cliente acesso direto a um recurso AWS.
- Como:
  - Login no Federated Identity Provider ou permanecer como anonimo.
  - Recebe credencias AWS temporárias do Federated Identity Pool.
  - Essas credenciais veem com um IAM Policy com permissões pré definidas.
- Exemplo:
  - Oferece acesso temporário para escrever no S3 bucket usando o login do Facebook.

## AWS Cognito Sync

- Armazena preferencias, configurações e estado do app.
- Sincronização entre vários dispositivos e qualquer plataforma.
- Capacidade de sincronização offline (sincroniza quando o dispositivo ficar online).
- Requer Federated Identity Pool no Cognito.
- Armazena dados de até 1MB no dataset.
- Máximo de 20 datasets para sincronização.
- Cognito Sync foi depreciado para uso do serviço AppSync.
