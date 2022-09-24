# ElastiCache

- ElastiCache é similar ao RDS. Oferece banco de dados gerenciado pela AWS.
- Duas opções disponíveis: Redis e Memcached.
- Caches são banco de dados na memória RAM, alta performance e baixa latência.
- Ajuda reduzir a carga no banco de dados principal para a leitura intensa.
- Ajuda a fazer sua aplicação Stateless.
- Escrita escalona usando Sharding.
- Leitura escalona usando Read replica.
- Multi AZ com capacidade Failover.
- AWS cuida da manutenção, patching, otimização, setup, configuração, monitoramento, recuperação de falhas e backups.
- Cache deve ter uma regra de atualização para garantir que somente os dados mais recentes são utilizados.

## ElastiCache - Guardar sessão do usuário

Podemos utilizar o ElastiCache para guardar a sessão dos usuários. Seguindo o fluxo:

1. Usuário loga na aplicação pela instância A.
2. Aplicação escreve os dados da sessão no ElastiCache.
3. Usuário acessa a aplicação novamente pela instância B.
4. Instância B lê os dados do cachê e verifica que o usuário já está logado.

## Redis

- É um banco de dados do tipo Key-Value que armazena os dados na RAM.
- Latência muito baixa (sub ms).
- Cache não se perde quando a instância é reiniciada (persistente).
- Ideal para:
  - Sessão do usuário.
  - Aliviar a carga no banco de dados.
  - LeaderBoard (placar de jogos).
- Multi AZ com failover automático para recuperação de desastres se você não quiser perder seus dados.
- Suporta Read Replicas.

## MemCached

- Armazena objetos na memória RAM.
- Perde os dados se a instância for reiniciada.
- Leitura rápida dos objetos armazenados.
