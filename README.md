# OneTScan
被动代理扫描工具，帮助进行渗透测试
# 被动代理
通过 go-mitmproxy 实现。
## 证书下载
被动代理下HTTPS 网站需要安装证书，HTTPS 证书相关逻辑与 mitmproxy 兼容，
证书会在首次启动命令后自动生成，路径为 ~/.mitmproxy/mitmproxy-ca-cert.pem。windows在C盘的用户名目录下
安装信任根证书

## 启动
OneTScan.exe  web --listen :9081 -o output.html
这样会监听 9081 端口

## 基本使用

### 配置

一些配置可以通过config.yaml修改

`./Jie web -h`

```bash
Flags:
  -h, --help             help for web
      --listen string    use proxy resource collector, value is proxy addr, (example: 127.0.0.1:9080).
                         被动模式监听的代理地址，默认 127.0.0.1:9080
      --np               not run plugin.
                         禁用所有的插件
  -p, --plugin strings   Vulnerable Plugin, (example: --plugin xss,csrf,sql,dir ...)
                         指定开启的插件，当指定 all 时开启全部插件

Global Flags:
      --debug           debug
  -o, --out string      output report file(eg:vulnerability_report.html)
                        漏洞结果报告保存地址
      --proxy string    proxy, (example: --proxy http://127.0.0.1:8080)
                        指定 http/https 代理
```

## 功能

通过主动或者被动收集过来的流量插件内部会判断是否扫描过 （TODO 扫描插件是否要按某个顺序执行？）

### 信息收集

- 网站指纹信息(TODO)
- 敏感信息 
- 主动的路径扫描

#### 扫描目录结构

`scan`目录为扫描插件库，每个目录的插件会处理不同情形

-   PerFile 针对每个url，包括参数啥的
-   PerFolder 针对url的目录，会分隔目录分别访问
-   PerServer 对每个domain 的，也就是说一个目标只扫描一次

|         插件          |                             介绍                             |
| :-------------------: | :----------------------------------------------------------: |
|          xss          |           语义分析、原型链污染、dom 污染点传播分析           |
|          sql          |               目前只实现一些简单的SQL注入检测                |
|         ssrf          |                                                              |
|         jsonp         |                                                              |
|          cmd          |                           命令执行                           |
|          xxe          |                                                              |
|       upload          |                                                              |
|       bypass403       | 	                      403 绕过检测 |
|         crlf          |                           crlf注入                           |
