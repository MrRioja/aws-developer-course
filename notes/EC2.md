# EC2 (Elastic Compute Cloud)

**Para conectar via SSH à uma instancia EC2:**

- Baixar a chave para conexão SSH.
- Aplicar: `chmod 400 ec2keypair.pem` para modificar a segurança do arquivo.
- Executar comando de execução que estará disponível no console.

## EC2: Tipos de instancias

- **_On Demand_** - Processos a curto prazo, é pago o que é usado.
  - Pago pelo uso. Cobrado por segundo após o primeiro minuto.
  - Sem contrato.
  - Recomendado para aplicações de curto prazo e em desenvolvimento.
- **_Reserved_** - Processos de longo prazo (períodos maiores que 1 ano).
  - Até 75% de desconto em relação à On Demand.
  - Pagamento adiantado.
  - Período de reserva de 1 ou 3 anos.
  - Reserva específica para determinado tipo de instancia.
  - Recomendado para aplicações estáveis (_ex: banco de dados_).
- **_Convertible Reserved_** - Processos de longo prazo com flexibilidade de tipos.
  - Pode mudar o tipo de instancia.
  - Até 54% de desconto.
- **_Scheduled Reserved_** - Para processos com janela determinada.
  - Funciona na janela de tempo determinada.
- **_Spot_** - Processos curtos, mais baratos, podem ser finalizados a qualquer momento.
  - Desconto de até 90% comparado com On Demand.
  - Funciona tipo leilão. Você usa a instancia até pagarem mais pela instancia.
  - Preço varia de acordo com a oferta e demanda.
  - Você é notificado quando cobrem sua oferta e em 2 minutos sua instancia é terminada.
  - Usados para batch jobs, Big Data Analysis ou processos que aceitam interrupções.
  - Não deve ser usado para processos críticos ou bancos de dados.
- **_Dedicated_**: Equipamento único e exclusivo.
  - Podem compartilhar instancias com outras que pertencem a mesma conta AWS.
  - Sem controle de Placement e pode ser movida para outro hardware se houver o stop e start da instancia.
- **_Dedicated Hosts_**: Reserva de equipamento inteiro, com controle de Placement.
  - Controle sobre onde colocar a instancia.
  - Reservado por 3 anos.
  - Mais caro.
  - Usado para softwares com modelos de licenças complexos ou empresas com regras rígidas sobre localização de servidores.

## Security groups

- É o fundamento básico de segurança na AWS.
- Determina o que é permitido entrar e sair do EC2.
- É o fundamento mais importante para solucionar problemas de rede.
- Funcionam como firewall para o EC2.
- Controlam:
  - Acesso as portas.
  - Endereços IP permitidos (IPV4 e IPV6).
  - Controlam entrada e saída de dados.
- Pode ser associado a múltiplas instancias.
- O security group é do escopo da VPC onde foi criado.
- É uma boa pratica ter um Security Group somente para acesso SSH.
- Se houver problema de `Timeout` ao tentar conexão com a instância EC2 provavelmente trata-se de security group.
- Se o problema for `Connection Refused`, **NÃO** trata-se de security group.
- Todo trafego de entrada é bloqueado por padrão.
- Todo trafego de saída é liberado por padrão.
- É possível referenciar um outro security group ao criar um security group para permitir acesso entre instancias.
- Private IPV4:
  - **_10.0.0.0/8 [10.0.0.0 - 10.255.255.255]_**.
  - **_172.16.0.0/12 [172.16.0.0 - 172.31.255.255]_**.
  - **_192.168.0.0/16 [198.168.0.0 - 192.168.255.555]_**.

## EC2: User Data

- Usado para rodar scripts na primeira vez que uma nova instância for iniciada.
- O script fica nas configurações avançadas da instância.
- Os comandos escritos no User Data são executados como usuário root.

Exemplo de script para User Data:

```bash
#!/bin/bash
yum update -y
yum install -y httpd.x86_64
systemctl start httpd.service
systemctl enable httpd.service
echo "My web server - Address: $(hostname -f)" > /var/www/html/index.html
```

## EC2 - Launch template

O Launch Template é um modelo de criação de instâncias que podemos criar para iniciar instâncias a partir dele. É um modelo com todas as opções que temos quando criamos uma instância manualmente e que podemos versionar e modificar sempre que necessário.

- É uma boa prática sempre que criarmos um novo modelo testar a criação de uma instância a partir dele para assegurar de que está tudo configurado conforme o esperado.

## EC2 - Teste de stress

Abaixo script útil para fazer teste de stress em instâncias EC2:

```bash
for i in $(seq $(getconf _NPROCESSORS_ONLN)); do yes > /dev/null & done
```

Após o stress, podemos executar o comando abaixo para matar o processo de stress:

```bash
kill -9 PID
```

## AMI (Amazon Machine Image)

- Possui imagens já disponibilizadas e que podem ser personalizadas através do User Data.
- Também é possível criar uma própria imagem personalizada e reutilizável.
- É criada por região.

## Elastic IP

