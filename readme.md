# 先拉取镜像与脚本

[Backendless Pro](https://github.com/Backendless/BackendlessPro)

# 生成新的 docker 镜像
``` bash
docker build -t backendless/bl-server-szd:5.7.3.3.pro .
```
// docker rm $(docker ps -aq) 

# 修改 Scripts 下 backendless-compose.yml

``` yaml
bl-server:
    # image: "${REGISTRY}bl-server:${VERSION}"
    image: "${REGISTRY}bl-server-szd:${VERSION}"
    ...
      
bl-taskman:
    # image: "${REGISTRY}bl-server:${VERSION}"
    image: "${REGISTRY}bl-server-szd:${VERSION}"
    ...
```