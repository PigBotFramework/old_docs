# NSFW检测
在很多情境下，我们需要避免机器人发送涉黄图片。而大多数检测接口都要收费。没办法我们只能使用NSFW检测  
  
# NSFW是什么
NSFW意为Not Suitable For Work。  
我们使用Yahoo开源的一套NSFW类的深度神经网络`Caffe`模型代码，魔改成了基于`Tensorflow`的NSFW检测。  
有关Yahoo的NSFW Caffe请见[open_nsfw](https://github.com/yahoo/open_nsfw)  
  
# 如何使用猪比内置的NSFW
首先需要引入NSFW模块：
```python
import sys
sys.path.append('../..') # 添加引入模块目录
from bot import bot... # 引入BOT基类
import nsfw.classify_nsfw as nsfw # 引入NSFW模块
```
然后我们可以使用
```python
nsfw.main("path_to_porn_picture") # path为本地地址，不能使用网络地址。本地地址详见imageutils
```
来进行检测。  
请注意**path为本地地址，不能使用网络地址。本地地址详见[imageutils](/api/imageutils?id=路径问题)**  
返回一个字典：
```python
{
    "sfw": (double),
    "nsfw": (double)
}
```
`sfw`即为Suitable For Work。宜观看的程度  
`nsfw`即为不宜观赏的程度  
  
# 注意
- **一般`nsfw`大于0.8我们就建议屏蔽！**
- **内置nsfw检测的速度较慢，建议尽量减少nsfw检测的次数！**