- Quando paramos uma instancia EC2 seu endereço IP público pode mudar.
- Para que, ao parar uma instancia, seu endereço IP não mude precisamos associa-lá à um Elastic IP, que por sua vez só pode estar associado à uma instancia.
- O limite de Elastic IP por conta AWS é 5, por padrão. Podemos solicitar o aumento desse limite através de um AWS Ticket.
- Elastic IP que esteja criado e não anexado a nenhuma instancia será cobrado.
- Recomendado apenas para desenvolvimento e seu uso em produção demonstra uma arquitetura de baixa qualidade.

## ELB - EC2 Load Balancer

- São servidores que distribuem o trafego entre instancias EC2.
- Somente o DNS do Load Balancer é visto pelos clientes.
- Oferece SSL (HTTPS) para usa aplicação.
- Opção Stickness com cookies. Durante uma mesma sessão o cliente sempre é enviado para a mesma instancia EC2. E é gerenciado pelo Load Balancer.
- Separa trafego publico e privado.
- Podem ser configurados para rede publica e privada.

### Tipos de load balancer

- **CLB - _Classic Load Balancer_** - 2009.
  - Obsoleto.
  - Suporta SSL.
- **ALB - _Application Load Balancer_** - 2016:
  - Trabalha na layer 7.
  - Suporta SSL.
  - Múltiplas aplicações HTTP em maquinas diferentes (Target Groups).
  - Múltiplas aplicações HTTP na mesma máquina (Containers).
  - Load Balancing baseado em rotas(URL).
  - Load Balancing baseado em hostname(URL).
  - Excelente para aplicações rodando em containers (Ex: ECS).
  - Recurso de Port Mapping para redirecionar para porta dinâmica.
  - Suporta HTTP/HTTP e protocolo websocket.
  - A aplicação não vê o endereço IP da máquina cliente.
- **NLB - _Network Load Balancer_** - 2017, TCP.
  - Trabalha na layer 4.
  - Direciona tráfego TCP para instâncias.
  - Capacidade para milhões de instancias.
  - Suporta IP estático ou Elastic IP.
  - Menor latência. ~100ms VS 400ms da ALB.
  - Pode ver IP do cliente diretamente.
  - Usado em aplicações com alto volume de tráfego.

> **TODOS** os tipos de Load Balancer possuem DNS e fazem health check. **NÃO** usar endereço IP.

Por padrão as instancias não possuem acesso ao IP do cliente final, mas ele pode ser acessado através do header:

- `X-Forwarded-for` - Endereço do cliente.
- `X-Forwarded-Port` - Porta da requisição.
- `X-Forwarded-Proto` - Qual o proto da requisição.

#### Erros Load Balancer

- `4XX` - Erros gerados pelo cliente.
- `5XX` - Erros gerados pela aplicação.
- `Erro 503` - Capacidade máxima atingida ou target não definido.

## ELB - Health Check

- Health check é crucial para o funcionamento do Load Balancer.
- Health Check verifica se as instancias são capazes de responder as requisições.
- São executados em portas e caminhos específicos. Ex: `/health`. E se a resposta não for `200 (OK)` a instância é considerada deficiente e o Load Balancer para de enviar trafego para ela.

## ASG - Auto Scaling Group

Cria e remove instancias EC2.

- **_Scale out_** - Cria instancias para atender o aumento da demanda. Sempre as registra no Load Balancer automaticamente.
- **_Scale in_** - Remove instancias EC2 por tolerar a demanda atual.

- É possível escalonar um ASG baseado em alarmes do CloudWatch.

- Pode ser acionado por uso de CPU, network, agendamentos ou por Custom Metrics.

- Usa Launch configurations.

- IAM Roles da ASG serão atribuídos a instância EC2 criada.

- Instancias problemáticas iniciadas pelo ASG são substituídas por novas instancias também criadas pelo ASG.

- Instancias problemáticas podem ser identificadas pelo ASG e Load Balancer.

- Podemos enviar Custom Metrics da aplicação EC2 para o CloudWatch usando **PutMetrics API**.

## EBS - Elastic Block Storage

- Permite que os dados não se percam quando uma instancia EC2 seja terminada.
- É um driver de rede.
- Pode ser desanexado de uma instancia e anexado em uma outra instancia.
- Pertence a somente um Availability Zone.
- Não pode ser associado a uma instancia EC2 de outra availability zone.
- É possível aumentar o tamanho depois de criado. Após aumentar é necessário reparticionar.
- Pode ser feito backup do EBS usando o Snapshot, que terá o tamanho do volume usado e não o total do volume.
- Criptografado usando KMS (AES-256).
- Volumes EBS root é terminado quando a instancia EC2 é terminada, por padrão.

### EBS - Tipos de volumes

- **_GP2 (General Purpose SSD)_**: Meio termo entre preço e performance.
- **_IOI_**: Alta performance, baixa latência e alto custo.
- **_STI (HDD)_**: Baixo custo. Acesso frequente.
- **_SCI (HDD)_**: Mais barato de todos. Acesso não frequente.

## EBS - Snapshot

São usados para:

- Fazer backup.
- Salvar dados em caso de catástrofe.
- Migração:
  - Diminuir volumes.
  - Mudar o tipo de volume.
  - Criptografar um volume.
