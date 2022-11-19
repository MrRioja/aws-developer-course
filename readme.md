# AWS Developer

O curso preparatório para certificação AWS Developer tem como objetivo ensinar todos os serviços que são cobrados no exame. Veremos cada serviço com a profundidade que será cobrada no dia da prova. Além de materiais em texto teremos também laboratórios para colocar em pratica o ensinamento ao decorrer dos módulos do curso.

**Data da prova: 05/12/2022 18:00** 📝

## Legenda

|    Status    | Ícone |
| :----------: | :---: |
| Não iniciado |  📌   |
| Em progresso |  📚   |
|  Concluído   |  ✅   |

## Seções de estudo ✍🏼

### Seção 1: Introdução ✅

Nessa seção foi apresentada a estrutura do curso, como funciona o exame da certificação, como criar conta na AWS e como criar alertas de custos.

> Foi configurado um orçamento de US$ 1 com um alerta de custo > 70%.

### Seção 2: Fundamentos AWS: IAM e EC2 ✅

Nessa sessão foram apresentadas as possibilidades de configuração e integrações disponíveis no IAM, bem como as boas práticas na concessão de acessos aos usuários. Também foram apresentadas as possibilidades que o serviço EC2 nos oferece, as quais foram exploradas visando o que é cobrado no exame e o que pertence às boas praticas de uso do serviço.
Aqui estão as anotações dos serviços feitas durante as aulas: [IAM](./notes/IAM.md) e [EC2](./notes/EC2.md).

### Seção 3: Fundamentos AWS: ELB, ASG E EBS ✅

Durante essa sessão ocorreram diversos laboratórios visando a integração de instâncias EC2 com Load Balancer, Auto Scaling Group e Elastic Block Storage.
Seguem as anotações feitas durante as aulas sobre os serviços
[ELB](./notes/EC2.md), [ASG](./notes/EC2.md) e [EBS](./notes/EC2.md).

### Seção 4: Fundamentos AWS: Route 53, RDS, ElasticCache e VPC ✅

