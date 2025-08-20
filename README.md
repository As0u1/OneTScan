# OneTScan
安全漏洞扫描工具
# 功能介绍
- 子域名爆破
- 子域名被动收集
- 端口扫描
- 站点探测存活
- 目录扫描
- 动态浏览器爬虫
- 指纹识别
- POC漏洞导入，扫描
- 敏感信息检测
- 数据统计
- 常规漏洞扫描（类xray）
# todo
- 爬虫子域名收集
- 基于目录的指纹扫描
- 对危险接口做分离（如delete等）
- waybackurl获取历史接口
- 常规漏洞扫描增加插件（ssti，缓存中毒等）

# 功能详解
## 子域名收集
  主动使用字典进行子域名爆破，被动使用各个接口进行子域名的收集，例如fofa，quake等。  
  后续增加在爬虫过程中的子域名收集。
  被动收集配置文件 provider-config.yaml，支持从以下来源中获取  
  BeVigil, BinaryEdge, BufferOver, C99, Censys, CertSpotter, Chaos, Chinaz, DNSDB, Fofa, FullHunt, GitHub, Intelx, PassiveTotal, quake, Robtex, SecurityTrails, Shodan, ThreatBook, VirusTotal, WhoisXML API, ZoomEye API china - worldwide, dnsrepo, Hunter, Facebook, BuiltWith  
  其中Censys、PassiveTotal、Fofa、Intellix 和 360quake 等来源的复合键需要用冒号 (:) 分隔。
文件示例如下  
  <img width="2105" height="1074" alt="image" src="https://github.com/user-attachments/assets/9ebfd61e-54ec-4e78-b7f6-d3cd0ffccd19" />
  <img width="2127" height="1272" alt="image" src="https://github.com/user-attachments/assets/63c2e474-d8e9-4834-94e3-b86f700ecc3a" />


## 端口扫描
  目前只支持对端口的扫描，并不支持对端口的服务进行指纹识别。  
  支持端口被动收集，来源于shadon

## 目录扫描
  使用go以及更快的fasthttp，扫描速度远超dirsearch，降噪更好。
<img width="2262" height="1338" alt="image" src="https://github.com/user-attachments/assets/eb424f3d-8b5e-461b-9053-07f2f36b8926" />

## 敏感信息检测
  参考arl灯塔wih，在爬虫过程中对响应进行识别。可执行修改规则文件wih.yaml
<img width="2118" height="1314" alt="image" src="https://github.com/user-attachments/assets/bfb0cca3-f299-44e6-82a5-317e5aaeb7ef" />

## 爬虫
  采用浏览器动态爬虫，不采用静态爬虫。速度比静态慢，但爬取接口更全面更多，支持表单填充和点击，能够获取到POST接口以及参数
<img width="2232" height="1317" alt="image" src="https://github.com/user-attachments/assets/6ea12b61-5875-467b-8bed-b2251b4312fe" />

## 常规漏洞扫描
  自研的跟xray的差不多的黑盒扫描，爬虫会将数据全部交给这部分来进行扫描，对各个参数以及其他部分进行fuzz，支持多种漏洞。
  支持插件如下
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
|         bucket          |                           存储桶未授权                           |

示例
<img width="2262" height="1284" alt="image" src="https://github.com/user-attachments/assets/514f3de9-b5ac-4d9b-8539-d57e2c4200d3" />
详细漏洞描述
<img width="2195" height="306" alt="image" src="https://github.com/user-attachments/assets/4a1b9355-fb4a-4692-aa44-9f57b595a061" />
详细请求包
<img width="2040" height="969" alt="image" src="https://github.com/user-attachments/assets/2e9ada47-6223-450f-8bc9-ff70155f2979" />



## 指纹扫描
  支持添加，导入，导出指纹。
  指纹规则：
<img width="2280" height="720" alt="image" src="https://github.com/user-attachments/assets/f04a9fea-6c56-4c71-9332-5f8d788b6a36" />
<img width="2526" height="708" alt="image" src="https://github.com/user-attachments/assets/063557bb-bb43-4039-9e2c-974448b3e3ed" />

## POC扫描
  POC部分采用的是nuclei，可以自行上传nuclei插件。注意目前POC扫描只会基于指纹进行扫描，只有指纹识别到，才会采用对用的nuclei-POC。（后续会添加全量扫描的选项）  
  标签即nuclei模板中的tag，与指纹扫描为映射关系  
  例如tag中带有apache，那么指纹识别时，apache被识别到，才会使用该nuclei模板进行扫描  
  注意：匹配模式为精准匹配，需要将指纹名一字不差的写在tag上 。  
<img width="2559" height="807" alt="image" src="https://github.com/user-attachments/assets/690f93f7-2c62-447c-8d43-1afbf7aca746" />
<img width="2514" height="1251" alt="image" src="https://github.com/user-attachments/assets/c3f040f0-3416-40af-926d-b876f1a8e6bb" />


## 数据统计
在爬取过很多站点后，点击“统计路径”，即可对数据库中所有的爬虫爬取到的路径进行统计，以及进行自动化的去重  
例如扫描过程中经常会出现相同的好几个甚至几十个一模一样的web站点，对这种重复爬取的路径进行统计是没有意义的  
OneTScan会对其body进行hash计算。如果两个站点的body_hash相同认为是同站点，则不会重复统计路径  
这个功能用于统计高频率出现的爬虫路径，加入字典，打造自己的目录字典。  
<img width="2553" height="1344" alt="image" src="https://github.com/user-attachments/assets/8ac748fe-1ff6-42f0-aab9-7d8aca15adcd" />

# 参考
感谢以下工具
https://github.com/Qianlitp/crawlergo  
https://github.com/yhy0/Jie  
https://github.com/projectdiscovery/dnsx  
https://github.com/projectdiscovery/nuclei  
https://github.com/chainreactors/spray  

