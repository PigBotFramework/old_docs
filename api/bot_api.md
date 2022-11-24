# BOT类接口列表
> 在[教程](guide/start)中我们介绍了制作机器人插件需要使用父类BOT类。  但是在里面我们没有介绍更多的接口。  
  
在这里，列出了BOT类中所有的接口和类变量  
**以下内容`2022-11-10`更新，可能有部分变动，若遇问题请联系作者！**  
  
# 所有接口 
**参数前带`*`的表明该参数必须传入，不带则可以不传**  

| 接口名 | 参数 | 描述 | 备注 |
| ------ | ---- | ---- | ---- |
| `WriteCommandListener` | `func`执行的函数, `args`储存的参数, `step`步骤 | 注册/修改指令监听器 | 使用方法详见[问与答](guide/ask) |
| `RemoveCommandListener` | 无需要传递的参数 | 删除指令监听器 | 使用方法详见[问与答](guide/ask) |
| `ReadCommandListener` | 无需要传递的参数 | 读取指令监听器 | 使用方法详见[问与答](guide/ask) |
| `requestInit` | *`se`上报的原始内容, *`uuid`机器人UUID | 初始化各项 | 该方法应该由FastAPI调用，插件内请勿调用 |
| `GetPswd` | *`uuid`机器人uuid | 获取某UUID的对应通信密钥 | 插件不应该随意获取机器人实例的通信密钥 |
| `GetConfig` | *`uuid`机器人UUID, *`key`键, *`value`值, *`template`SQL语句模板, `sql`附加SQL语句 | 获取配置 | 该方法用来快速获取配置（懒人专用） |
| `encryption` | *`data`DATA, *`secret`SECRET | `sha1`加密 | 进行SHA1加密，主要用于验证上报是否真实 |
| `selectx` | *`sqlstr`SQL查询语句, `params`execute参数, `host`主机地址, `user`用户名, `password`密码, `database`数据库 | 执行查询语句并返回查询结果 | 为防止SQL注入，请将参数用`%s`代替（无需带`""`），然后放在`params`中（`params`为元组），执行execute时会自动替换。多个查询结果返回列表，只有一个返回字典 |
| `commonx` | *`sqlstr`SQL执行语句, `params`execute参数, `host`主机地址, `user`用户名, `password`密码, `database`数据库 | 执行SQL语句 | 为防止SQL注入，请将参数用`%s`代替（无需带`""`），然后放在`params`中（`params`为元组），执行execute时会自动替换。无返回值 |
| `sendRawMessage` | *`content`消息内容 | 发送原始消息 | 发送消息推荐使用`send`方法 |
| `sendChannel` | *`content`消息内容 | 发送频道消息 | 发送消息推荐使用`send`方法 |
| `sendInsertStr` | *`content`消息内容 | 在内容里随机插入字符 | 返回插入后的消息内容 |
| `send` | *`content`消息内容, `coinFlag`是否随机添加好感度, `insertStrFlag`是否随机插入字符, `retryFlag`是否重试, `translateFlag`是否翻译 | 发送消息 | 详见[send发送消息](api/send) |
| `SendOld` | *`uid`用户QQ号, *`content`消息内容, `gid`群号 | 指定用户或群聊发送消息 | 不指定群号则发送给QQ用户，指定群号则忽略`uid` |
| `CrashReport` | *`message`消息内容, `title`标题 | 错误报告 | 可在[日志](https://qb.xzy.center/logging)查看 |
| `CallApi` | *`api`接口名, *`parms`参数（字典形式） | 请求(GOCQ)API | 返回json解析后的字典 |
| `weijinWhileFunc` | *`message`消息内容 | 检查违禁词 | 返回True或False |
| `checkWeijin` | *`weijinFlag`是否检查违禁词 | 检查违禁词并处理 | `weijinFlag`为真时，检查到违禁词就提醒+撤回，假的时候，只会检查辱骂机器人并回击。初始化时会调用该函数 |
| `GroupInit` | 无需要传递的参数 | 初始化群组 | 该方法应该由`requestInit`调用，插件内请勿调用 |
| `checkCoin` | 无需要传递的参数 | 检查好感度 | 未注册返回`-1`，否则返回用户好感度 |
| `addCoin` | `value`好感度值 | 添加好感度 | 不带入`value`则使用randint |
| `delCoin` | `value`好感度值 | 删除好感度 | 不带入`value`则使用randint |
| `getCQValue` | *`key`键, *`message`消息内容 | 获取CQ码的值 | 默认获取第一个CQ码的值 |
| `getCQArr` | *`message`消息内容 | 获取CQ码数组 |  |
| `keywordPair` | *`replyKey`匹配规则, *`message`内容 | 匹配（加回复的规则） |  |
| `findObject` | *`key`键, *`value`值, *`ob`对象 | 在字典套数组(`[{}]`)中查找对象 |  |
| `getNumByObject` | *`key`键, *`value`值, *`ob`对象 | 查找键值对并返回在数组中的下标 |  |
| `generate_code` | *`num`长度 | 生成指定长度的验证码 |  |
| `openFile` | *`path`文件路径 | 获取文件内容 |  |
| `writeFile` | *`path`文件路径, *`content`内容 | 写入文件 | 不知道为啥，此方法不能创建文件 |
| `reply` | 无需要传递的参数 | 智能回复 | 使用`ChatterBot`机器学习配合青云语料库 |
| `sendImage` | 无需要传递的参数 | 生成图片 | 内容为`self`的`message`类变量 |
| `chushihuacd` | 无需要传递的参数 | 初始化分类菜单 |  |
| `cd3` | 无需要传递的参数 | 发送分类菜单某类的具体指令列表 |  |
| `KeywordExcept` | *`replyKey`替换关键词, *`message`消息内容 | 匹配关键词 | 该方法应由`keywordPair`调用 |
| `KeywordReplace` | *`replyContent`回复内容 | 替换回复内容中的`%%` |  |
| `KeywordOr` | *`replyKey`替换关键词, *`message`消息内容 | 匹配关键词 | 该方法应由`keywordPair`调用 |
| `KeywordAnd` | *`replyKey`替换关键词, *`message`消息内容 | 匹配关键词 | 该方法应由`keywordPair`调用 |
| `sendKeyword` | *`replyContent`回复内容 | 发送关键词回复 |  |
| `translator` | *`text`翻译内容, `from_lang`源语言, `to_lang`目标语言 | 谷歌翻译 | 准确率感人 |
| `execPlugin` | *`func`运行的字符串 | 运行插件中的方法 | 返回方法的返回值 |
| `execPluginThread` | *`func`运行的字符串 | 多线程运行插件中的方法 | 无返回值！！！ |
| `checkPromiseAndRun` | *`i`指令对象, `echoFlag`是否发送权限不足时的提示, `senderFlag`是否检查sender相关的权限 | 检查权限并运行 | 运行即调用`execPlugin`运行 |
| `start` | 无需要传递的参数 | 分发监听 | 除`command`监听。插件不应该调用该方法 |
| `bot` | 无需要传递的参数 | 分发指令、检查违禁词、关键词回复、菜单等各种基本功能 | 相当于一个`message`监听 |

  
# 所有类变量
| 字段名 | 可能的值（数据类型） | 解释 |
| --- | --- | --- |
| self.`se` | {...} | 接到gocq的上报原数据 |
| self.`userCoin` | -1 | 用户（只发送该消息的用户）的好感度 |
| self.`userInfo` | {...} | 该用户的各种信息 |
| self.`groupSettings` | {...} | 该群组的各项设置，若不是群组则为None |
| self.`args` | [...] | 用户发送的消息的各项参数 |
| self.`message` | "..." | 经过处理后的消息 |
| self.`isGlobalBanned` | {...} | 是否被全局屏蔽，没有则为None |
| self.`uuid` | "..." | 该机器人实例的UUID |
| self.`botSettings` | {...} | 该机器人实例的设置 |
| self.`pluginsList` | [...] | 该机器人实例已获取的插件 |
| self.`commandmode` | [...] | 该机器人实例已获取的插件类型 |
| self.`ocrImage` | "..." | 消息中的图片的OCR结果 |

**请不要在`__init__`方法中读取以上类变量，否则他们全都是None或空**  