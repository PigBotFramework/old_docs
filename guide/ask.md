# 互动式问答
有很多情景，用户不只是需要发送一条指令，而是发送多条消息。例如：`加回复`指令。  
在这种情况下，互动式问答就是必不可少的一个功能。
  
# 指令下发
在开始使用互动式问答之前，您需要明白猪比的指令是如何下发的。  
  
当收到gocq上报后，`post_data`会实例化bot类，调用`requestInit`方法进行初始化。（这里不使用`__init__`方法是因为如果使用`__init_`bot模块后面的flask部分就会起冲突）  
然后调用`bot`方法。`bot`的功能之一就是下发指令。  
  
这里不得不提到一个东西：`指令监听器（commandListener）`（其实它的实质是一个列表）。下发指令时，PBF会先查找指令监听器中有没有存在的互动式问答。如果有，就会调用监听器中设置的函数，反之，则会按照正常的方式判断是否触发某个指令。
  
# 开始
使用互动式问答需要掌握三个方法：`WriteCommandListener`、`ReadCommandListener`、`RemoveCommandListener`。
  
让我们来看看`加回复`是怎么写的：
```python
import ...
class keyword(bot):
    ...
    def addKeyword(self):
        # 获取需要的信息
        uid = self.se.get('user_id')
        gid = self.se.get('group_id')
        message = self.message
        
        # 查找指令监听器，看有没有存在的监听
        ob = self.ReadCommandListener()
        if ob.get('code') == 404: # code字段404则为没有
            self.WriteCommandListener('keyword.addKeyword()', {'key':' ','value':' '}) # 注册指令监听器
            self.send('开始添加关键词回复，在此期间，你可以随时发送“退出”来退出加回复\n请发送触发该回复的关键词') # 提示开始添加
            self.send('提示：加回复规则详见：\nhttps://blog.xzy.center/archives/21/')
            return True # 记得return
        
        # 否则code不是404则有已注册的监听器
        # 说明正在添加回复
        step = int(ob.get('step')) # step是当前进行到的步数，记得int一下
        args = ob.get('args') # 这个是储存的参数
        
        if step == 1: # 如果是第一步
            self.WriteCommandListener(args={'key':message,'value':' '}) # 将消息内容写为触发关键词
            self.send('{0}知道了呢，那我该回答啥呢qwq？'.format(self.botSettings.get('name')))
        
        if step == 2: # 如果是第二步
            self.WriteCommandListener(args={'key':args.get('key'),'value':message}) # 将消息内容写为回复内容
            self.send('好的呢，你希望用户的好感度至少为几的时候我会回答这条消息呢\n提示：未注册用户好感度-1，也就是说你如果想让该回复适用于所有用户请发送-1')
        
        if step == 3: # 如果是第三步
            key = args.get('key') # 获取上两步储存的参数
            value = args.get('value')
            self.RemoveCommandListener() # 删除该监听器
            
            # 将该回复添加到数据库
            if uid == self.botSettings.get('owner'):
                sql = 'INSERT INTO `botKeyword` (`key`, `value`, `state`, `uid`, `coin`, `uuid`) VALUES ("'+str(key)+'", "'+str(value)+'", 0, '+str(uid)+', '+str(message)+', "'+str(self.uuid)+'");'
            else:
                sql = 'INSERT INTO `botKeyword` (`key`, `value`, `state`, `uid`, `coin`, `uuid`) VALUES ("'+str(key)+'", "'+str(value)+'", 1, '+str(uid)+', '+str(message)+', "'+str(self.uuid)+'");'
            self.commonx(sql)
            self.send('恭喜你，现在只需要等待我的主人审核通过后就可以啦！')
```
  
上面这个例子我已经把指令监听器的使用介绍的很全面了。不过你可能还不知道这三个方法需要的参数：  

# 具体使用

- WriteCommandListener  
他是这样定义的：
```python
def WriteCommandListener(self, func=None, args=None, step=1):
```
可以看到，这三个参数都可以不带。
  
在第一次使用该函数（即注册指令监听器时），我们通常带入`func`和`args`。`func`用来指定下一次用户发消息时指令监听器要调用哪个函数；`args`则为储存的数据
  
通常情况下，我们如果不指定`func`或`args`时，指令监听器不会对他们作出任何更改，他们会继续保留原值；而如果我们不指定`step`，他则会递增（注册指令监听器时递增为1）
  
- ReadCommandListener  
他的使用非常简单，无需参数（`self`除外），会返回一个json字符串。这个字符串中包括注册指令监听器时带入的所有内容（即`func`、`args`等）。类型为字典类型。
  
- RemoveCommandListener  
他用于删除一个指令监听器。同样无需参数（`self`除外）。
  

# 注意
- 在所有的互动式问答完成后，请务必调用`RemoveCommandListener`删除指令监听器。否则指令下发时PBF会一直调用监听器中注册的函数，造成用户无法正常使用机器人、机器人也无法正常处理消息。
- 用户在进行互动式问答时，可随时发送`退出`来删除当前的监听器。所以您需要尽量避免用户在进行互动式问答时误发`退出`。
  
  
# 下一节
> 如果您已经掌握了指令监听器的使用，请前往下一节  
  
[下一节-json配置与MD文件](guide/json_and_md)