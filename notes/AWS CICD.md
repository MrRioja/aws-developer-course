# AWS CICD (Continuous Integration and Continuos Delivery)

**Fluxo de CI/CD na AWS:**

![CI/CD workflow](../readme/ci-cd-workflow.png)

- AWS CodeCommit: Repositório para seu código.
- AWS CodePipeline: Automatiza pipeline do código para o ElasticBeanStalk.
- AWS CodeBuild: Building e testa o código.
- AWS CodeDeploy: Deploy do código para grupo de instâncias EC2.

## Continuous Integration

- Desenvolvedores fazem o push do código para repositórios no Github ou CodeCommit.
- Build Server verifica/testa o código assim que é atualizado usando o CodeBuild ou Jenkins.
- O desenvolver recebe o feedback sobre o teste e verifica se o código passou no teste ou falhou.
- Entre bugs rapidamente.
- Rápido deployment depois do código testado.
- Deployment mais frequente.

## Continuous Delivery

- Garante que o código será distribuído de forma confiável.
- Garante que o deployment pode ser frequente e rápido.
- Muda o conceito de atualizar o código 1 vez por semana para 4 vezes no dia.

## AWS CodeCommit

- Version control é a capacidade de entender e armazenar várias versões do código com a capacidade de rollback.
- Habilitado usando Version Control System tipo git.
- Repositório git pode ser armazenado somente em uma máquina, mas geralmente é armazenado em um repositório online.
- Vantagens do repositório online:
  - Colaborar com outros desenvolvedores.
  - Garante que é feito o backup do código.
  - Garante que está disponível e auditável.
- Repositórios git podem custar caro.
- Github oferece repositórios públicos e privados grátis.
- AWS CodeCommit:
  - Repositório git privado.
  - Sem limite de armazenamento.
  - Gerenciado pela AWS.
  - Alta disponibilidade.
  - Código disponível somente na conta AWS (mais seguro).
  - Segurança com criptografia e controle de acesso.
  - Integrado com Jenkins/CodeBuild e outras ferramentas.

### AWS CodeCommit - Segurança

- Interações são feitas usando git.
- Autenticação em git:
  - SSH: Usuário AWS pode configurar SSH Keys no console IAM.
  - HTTPS: Usando AWS CLI Authentication Helper ou gerando credenciais HTTPS.
  - MFA (Multi Factor Authentication): Pode ser habilitado para segurança extra.
- Autorização git:
  - IAM Policies/Roles com permissão para acessar o repositório.
- Criptografia:
  - Repositório é automaticamente criptografado usando KMS.
  - Criptografia em trânsito deve usar HTTPS ou SSH.
- Cross Account Access:
  - Não compartilhe sua SSH Key.
  - Nao compartilhe sua Credential AWS.
  - Use IAM Role na sua conta AWS e use AWS STS com AssumeRole API.

### AWS CodeCommit VS Github

- Similaridades:
  - Repositório git.
  - Code reviews (pull requests).
  - Pode ser integrado com AWS CodeBuild.
  - Suporta HTTPS e SSH.
- Diferenças:
  - Segurança:
    - Github: Github users.
    - CodeCommit: AWS IAM.
  - Hospedagem:
    - Github: hospedado pelo Github.
    - Github Enterprise: hospedado no seu servidor.
    - CodeCommit: gerenciado e hospedado pela AWS.
  - Interface com o usuário:
    - Github: oferece mais funções.
    - CodeCommit: oferece apenas o básico.

### AWS CodeCommit - Notifications

- Você pode notificar no CodeCommit usando AWS SNS (Simple Notification Service), AWS Lambda ou AWS CloudWatch Event Rules.
- Quando usar notificações SNS/AWS Lambda.
  - Branches deletados.
  - Gatilho para atualizações na branch master.
  - Notificar Build System fora da AWS.
  - Gatilho para AWS Lambda para realizar análise de código.
- Quando usar CloudWatch Event Rules:
  - Gatilho para pull requests updates (created, updated, deleted, commented).
  - CloudWatch Event Rules dispara um tópico SNS.

## AWS CodePipeline

- Utilizado para Continuous Delivery.
- É possível ver o fluxo passo a passo.
- Source (aonde está o código): Github, CodeCommit, Amazon S3.
- Build: CodeBuild, Jenkins.
- Load Test: ferramentas de terceiros.
- Deploy: AWS CodeDeploy, ElasticBeanStalk, CloudFormation, ECS.
- Estágio:
  - Cada estágio pode ter ações executadas de forma serial ou paralela.
  - Exemplos de stages: build, test, deploy, load test...
  - Aprovação manual pode ser habilitada para qualquer estágio.

### AWS CodePipeline - Artifacts

- Cada estágio do pipeline pode criar "artifacts".
- Artifacts são armazenados no S3 e repassados para o proximo estágio.

### AWS CodePipeline - Troubleshooting (solução de problemas)

- Mudanças no CodePipeline Stage acontecem no AWS CloudWatch Event e podem gerar notificações SNS:
  - Possível criar eventos para falhas no pipeline.
  - Possível criar eventos para estágios cancelados.
- Se CodePipeline falhar em um estágio, seu pipeline para e você pode receber informações sobre o erro no console AWS.
- AWS CloudTrail pode ser usado para auditar AWS API Calls.
- Se pipeline não puder executar uma tarefa, confira se IAM Service Role que esta associado tem permissão suficiente (IAM Policy).
