# ENV
```shell
#!/bin/bash

export DB_USERNAME="ct_admin"
export DB_NAME="geoconnections"
export DB_PASSWORD="wowimsosecure"
export DB_HOST="localhost"
export DB_PORT="5432"

# gRPC
export GRPC_SERVER_ADDRESS="localhost:50051"

# Kafka
export GRPC_SERVER_ADDRESS="localhost:9092"
```

## Start RPC server
```shell

python app/server.py

```
## Run test
```shell

python app/client.py

```
