# EC2 (Elastic Compute Cloud)

- Baixar a chave para conexão SSH.
- Aplicar: `chmod 400 ec2keypair.pem` para modificar a segurança do arquivo.
- Executar comando de execução que estará disponível no console.

# EC2 Tipos de instancias

- **_On Demand_** - Processos a curto prazo, é pago o que é usado.
- **_Reserved_** - Processos de longo prazo (períodos maiores que 1 ano).
- **_Convertible Reserved_** - Processos de longo prazo com flexibilidade de tipos.
- **_Scheduled Reserved_** - Para processos com janela determinada.
- **_Spot_** - Processos curtos, mais baratos, podem ser finalizados a qualquer momento.
- **_Dedicated_**: Equipamento único e exclusivo.
- **_Dedicated Hosts_**: Reserva de equipamento inteiro, com controle de Placement.

# Security groups

- É o fundamento básico de segurança na AWS.
- Determina o que é permitido entrar e sair do EC2.
- É o fundamento mais importante para solucionar problemas de rede.
- Funcionam como firewall para o EC2.
- Controlam:
  - Acesso as portas.
  - Endereços ip permitidos (IPV4 e IPV6).
  - Controlam entrada e saída de dados.
- Pode ser associado a múltiplas instancias.

#### Escopo: VPC

- É uma boa pratica ter um Security Group somente para acesso SSH.
- Se o problema for Connection Refused, não trata-se de security group.
- Todo trafego de entrada é bloqueado por padrão.
- Todo trafego de saída é liberado por padrão.
- É possível referenciar um outro security group ao criar um security group.
- Private IPV4:
  - 10.0.0.0/8 [10.0.0.0 - 10.255.255.255].
  - 172.16.0.0/12 [172.16.0.0 - 172.31.255.255].
  - 192.168.0.0/16 [198.168.0.0 - 192.168.255.255].

# EC2 User Data

- Usado para rodar scripts na primeira vez que uma nova instância for iniciada.
- O script fica nas configurações avançadas da instância.

# AMI (Amazon Machine Image)

- Possui imagens já disponibilizadas e que podem ser personalizadas através do User Data.
- Também é possível criar uma própria imagem personalizada e reutilizável.
- É criada por região.

# Load Balancer

Podem ser exposto para rede publica e privada.

#### Tipos de load balancer:

- **_Classic Load Balancer_** - 2009 Obsoleto.
- **_Application Load Balancer_** - 2016, suporta HTTP, HTTPS e Websocket, suporta SSL, pode rotear baseado em path e hostname.
- Network Load Balancer - 2017, TCP.

> **TODOS** os tipos de Load Balancer possuem DNS. **NÃO** user endereço IP.

Por padrão as instancias não possuem acesso ao IP do cliente final, mas ele pode ser acessado através do header:

- `X-Forwarded-for` - Endereço do cliente.
- `X-Forwarded-Port` - Porta da requisição.
- `X-Forwarded-Proto` - Qual o proto da requisição.

`Erro 503` - Capacidade máxima atingida ou target não definido

# Health Check

- Health Check verifica se as instancias são capazes de atender as requisições.
- São executados em portas e caminhos específicos.

# Auto Scaling Group

Cria e remove instancias EC2.

- **_Scale out_** - Cria instancias para atender o aumento da demanda. Sempre as registra no Load Balancer automaticamente.
- **_Scale in_** - Remove instancias EC2 por tolerar a demanda atual.

- Pode ser acionado por uso de CPU, network, agendamentos ou por Custom Metrics.
- Usa Launch configurations.
- IAM Roles da ASG serão atribuídos a instância EC2 criada.
- Instancias problemáticas iniciadas pelo ASG são substituídas por novas instancias também criadas pelo ASG.
- Instancias problemáticas podem ser identificadas pelo ASG e Load Balancer.

# EBS (Elastic Block Storage)

- Permite que os dados não se percam quando uma instancia EC2 seja terminada.
- É um driver de rede.
- Pode ser desanexado de uma instancia e anexado em uma outra instancia.
- Pertence a somente um Availability Zone.
- Não pode ser associado a uma instancia EC2 de outra availability zone.
- É possível aumentar o tamanho depois de criado. - Após aumentar é necessário reparticionar.
- Pode ser feito backup do EBS usando o Snapshot, que terá o tamanho do volume usado e não o total do volume.
- Criptografado usando KMS.
- Volumes EBS root é terminado quando a instancia EC2 é terminada.

# Snapshot

São usados para migração:

- Diminuir volumes.
- Mudar o tipo de volume.
- Criptografar um volume.

Tipos de volumes:

- **_GP2 (General Purpose SSD)_**: Meio termo entre preço e performance.
- **_IOI_**: Alta performance, baixa latência e alto custo.
- **_STI (HDD)_**: Baixo custo. Acesso frequente.
- **_SCI (HDD)_**: Mais barato de todos. Acesso não frequente.
