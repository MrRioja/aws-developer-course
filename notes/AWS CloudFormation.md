# AWS CloudFormation

- Infraestrutura como código.
- Automatiza o processo de criação de infraestrutura:
  - Em outra região.
  - Em outra conta AWS.
  - Na mesma região, se tudo for deletado.
- CloudFormation é uma forma declarativa de representar sua infraestrutura na AWS para a maioria dos recursos.
- Em CloudFormation podemos provisionar, por exemplo:
  - Security Group.
  - Criar duas instâncias EC2 com o Security Group criado acima.
  - Criar dois Elastic IPs para as duas instâncias acima.
  - Criar um Bucket S3.
  - Criar um Load Balancer (ELB) na frente dessas duas instâncias.
- CloudFormation criará tudo que for pedido na ordem correta, sem a necessidade de ordenar o provisionamento ao realizar a solicitação de criação.

## AWS CloudFormation - Vantagens

- Infraestrutura como código:
  - Nenhum recurso é criado manualmente, o que é ótimo para controle.
  - Código pode usar controle de versão, como por exemplo Git.
  - Mudanças na infraestrutura são vistas como código.
- Custo:
  - Cada recurso recebe um identificador, fácil de saber quanto a stack custa.
  - É possível estimar o custo usando o CloudFormation Template.
  - É possível programar seu ambiente de desenvolvimento para terminar às 17hrs e ser recriado às 09hrs.
- Produtividade:
  - Possibilidade de destruir e criar recursos AWS dinamicamente.
  - Diagrama da sua infraestrutura é gerado automaticamente.
  - Programação declarativa. Sem necessidade de especificar ordem.
- Possível criar várias stacks para vários níveis:
  - VPC stack.
  - Network stack.
  - App stack.
- Não crie stacks do zero, utilize modelos disponíveis na internet.

## AWS CloudFormation - Como funciona

- Templates precisam ser salvos no S3 e então serem referenciados no CloudFormation.
- Para atualizar o template não é possível altera-lo, deverá ser realizado o upload de uma nova versão do template .
- Stacks são identificados pelo nome.
- Deletar uma stack irá deletar todos os artifacts que foram criados pelo CloudFormation.

## AWS CloudFormation - Deploy de um CloudFormation Template

- Manualmente:
  - Editando o template no CloudFormation Designer.
  - Usar o console para digitar os parâmetros.
- Automaticamente:
  - Editar o template usando arquivo YAML ou JSON.
  - Usando CLI para Deploy Templates.
  - Métodos recomendados quando se deseja automatizar sua plataforma.

## AWS CloudFormation - Estrutura

- Componentes dos templates:
  - Resources: Recursos AWS declarados no template (**obrigatório**).
  - Parameters: Entradas dinâmicas para seu template.
  - Mapping: Constantes para seu template.
  - Outputs: Referência aos Resources que foram criados.
  - Conditionals: Lista de condições para criar os Resources.
  - Metadata.
- Template Helpers:
  - Reference.
  - Functions.

## AWS CloudFormation - YAML e JSON

- YAML e JSON podem ser usados no CloudFormation.
- Suporta:
  - Key Value pair.
  - Nested Objects.
  - Suporta arrays.
  - Strings em múltiplas linhas.
  - Pode incluir comentários.

## AWS CloudFormation - Resource

- Resource é o centro do CloudFormation Template (**obrigatório**).
- Representa os diferentes componentes da AWS que serão criados e configurados.
- Resources podem referenciar outros Resources.
- Existem mais de 224 tipos de Resources.
- Resources Types Identifiers são escritos no formato:
  - AWS::_nome-do-serviço_::_nome-data-type_.
  - Exemplo: **AWS::EC2::Instance**.

## AWS CloudFormation - Parameters

- Parameters é a forma mais segura de entrarmos com dados para a criação de Resources.
- Exemplos de uso de Parameters:
  - Reusar seus Templates dentro da empresa.
  - Quando algumas das entradas não podem ser definidas antecipadamente.
- Parameters são muito importantes e previnem errors.

### AWS CloudFormation - Parameters settings

- ConstraintDescription (String).
- Min/MaxLength.
- Min/MaxValue.
- Defaults.
- AllowedValues (array).
- AllowedPatterns (regexp).
- NoEcho (boolean).
- Parameters podem ser controlados pelos seguintes itens:
  - Type:
    - String.
    - Number.
    - CommaDelimitedList.
    - List\<Type>.
    - AWS Parameter (Ajuda a detectar valores inválidos. Valores são comparados com valores na conta AWS).
  - Description.
  - Constraints.

### AWS CloudFormation - Referenciar Parameter

- Função **Fn:Ref** é usada para referenciar parameters.
- Parameters podem ser usados em qualquer lugar no Template.
- Pode-se usar **!Ref** ao invés de **Fn:Ref**.
- **!Ref** pode referenciar outros elementos dentro do template.

### AWS CloudFormation - Pseudo Parameters

- AWS oferece Pseudo Parameters para qualquer CloudFormation Template.
- Pode ser usado a qualquer momento e são habilitados por padrão.
- Alguns deles são:
  - AWS::Region: Retorna a região da stack.
  - AWS::StackId: Retorna o ID da stack.
  - AWS::StackName: Retorna o nome da stack.
  - AWS::URLSuffix: Retorna o sufixo de url.

## AWS CloudFormation - Mappings

- São valores fixos dentro do CloudFormation Template.
- Muito útil para diferenciar dados entre ambientes (dev, prod...), Regions, tipos de AMIs e outros dados.
- Todos os valores são hardcoded no Template.

### AWS CloudFormation - Mappings VS Parameters

- Mapping é ideal quando se sabe os valores que serão usados e esses valores podem vir de variáveis tais como:
  - Region.
  - AvailabilityZone.
  - AWS Account.
  - Environment.
- Permite um controle seguro sobre o template.
- Use parameters quando os valores devem ser definidos pelo usuário.
