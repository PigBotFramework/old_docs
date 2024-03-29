# 注意
**该版本为PBFv3文档，介于v3版本接口混乱，性能落后，开发方式落后，该版本已经停止更新与维护，请移步新的[v4版本](https://github.com/PigBotFramework/v4)（文档有待编写）！**

---

# PigBotFramework Docs
小猪比机器人框架 开发文档

# 这是什么
小猪比机器人使用Python的`FastAPI`库编写，提供一个基本框架。开发者可以自由地基于这个框架编写插件。

# 猪比生态
猪比可以快速地处理各种消息以及上报（基于`OneBot`(v11)标准）。猪比是开放的，任何人都可以接入猪比，只需要自己挂一个`go-cqhttp`即可。接入后，用户可以在面板管理机器人实例的各项配置，也可以安装他想要的插件。具体的实例创建以及接入方法不在此赘述。  
而开发者可以按照此文档开发基于PBF的插件，并提交上架插件市场。上架后，用户就可以将该插件安装到自己的机器人实例上。

# 您需要的基础
- Python基础
- MySQL基础
- [go-cqhttp的使用](https://docs.go-cqhttp.org/)

# 开始
> 如果您确保已具备了各项基础，请开始您的开发  
  
[开始](guide/start)

> 本文档正在编写，有些内容未完成编写
