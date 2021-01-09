```
# FROM scratch  // 空白镜像，为构建最原始镜像服务

FROM hub.pf.xiaomi.com/ci-images/ci:golang-1.14.2-onbuild AS builder

ARG APP
RUN  mkdir -p  /home/work/data/www/${APP}
WORKDIR /home/work/data/www/${APP}
COPY . .
RUN GOPROXY=https://goproxy.cn,direct  GOPRIVATE=*.xiaomi.com GO111MODULE=on make build tar
# Final step
FROM hub.pf.xiaomi.com/neo-images/online-app:centos7.3-base
ARG APP
MAINTAINER b2c-sre "b2c-sre@xiaomi.com"
WORKDIR /home/work/data/www/${APP}
COPY --from=builder /home/work/data/www/${APP}/*.tar.gz ./
RUN tar zxvf /home/work/data/www/${APP}/*.tar.gz && \
    rm /home/work/data/www/${APP}/*.tar.gz && \
    /bin/chown -R work:work /home/work

ENV APP ${APP}
RUN /bin/chown -R work:work /home/work
USER work
CMD ["sh", "-c", "  ./bin/${APP}"]
```

## 注意每次调用RUN命令，都多构建一层，为避免多余资源浪费，尽量把RUN命令合作一条。
## 当前生成的确定无用的文件，应该在当前层构建完毕后删掉，因为在后续层只能逻辑隐藏，即只是看不见，但是还在镜像里占用资源。
[各指令含义](https://vuepress.mirror.docker-practice.com/image/dockerfile/)
