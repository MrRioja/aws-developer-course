# IAM (Identity and Access management)

> IAM - Identidade e Gerenciamento de acesso

- **Root Account** NUNCA deve ser usada ou compartilhada.
- Aos usuários deve ser dada mínima permissão para executar tarefas requeridas.
- **Policy** (regras) são escritas em JSON (Javascript Object Notation), define as permissões.
- Muito importante para segurança na plataforma AWS:
  - **Users** - Atribuído a usuários.
  - **Groups** - Atribuído a times e funções.
  - **Roles** (atribuição/papeis) - Atribuído a recursos AWS.
- Pode ser configurado o Multi Factor Authentication (MFA).
- IAM tem policies pré definidas (**_Managed policies_**)

## IAM Federation

- Grandes empresas normalmente integram seu repositório de usuários com IAM.
- Identity Federation usa o padrão SAM (active directory).
- Dessa forma os usuários pode acessar AWS usando a credenciais da empresa.

## IAM - MUITO IMPORTANTE

- Credenciais IAM **NUNCA** devem ser compartilhadas.
- Nunca coloque suas credenciais no código da sua aplicação.
- Nunca use a conta ROOT. Somente para a configuração inicial.
- Nunca use credenciais da conta ROOT.
