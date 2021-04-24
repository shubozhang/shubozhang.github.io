---
title: "Micronaut + GraalVM 是不是真的香"
date: 2021-03-01
hero: index.png
menu:
    sidebar:
        name: mn-graalvm
        identifier: mn-graalvm
        parent: micronaut
        weight: 10
---

微服务规模大了以后，传统build 的docker image就显得非常臃肿，启动慢，体积大，非常不利于scale，
资源消耗也大。一般提高方法就是使用轻量级模块注入，用什么，加什么， 并且不用大的框架，像Spring， Spring boot就
非常不推荐在为服务使用。。。

经过上面优化，性能会好一些，但没啥惊人变化， 因为base image还是openjdk，依赖注入，反射，注解之类
还是会降低启动速度。

### GraalVM（https://www.graalvm.org/）

GraalVM作为一款新型通用的虚拟机，不废话，单刀直入解决问题，启动快50倍，内存消耗少5倍。

![Alt Text](/images/posts/micronaut/graalvm-1.png)

为啥它这么牛，因为它用AOT(ahead-of-time)编译，可以快速启动和低内存消耗。官方列的优点：
* 高效
* AOT 编译 （快速启动）
* 多语言
* 高级工具（调试，监控，profile等等）

后两项还没有特别感觉，因为公司后台主要用Java/Kotlin/Grrovy，没有多语言问题。高级工具这些，公司有统一的stack，也不准备用。

### Micronaut（https://github.com/micronaut-projects/micronaut-core）

Micronaut是一款现代, 基于JVM的全栈Java框架，适用于模块化搭建，易于测试的JVM应用，支持Java, Kotlin和Groovy.
它提供了编译时的依赖注入和AOP处理能力，并最小化反射和动态代理的使用。它应用有着更快的启动速度和更低的内存占用。

### Micronaut + GraalVM （POC）

1. 用Micronaut template创建一个App
    * version: 2.3.3
    * language: Java 11
    * build: gradle
    * test: jUnit
    * feature： netty-server

2. 创建一个简单的API
```
@Controller("/hello")
public class HelloController {
   @Get("/")
   public String index() {
       return "hello from Micronaut + GraalVM";
   }
}
```

3. 添加lib到build.gradle
```
annotationProcessor("io.micronaut:micronaut-graal")
compileOnly("org.graalvm.nativeimage:svm")
```

4. 新建src/main/resources/META-INF/native-image/native-image.properties
```
Args= -H:IncludeResources=logback.xml|application.yml \
      -H:Name=mn-graalvm \
      -H:Class=com.sz.App \
      --report-unsupported-elements-at-runtime
```

5. Dockerfile
```
# this image is only used in building, not for runtime
FROM ghcr.io/graalvm/graalvm-ce:latest as graalvm

# gu = graal updater
RUN gu install native-image

COPY . /home/app/mn-graalvm
WORKDIR /home/app/mn-graalvm

RUN native-image --no-server -cp build/libs/mn-graalvm-*-all.jar

FROM frolvlad/alpine-glibc:alpine-3.12
RUN apk update && apk add libstdc++

# No rest api, so no port to expose
EXPOSE 8080

COPY --from=graalvm /home/app/mn-graalvm/mn-graalvm /app/mn-graalvm
ENTRYPOINT ["/app/mn-graalvm"]
```

6. Build
* `./gradlew clean assemble`
* `docker build . -t mn-graalvm`

提示，这个过程比较消耗时间和资源，分配给docker6～8GB内存，否则容易报错

### 结果

##### 启动时间比较
* GraalVM原生 (32 ms)
  ![Alt Text](/images/posts/micronaut/graalvm-startup.png)


* 非原生（961 ms）
  ![Alt Text](/images/posts/micronaut/graalvm-raw-startup.png)

##### 镜像大小
* 原生 81MB， 非原生251MB    
    ![Alt Text](/images/posts/micronaut/graalvm-size.png)


##### 内存使用
* 原生 (7.64 MB)
  ![Alt Text](/images/posts/micronaut/graalvm-mem.png)
  

* 非原生 (47.27 MB)
  ![Alt Text](/images/posts/micronaut/graalvm-raw-mem.png)

##### 比较总结
| | 原生 | 非原生 |
|---|---|---|
| 启动时间 | 32 ms | 961 ms
| 镜像大小 | 81 MB | 251MB
| 内存使用 | 7 MB | 47MB

通过比较一个最简单的web app来看，GraalVM效果性能还是不错，启动时间和内存使用这两项还是很吸引人的。下一步用大项目，可以
对性能进一步的比较和分析。


源码: https://github.com/shubozhang/micronaut-with-java/tree/main/mn-graalvm
