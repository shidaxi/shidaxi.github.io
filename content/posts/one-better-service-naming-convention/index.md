---
title: "一种我喜欢的互联网公司应用服务命名规范"
date: "2021-05-23T21:00:00+08:00"
tags: 
- naming
- convention
keywords:
- naming
- convention
---

{{< image src="imgs/programmer-s-hardest-task.png" style="border-radius: 8px;" >}}

## 服务名的作用

* 给人用，标识一个服务，便于团队沟通协作
* 给程序用，用于围绕该应用的各种资源的命名，进而用于自动化处理，例如
  * 给云资源命名， dev-fooapp-ec2
  * 给k8s资源命名, fooapp-ingress, fooapp-deployment
  * 给域名命名 foo.example.com

## 常见命名风格

* camelCase, 首字母小写，后续每个单词首字母大写，比较常见应用于Java等编程语言
* PascalCase, 跟camelCase 类似，但是每个单词首字母大写
* snake_case, 比较常见于C，bash等大多数编程语言
* kebab-case, 跟snake_case 类似，只不过用的是短横线-而非下划线
* Pascal-Kebab-Hybride-Case, 微软powershell里见过
* Pasical_Snake_Hybride_Case, 不常见
* snake_kebab-hybrade_case, 看到有些头痛...
* Snake_Kebab-Hybrade_Case, 看到十分头痛...
* Snake_kebab-hybrade_Case, 脑袋要炸了...
* lowercase, 全小写，但是如果命名不好，会影响人肉眼可读性

命名风格没有好坏之分, 只有适合的场景, 以及最重要的是要 **风格统一**, 跟团队保持统一, 跟开源社区保持统一

## 服务名如何命名

首先前面提到，服务名一个很重要的作用是，**用于构成其他资源名**，进而我们可以通过资源名中包含的服务名来识别 该资源属于哪一个服务.
我们互联网服务中可能会使用到各种各样的资源，会使用服务名的资源包括但不限于
  * 各种云资源命名，Tag名
  * 容器/K8S相关的各种资源
  * 程序、脚本的变量名/方法名
  * 配置项
  * hostname，domain name

各种资源都会有各种限制，例如
  1. domain name不允许有`_`下划线,同理会用到hostname或者domain name的地方，都不允许，例如aws的alb名
  1. 部分web服务器例如nginx，会默认drop掉name中包含`_`的http header, 因此http header 不允许有`_`
  1. 有的场景严格区分大小写，有的场景仅能用小写、仅能用大写，或者会自动转换
  1. 几乎所有编程语言变量名，不允许包含短横线`-`， 例如你不能将变量名命名为 FOO-SERVICE-API
  1. 各种全文搜索引擎会对`-`进行分词, 因此，如果你搜索 foo-service 你会搜到包含foo或者包含service的结果，如果你搜索fooservice那只有fooservice结果，这个在日志搜索里经常遇到

由于以上限制存在，很多工具或者框架，会做一些自动转换处理，例如 snake_case变CamelCase, 大写变小写，自动去掉`-`或者`_`， 我们在编写脚本的时候也要按需做这些处理，以适应要求。


所以，有没有一种可以规避以上大部分限制的，有，那就是全部小写，**牺牲一丢丢人眼可读性换取极大提升程序可处理性**.

全小写可读性不好，是否可以用 kebab-case呢? 假如 我们有3个服务 foo, foo-api, foo-api-gateway...
* 你搜索foo的日志的时候，会找不到你想要的
* 你用terraform创建ec2的时候，你的Name Tag可能是下面的, 当你用脚本想通过解析tag获取它属于哪个serice的时候，会比较困难
  * prod-orgname-foo-ec2
  * prod-orgname-foo-api-ec2
  * prod-orgname-foo-api-gateway-ec2
  如果是这样，是不是好很多
  * prod-orgname-foo-ec2
  * prod-orgname-fooapi-ec2
  * prod-orgname-fooapigateway-ec2

## 比较好的服务命名规范

好的名称目标是
1. 保持良好的人眼可读性, 对人友好
1. 规避大部分情况的限制, 对程序友好

因此可以使用如下规则
1. 使用1~3个有明确意义的全小写单词或者数字组成(但不能用数字开头), 见名知意，不要超过20个字符, 不要使用生僻词。 好的例子: `mysql` `github` `springbootadmin` `phpmyadmin` `elasticsearch` `alertmanager` `djangorestframework`. 不好的例子：`kubernetes`, 太生僻，以至于大家都用`k8s`取代
1. 不要使用特殊代号, 除非是轮子类项目
1. 推荐使用 业务名+功能名的方式，比如 `paymentapi` `paymentjob` `userconsole` `usercenter` `orderapi` `orderui`
1. 可以包含数字，数字可以起到了分隔符的作用，有利于提高可读性，例如可以用数字4来做for的谐音, 可以用于有特定关系的应用，例如`log4j`; 2作为to的谐音, 可以用于有转换功能的应用, 例如`json2yaml`
1. 避免使用一些太general的词汇，比如 `mainapp`, `defaultapp`, `webapp`
1. 避免使用new/old含有历史信息的词汇，比如如果你重构了一个全新版本的foo，起名为newfoo，如果再有重构呢？可以用 foo2 foo3
1. 慎用缩写，除非是well known的缩写，例如，用`k8s` `kube`缩写来代指 kubernetes，因此比较好的名字有 `kubectl` `kubectx`, 再例如用 `gw` 代替 `gateway`, `svc` 代替 `service`, `es` 代替 `elasticsearch`, `mgmt` 替代 `management`

## 常见功能词汇列表参考

加上你的业务词汇，就是一个很好的服务名

| 功能 | 词汇 | 缩写 | 例子 |
| --- | ---- | ---- | --- |
| http api | api | api | orderapi |
| api 网关 | gateway | gw | ordergw/ordergateway/orderapigw|
| web页面 | web | web | orderweb |
| csr/ssr的前端页面 | ui | ui | orderui |
| 管理后台 | console | console | orderconsole |
| 管理后台 | admin | admin | orderadmin |
| 管理后台 | management | mgmt | ordermgmt |
| 后台job类 | job | job | orderjob |
| 后台job类 | task | task | ordertask |
| 后台job类 | cron | cron | ordercron |
| 后台job类 | runner | runner | orderprocessrunner |
| 调度器类 | scheduler | scheduler | fooscheduler |
| 认证/授权 | auth | auth | userauth |
| 通知类 | notify | notify | ordernotify |
| 通知类 | message | msg | ordermsg |
| 告警类 | alert | alert | useralert |
| 数据处理类 | data | data | userdata |
| 平台类| platform | platform | userplatform |
