
# 生成新的 docker 镜像
``` bash
docker build -t backendless/bl-server-szd:latest .
```
// docker rm $(docker ps -aq) 

# 修改 Scripts 下 backendless-compose.yml

``` yaml
bl-server:
    # image: "${REGISTRY}bl-server:${VERSION}"
    image: "${REGISTRY}bl-server-szd:${VERSION}" // *
    #stop_signal: SIGINT
    hostname: bl-server
    command: ["play"]

    env_file:
      - ports.env

    environment:
      - BL_CONFIG_PROVIDER=consul
      - BL_CONSUL_HOST=bl-consul
      - BL_CONSUL_PORT=8500

    #environment:
    #-

    ports:
    - "${BL_PROPERTY_config_server_publicPort}:9000"
    volumes:
    #- nfsRepo:/opt/backendless/repo
    - ${MOUNTS}/repo:/opt/backendless/repo
    - ${MOUNTS}/server/logs:/opt/backendless/server/play-logs
    networks:
    - bkndls-net

    depends_on:
    - bl-consul
    - bl-redis
    - bl-mysql
    - bl-mongo

    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9000"]
      interval: 20s
      timeout: 1s
      retries: 3
      start_period: 180s
      
bl-taskman:
    # image: "${REGISTRY}bl-server:${VERSION}"
    image: "${REGISTRY}bl-server-szd:${VERSION}" // *
    #stop_signal: SIGINT
    hostname: bl-taskman
    command: ["taskman"]

    env_file:
      - ports.env

    environment:
      - BL_CONFIG_PROVIDER=consul
      - BL_CONSUL_HOST=bl-consul
      - BL_CONSUL_PORT=8500

    volumes:
    #- nfsRepo:/opt/backendless/repo
    - ${MOUNTS}/repo:/opt/backendless/repo
    - ${MOUNTS}/taskman/logs:/opt/backendless/server/play-logs
    networks:
    - bkndls-net

    depends_on:
    - bl-redis
    - bl-mysql
    - bl-mongo
    - bl-server

```