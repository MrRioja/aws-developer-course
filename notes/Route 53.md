# Route 53

- É um DNS (_Domain Name System_) gerenciado pela AWS.
- DNS é uma coleção de registros (**_Records_**) e regras (**_Rules_**) que auxiliam o cliente a encontrar o servidor desejado utilizando uma URL (**_Universal Resource Locator_**).
- Na AWS os **Records** mais comuns são:
  - **A**: URL para IPV4.
  - **AAAA**: URL para IPV6.
  - **CNAME**: URL para URL.
  - **Alias**: URL para recursos AWS.
- AWS Route 53 pode usar:
  - Public Domain Name que pertence a você. (_Ex: meuenderecopublico.com_)
  - Private Domain Name que podem ser resolvidos pelas suas instâncias EC2 dentro da sua VPC. (_Ex: app.empresa.internal_)
- Route 53 tem função avançada, por exemplo:
  - Load Balancing (usando DNS - também chamado de Client Load Balancing).
  - Routing Policy: simple, failover, geolocation, geoproximity, latency and weighted.
- Use Alias para recursos AWS ao invés de CNAME para ter mais performance.
