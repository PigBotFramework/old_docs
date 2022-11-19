# imageutils
猪比的`imageutils`基于[nonebot-plugin-imageutils](https://github.com/noneplugin/nonebot-plugin-imageutils)魔改，具体用法参见[nonebot-plugin-imageutils](https://github.com/noneplugin/nonebot-plugin-imageutils)仓库。该仓库代码中注释写的非常详细，相信不用多余的解释你也能看懂。  
这里只强调几个魔改后的地方  

# 引入
```
import sys
sys.append("../..")
from imageutils.build_image import BuildImage, Text2Image
```

# `save`等方法的返回
- `save`方法会返回BytesIO类型的图片
- `save_jpg`和`save_png`方法会返回保存后的路径
  
  
那么说到返回路径，这里就有必要讲一下路径问题  

# 路径问题
云端处理器位于Docker容器中，具体的目录结构如下：
```directory structure
/
|- /pbf
  |- ./plugins        # 插件们
    |- ./foo          # 你的插件包
  |- ./imageutils     # imageutils
  |- ./logs           # 日志文件
  |- ./nsfw           # NSFW
  |- ./resources      # 资源文件
    |- ./createimg    # 创建的图片
    |- ./fonts        # 字体们
    |- ./images       # 静态图片（用于生成表情包等）
  |- ./bot.py         # BOT基类
  |- ./fabot.py       # FastAPI
```
由此可见，`imageutils`位于同名文件夹中，而生成的图片会保存到`/pbf/resources/createimg/`中。**具体的文件名为`time.time()生成的时间戳`+`.jpg/.png后缀`。**比如`save_jpg`可能会返回`/pbf/resources/createimg/1668035806.882322.jpg`  

# 访问静态资源
为了方便别的设备访问，访问`resources`可通过静态网站[小猪比机器人媒体资源站](https://resourcesqqbot.xzy.center/)进行访问  
例如`/pbf/resources/createimg/1668035806.882322.jpg`，此时可通过`https://resourcesqqbot.xzy.center/createimg/1668035806.882322.jpg`访问