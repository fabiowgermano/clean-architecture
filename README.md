# clean-architecture

Projeto contendo o exercício Clean-Architecture do curso Go-Experts.
API criar e listar ordens de serviço, informando preço e taxa, retornando preço final.

## Requisitos
- [x] API deve conter servidor REST, GraphQL e gRPC.
    - [x] Endpoint REST (GET /order)
    - [x] Service ListOrders com GRPC
    - [x] Query ListOrders GraphQL
- [x] Requests para criar e listar orders no arquivo api.http
- [x] Migrações necessárias
- [x] Usar docker e docker-compose e executar em containers
- [x] Documentar projeto e como executá-lo

## Tecnologias Utilizadas

- wire
- viper
- testify
- amqp(rabbitmq)
- net/http, grpc, graphql
- mysql

## Pré Instalação

1. Preparar o ambiente configurando o docker na maquina

2. Garanta que o Migrate e Evans estejam pré instalados na build do Go
``` shell
make install
```

## Instalação/Execução

1. Executar o comando abaixo para criação e execução do banco de dados, rabbitmq e aplicação nos containers
``` shell
docker-compose up
```
ou
``` shell
make up
```

2. Como testar a aplicação: REST API server
- faça uma chamada POST para criar uma nova order via rest-client usando o arquivo [api.http](api/api.http)
- faça uma chamada GET para listar as orders via rest-client usando o arquivo [api.http](api/api.http)

3. Como testar a aplicação: gRPC server
``` shell
## criar nova order
evans --proto internal/infra/grpc/protofiles/order.proto --host localhost --port 50051
> call CreateOrder
> 2
> 2.1
> 0.2

## listar orders
evans --proto internal/infra/grpc/protofiles/order.proto --host localhost --port 50051
> call ListOrders
```

4. Testar aplicação: GraphQL server 
``` shell
http://localhost:8080/
```

``` shell
## criar nova order
mutation createOrder {
  createOrder(input: {id:"1", Price: 1.1, Tax: 0.1}) {
    id
    Price
    Tax
  }
}

## listar orders
query queryOrders {
  listOrders {
    id
    Price
    Tax
    FinalPrice
  }
}
```

5. Baixar containers 
``` shell
make down
```