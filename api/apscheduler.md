# APScheduler
为了某些特殊任务，往往需要定时执行某些任务，如：[tools中的B站爬虫相关功能](https://github.com/PigBotFrameworkPlugins/tools/)  
因此，为了方便创建和统一管理定时任务，PBF提供内置的APScheduler  

# 使用
## 引入
在引入`bot基类`的同时引入`apscheduler`
```python
import sys
sys.path.append("../..")
from bot import bot, apscheduler
```

## 使用
与正常的`apscheduler`用法相同，该怎么用就怎么用  
  
注意事项：
- **需要注意的是，如果您使用`add_job`添加任务，请不要写在插件的类中，也不要写在类的外面，您应该写在特定的类的方法中**  
具体的方法可以在[data.json](https://docsqqbot.xzy.center/#/guide/json_and_md?id=datajson)进行配置  
例如您可以这样写
```json
{
    "name": "实用功具",
    "version": "1.0.1",
    "description": "超多实用工具，包含近一半的指令",
    "author": "xzyStudio",
    "cost": 0.00,
    "init": "tools.addJob()",
    "require": ["test_tick"]
}
```
则此时处理器启动时会自动调用`tools.addJob()`方法，此时您可以在`addJob`中再`apscheduler.add_job` [实例](https://github.com/PigBotFrameworkPlugins/tools/blob/main/main.py#L493)  
  
- `add_job`还需要指定运行的函数，**您可以使用`require`字段（例子见上面），将指定函数import到bot中**  
看例子，例子将`test_tick`函数引入到bot中，此时`add_job`就可以这么写：
```python
scheduler.add_job(test_tick, 'interval', seconds=20, id="test_tick", replace_existing=True)
```
（好像不用require也可以这么写，算了算了不研究了，能用就行）  
  
- **还有，您无需`apscheduler.start()`，他已经被启动了！不要`apscheduler.shutdown()`！！！**
  
- **`init`和`require`不仅仅用于`apscheduler`，如果您有其他需求也可以利用他们**