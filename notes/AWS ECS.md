# AWS ECS (Elastic Container Service)

## ECS - Clusters

- ECS Clusters são grupos lógicos de EC2.
- EC2 roda ECS agent (Docker container).
- ECS agent registra a instância EC2 no ECS Cluster.
- EC2 roda um AMI especial, feita exclusivamente para ECS.

## ECS - Task Definition

- Task Definition são metadata no formato JSON que diz ao ECS como rodar o container Docker.
- Contem informações cruciais sobre:
  - Nome da imagem.
  - Port Binding entre o host e o container.
  - Memória e CPU requerida.
  - Variáveis de ambiente.
  - Informações de rede.
  - IAM Role.

## ECS - Service

- ECS Services define quantas Tasks devem rodar e como elas devem rodar.
- Eles garantem que o número de Tasks definidas fique sempre rodando no Cluster de EC2.
- Pode trabalhar em conjunto com ELB, NLB e ALB se necessário.
