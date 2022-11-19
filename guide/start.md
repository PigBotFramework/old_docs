# 开始
> 这个引导式教程将会引导您编写您的**第一个**插件  
  
让我们假设我们现在要编写一个MD5加密的功能。
- 首先我们要创建一个`包`（这意味着我们要创建一个文件夹，并且必须包含`__init__.py`文件）  
假设这个插件名叫foo，那么目录结构是这样的：
```directory structure
PBF
|- [folder]imageutils
|- plugins
   |- foo
      |- __init__.py
      |- main.py
      |- commands.json
      |- data.json
      |- readme.md
|- [folder]resources
|- bot.py
|- timer.py
|- data.yaml
```
可见，所有的插件都存放在`plugins`文件夹中，而程序会逐个引入这些插件；而插件的主程序则存放在`main.py`中。（事实上，引入语句是这样的：`from plugins.foo.main import foo as foo`）  
您可能会注意到`foo`文件夹中还有两个json文件，至于这两个json文件我将会在之后的文档中详细解释他们。

- 开始编写  
真男人先来看代码：
```python
import sys, hashlib
sys.path.append('../..')
from bot import bot
class foo(bot):
    def md5(self):
        self.send('MD5加密结果：{0}'.format(hashlib.md5(self.message.encode(encoding='UTF-8')).hexdigest()))
```  

# 从头开始
先来看这段代码的第一行。在这一行里，我们引入了实现这个功能必须的两个库--即`sys`和`hashlib`。`hashlib`用于进行MD5加密；`sys`用于添加加载模块的目录，以引入`bot`模块。  

# 接收消息
在第四行，我们创建了一个类。  
需要注意，**类名必须要与这个插件的的包名一致。**比如我们刚刚创建的插件名叫foo，这里的类名也要叫foo  
这个类需要继承父类bot来获取上报的信息和使用API  
同时请注意，**该类不能定义`__init__`方法，如果定义，只能有`self`参数！**  
所有的信息都存放在类变量中，在这里我们只调用了`self.message`类变量，所有类变量见[BOT类](api/bot_api)  
**请不要在`__init__`方法中读取以上类变量，否则他们全都是None或空**  
  

# 发送消息
在上面的目录结构中，我们已经对bot类有所耳闻了，那么这个类的具体方法见[BOT类](bot_api)，这个例子中只用到了发送消息的功能。  
发送消息可以调用`send`方法，只需要传入一个参数即消息内容。  
`send`方法也不仅是简单的一个requests请求，他还有其他功能，详见[send发送消息](api/send)  
  
# 下一节
> 如果您全部掌握了以上内容，恭喜您，您已经可以编写一个**简单插件**了  
  
[下一节-互动式问答](guide/ask)