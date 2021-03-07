---
title: "介绍一个本地DynamoDB图形界面应用--dynamodb-admin"
date: 2021-03-06
hero: hero.svg
menu:
    sidebar:
        name: dynamodb-admin
        identifier: dynamodb-admin
        parent: tools
        weight: 10
---


### 设置

1. 通过npm安装dynamodb-admin

```node
npm install -g dynamodb-admin
```

2. 运行docker： local dynamodb

`docker-compose.yml` file
```dockerfile
version: '3.8'
services:
  dynamodb-local:
    command: "-jar DynamoDBLocal.jar -sharedDb -optimizeDbBeforeStartup -dbPath ./data"
    image: "amazon/dynamodb-local:latest"
    container_name: dynamodb-local
    ports:
      - "8000:8000"
    volumes:
      - "./docker/dynamodb:/home/dynamodblocal/data"
    working_dir: /home/dynamodblocal
```

Run `docker-compose.yml` file
```
docker-compose up -d
```

3. 启动dynamodb-admin
* MacOS：
    ```
    dynamodb-admin
    ``` 
* Windows: 
    ```
    export DYNAMO_ENDPOINT=http://localhost:8000
    dynamodb-admin
    ```

4. 完成
   * DynamoDB 运行在 `http://localhost:8000`
   * 图形界面运行在 `http://localhost:8001`