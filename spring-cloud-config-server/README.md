# Spring Cloud Config Server

## Backend File System

这里采用的是 Git，仓库地址 https://github.com/mambat/spring-cloud-config-data

> 需要开启 IDEA Github 配置项的 'Clone git repositories with ssh'

## git 分支与环境对应、单环境内不区分 profile

* 新建 dev、testing、prod 分支
* 为各个分支新建配置文件 billing.properties
* 访问 dev 环境的 billing 配置 `http://localhost:8888/billing/default/dev`，其他环境的访问规则一样，将 dev 替换成 testing/prod 即可

> 注意：读取配置说必须带上 profile。在无需区分 profile 时使用任意字符串即可，比如这里的 default，最终读取的依然是 billing.properties。

## git 分支与环境对应、单环境内区分 profile

* 新建 dev、testing、prod 分支
* 为各个分支新建 billing 服务 对应 abc profile 的配置文件 billing`-abc`.properties（以 -{profile} 结尾）
* 访问不同环境 billing 服务对应 profile 配置 `http://localhost:8888/billing/abc/dev`，将 dev 替换成 testing/prod 即可
* 可以把 billing 不同 profile 的相同配置抽取出来、新建 billing.properties 文件，各 profile 配置文件也可以覆盖 billing.properties 中的配置项

> URL Shema `http://localhost:8888/{name}/{profiles}/{label}`
>
> name，服务名/配置名
>
> profiles，多个 profile 用逗号分隔
>
> label，git 分支名

## 读取格式

* 返回 JSON 格式
>
> http://localhost:8888/dev/billing-abc.json
>
> http://localhost:8888/dev/billing-abc,xxx.json
>
> 还支持 yml 和 properties 格式，将 `.json` 替换成 `.yml` 或 `.properties` 即可

## 增加 HttpBasic 认证

* 增加依赖 spring-boot-starter-security

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

* application.yml 配置用户名和密码（用户名只能是 `user`）

```yaml
security:
  user:
    password: mypassword
```
* 通过浏览器访问，输入用户名、密码；或者 curl 时指定用户名、密码

```bash
curl -u user:mypassword http://localhost:8888/billing/default/dev
```





