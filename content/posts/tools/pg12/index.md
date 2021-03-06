---
title: "本地Docker安装Postgres 12 + pgadmin （支持Apple M1)"
date: 2021-03-22
hero: pg.svg
menu:
    sidebar:
        name: Postgres 12 + pgadmin in Docker
        identifier: pg12
        parent: tools
        weight: 10
---

### 介绍
项目最近要升级Posgres数据库， 从9.6升级到12+。为了做一些migration测试，我本地要安装几个版本的Postgres，最方便的就是
用Docker安装了，没有版本冲突的问题，好管理，也方便删除。

另外建议使用docker-compose，或者stack，简单说就是可以data存在本地，这样每次重新启动，数据不会丢，可以重复使用。如果
是做integration testing，则可以每次启动一个新的container。

下面docker-compose文件里面还有pgAdmin，这样使用Postgres更方便。也可以使用自己喜欢的DB browser，我用IDEA（ultimate）
带的Database plugin。

### 支持 Intel CPU
我在MacOS下用了一段时间，没问题。
* 保存成docker-compose.yml文件
* 在文件路径下运行 `docker-compose up -d`

说明：
* user和password自己随意设置
* volumes是本地保存数据库的路径
* ports：默认是5432。我一般喜欢改成15432，项目多了，10000下的port很拥挤
* pgadmin的email和password是页面登陆密码
* pgadmin的volumes和ports跟Postgres性质一样
```bash
version: '3.5'

services:
  postgres:
    container_name: pg12
    image: postgres:12
    environment:
      POSTGRES_USER: pg12
      POSTGRES_PASSWORD: pg12
      PGDATA: /data/postgres
    volumes:
       - postgres12:/Users/szhang/postgresql/pg12
    ports:
      - "5432:5432"
    networks:
      - pg12
    restart: unless-stopped

  pgadmin:
    container_name: pgadmin12
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: a@gmail.com
      PGADMIN_DEFAULT_PASSWORD: a@gmail.com
    volumes:
       - pgadmin12:/Users/szhang/postgresql/.pgadmin12
    ports:
      - "27777:80"
    networks:
      - pg12
    restart: unless-stopped

networks:
  pg12:
    driver: bridge

volumes:
    postgres12:
    pgadmin12:
```

### 支持 Apple M1
这个版本唯一不同在于Postgres image 是ARM版本的，专门支持最新的Apple M1芯片的电脑。另外多说一句，Apple M1电脑可以跑Docker，
但是很多Docker image还没有ARM版，所以目前用M1电脑做开发（需要docker）还不方便。
```bash
version: '3.5'

services:
  postgres:
    container_name: pg12
    image: arm64v8/postgres:12.6
    environment:
      POSTGRES_USER: pg12
      POSTGRES_PASSWORD: pg12
      PGDATA: /data/postgres
    volumes:
      - postgres12:/Users/shubozhang/dev/postgresql/pg12
    ports:
      - "5432:5432"
    networks:
      - pg12
    restart: unless-stopped

  pgadmin:
    container_name: pgadmin12
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: a@gmail.com
      PGADMIN_DEFAULT_PASSWORD: a@gmail.com
    volumes:
      - pgadmin12:/Users/shubozhang/dev/postgresql/.pgadmin12
    ports:
      - "27777:80"
    networks:
      - pg12
    restart: unless-stopped

networks:
  pg12:
    driver: bridge

volumes:
  postgres12:
  pgadmin12:
```

### 测试

* pgAdmin
    * 登陆，使用docker-compose里面的email和密码
    ![Alt text](/images/posts/tools/pg12-pgadmin-login.png)
      
    * 使用界面
    ![Alt text](/images/posts/tools/pg12-pgadmin.png)


* Intellij IDE

    使用用户名，密码，和端口就可以链接了。
    ![Alt text](/images/posts/tools/pg12-idea.png)