Foi estudado nessa sessão boas práticas ao utilizar os respectivos serviços, como migrar um domínio para AWS utilizando o Route 53, boas praticas ao utiliza-los e mais outras configurações e funcionalidades que estão escritas em detalhes nos arquivos de anotações dos serviços: [Route 53](./notes/Route%2053.md), [RDS](./notes//RDS.md), [ElastiCache](./notes/ElastiCache.md) e [VPC](./notes/VPC.md).

### Seção 5: Fundamentos AWS: Amazon S3 ✅

Durante esse módulo foram vistos conceitos de versionamento de objetos, suas regras e suas propriedades. Também foram abordados temas como versionamento de objetos, criptografia, segurança, websites estáticos no S3, CORS, Consistence Model e ganho de performance com key names adequados aos nossos objetos. Seguem as anotações feitas em aula sobre o serviço [S3](./notes/S3.md).

### Seção 6: AWS CLI, SDK, IAM Role e Policies ✅

Nessa sessão vimos como nos conectar com a API da AWS através da CLI ou das SDKs disponíveis para as mais variadas linguagens de programação. Também vimos quais são as praticas recomendadas para cada cenário diferente e aonde as Policies e IAM Roles se enquadram dentro desses cenários. Aqui estão as anotações feitas em aula sobre [AWS CLI](./notes/AWS%20CLI.md), [AWS SDK](./notes/AWS%20SDK.md) e [EC2 Instance Metadata](./notes/EC2.md).

### Seção 7: AWS Elastic Beanstalk ✅

Nesse tópico foram abordadas todas as características do BeanStalk, bem como as plataformas suportadas, sua CLI, mecanismo, tipos de deployments e seu funcionamento.
As anotações desse capitulo foram registras [nesse arquivo](./notes/ElasticBeanStalk.md).

### Seção 8: Ferramentas do desenvolvedor ✅

Essa seção foi voltada para ferramentas para desenvolvedores dentro da AWS. Foram estudados serviços como CodeCommit, CodeBuild, CodePipeline e CodeDeploy. Todos os detalhes de cada um desses serviços foram documentados [aqui](./notes/AWS%20CICD.md).

### Seção 9: AWS CloudFormation ✅

Aqui vimos como funciona o CloudFormation, quais suas principais vantagens e características, quais os formatos possíveis para o arquivo de Template, as estruturas dos templates e como fazer deploy de um CloudFormation Template.
Todas as anotações foram registradas nesse [arquivo](./notes/AWS%20CloudFormation.md).

### Seção 10: Monitoring and Audit ✅

Nessa seção foi abordado os serviços de monitoria e auditoria de logs. Foram vistas as principais funcionalidades do AWS X-Ray, CloudTrail e CloudWatch, bem como suas vantagens e propósitos.
Todos os detalhes dos serviços vistos nessa seção se encontram [nesse arquivo](./notes/AWS%20Monitoring%2C%20audit%20and%20troubleshooting.md).

### Seção 11: AWS Integração e Messaging ✅

Nessa seção vimos os serviços: SQS, SQN e Kinesis, além de suas principais configurações e suas diferenças em relação a uso. Todos os detalhes visto nesse módulo foram registrados [nesse arquivo](./notes/Messaging.md).

### Seção 12: AWS Serverless Lambda ✅

Foram abordadas as vantagens, características e configurações da AWS Lambda. Também foram vistas as boas práticas ao trabalhar com esse serviço e alguns exemplos do que podemos construir ao utiliza-lo. Todos esses tópicos estão [detalhados aqui](./notes/AWS%20Serverless.md).

### Seção 13: AWS Serverless DynamoDB ✅

Nessa seção vimos as principais características do DynamoDB, como as leituras e escritas são calculadas e cobradas, os tipos de leituras possíveis, os tipos de index, o que é e como usar DAX e Streams e os principais cuidados com a segurança quando o assunto é DynamoDB. Esses e outros tópicos estão descritos detalhadamente [nesse arquivo](./notes/AWS%20DynamoDB%20Serverless.md) de anotações.

### Seção 14: AWS Serverless API Gateway, SAM e Cognito ✅

O conhecimento adquirido nessa seção é constituído por diversas configurações sobre API Gateway, passando pelo seus fundamentos e cenários de aplicação e indo até configurações mais complexas como CORS, Caching, Deployment, Documentação de APIs, Monitoramento e Segurança. Também foram abordados temas a respeito do Cognito e SAM. As anotações de tudo que foi visto foram descritas em arquivos separados por arquivos e estão disponíveis a seguir: [API Gateway](./notes/AWS%20API%20Gateway.md), [Cognito](./notes/AWS%20Cognito.md) e [SAM](./notes/AWS%20SAM.md).

### Seção 15: ECS - Elastic Container service ✅

Nessa seção vimos como utilizar container's no ambiente AWS utilizando EC2 ou Fargate. Vimos as principais diferenças entre os dois tipos, suas principais configurações e como utilizar Load Balancer com os container's. As anotações sobre os serviços [ECS](./notes/AWS%20ECS.md) e [ECR](./notes/AWS%20ECR.md) foram documentadas nos seus respectivos arquivos.

### Seção 16: Segurança na AWS ✅

Nessa seção vimos como tornar nossa infraestrutura na AWS mais segura possível utilizando as melhores praticas na nuvem. Para isso, vimos como podemos utilizar o serviço [KMS](./notes/AWS%20KMS.md) para realizar criptografia de dados em diversos serviços AWS e vimos também como utilizar [Parameter Store](./notes/AWS%20Parameter%20Store.md) da maneira correta e mais segura possível para armazenar variáveis ambientes e configurações utilizadas por diversos outros serviços dentro da nossa infraestrutura cloud.

### Seção 17: AWS - Outros serviços ✅

Vimos mais alguns serviços disponíveis na AWS que podem auxiliar a implementação de serviços dentro da nossa infraestrutura na nuvem AWS.

### Seção 18: Limpando o ambiente AWS ✅

Nessa seção fizemos a limpeza na nossa conta com a exclusão de todos os serviços criados durante as aulas afim de evitar cobranças indesejadas.

### Seção 19: Pratica para o exame ✅

Realizado simulado para prova da certificação AWS Developer.

### Seção 20: FAQ ✅

Resposta em video sobre uma dúvida enviada no forum por alunos do curso.
