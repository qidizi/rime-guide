# 我的mac rime自定义配置

## 关于yaml

**注意下方的表述并非yaml文档翻译,属于我随意叫法,能看懂即可**

* 使用空格来表示层级关系,多少个空格无要求,仅要求同一层次空格数相同即可,否则就是属于不同层级了,如:

```yaml
爷:
    父:
        子: 2个
    叔:
        堂弟: 1个
        小辈表叔:
            汽车: 3台  
```

* 有空格的键与值

```yaml
symbols:
  "/0 1": "空 格"
```
* 键与值使用`:`号分隔,与值之间需要有空格
正确拼写:

```yaml
键: 值
```

错误无效写法:
```yaml
键:值
```

* 注释,给人类看的,不解析,无任何配置作用
```yaml
#这是方案名
name: 拼音  # 这是方案的名称

```

* 列表(数组)

```yaml
half_shape:
    "*": ["*", "#"] # 这个值是一个列表

schema_list:
    # 下面3个组成列表
    - py
    - qidizi
    - en
```

* 仅想改变某项如何做?

官方配置都叫`*.schema.yaml`,你可以直接使用`sublime`打开修改它保存,再点击部署即可生效,
但是这个做法在重置或是升级后,你的修改将丢失.
正确的做法是创建一个空白的`*.custom.yaml`,是的,直接把原文件名中的`schema`改成`custom`即可,
再在首行加上`patch:`,即可,如:

官方原件`*.schema.yaml`
```yaml
name: 拼音
version: 0.1
author: rime
```

我的自定义防覆盖文件`*.custom.yaml`

```yaml
patch:
    # 只改变那么,其它保持原样
    name: 宇宙拼音
```


* 整个层级干掉重造与只动层级的某个键(注意,根属性无此问题)

假设前人设置如下:
```yaml
punctuator:
    half_shape:
      "*": "88"
    full_shape:
      "*": "99"
    use_space: true 
```

丢失整个,重造方式

```yaml
patch:
    punctuator:
      use_space: false 
      # half_shape与full_shape已丢失或叫清空也行
```

只动指定,其它保持
```yaml
patch:
    "punctuator/use_space": false
      # half_shape与full_shape还在,且是原来值
```

* 我的修改(patch或叫补丁)何时生效?
修改后,点击部署,查看日志或是试用是否达到预期

* 我的修改到哪里去了?
查看配置目录下的动态生成的build对应*.schema.yaml,它把custom与schema合并一起的最终结果

* 更多语法建议直接查看官方自带的yaml,yaml官方文档没有必要看,又不是改rime程序

---

## 关于`发行版`

就是在[librime](https://github.com/rime/librime)输入法框架基础上，针对windows、
linux、mac os等编译出直接下载运行安装就能使用的输入法，每个人写的都算做一个发行版，而网络上讨论
都是基于以下几大发行版本进行讨论的：  

* [同文安卓輸入法平臺](https://github.com/osfans/trime)

  可能经常会提到`trime.yaml`  
  配置目录sdcard/rime  
  
* [squirrel【鼠鬚管】Rime for macOS](https://github.com/rime/squirrel) 
 
   可能经常会见到提`squirrel.yaml`   
   日志路径 `echo $TMPDIR/rime.squirrel.* `，2019－08－22止，发现它只会在启动时创建当天日志，当你电脑
   持续到次天时，而这时点击部署却没有看到今天日志，这时可以手动运行`/Library/Input\ Methods/Squirrel.app/Contents/MacOS/Squirrel`，
   创建今天日志（按ctrl+c）终止；  
   配置目录一般在`～/Library/Rime`,也可以点击右上角输入法`鼠鬚管`点击`用户设定...`打开； 
   
* [weasel 【小狼毫】Rime for Windows](https://github.com/rime/weasel)  
   
   可能会经常见到提`weasel.yaml`  
   它的日志路径 `%TEMP%\rime.weasel.*`  
   用户配置目录：用戶文件夾的默认路径 %APPDATA%\Rime。也可以通过「开始菜单＼小狼毫输入法＼用户文件夹」打开；   
   
* [ibus-rime【中州韻】Rime for Linux/IBus  ](https://github.com/rime/ibus-rime)  

    它的日志路径`/tmp/rime.ibus.*` 
    用户目录：`~/.confg/ibus/rime`  
    
更多发行版本像`fcitx-rime`、不同发行版本相关目录和差异请到相应发行版本看说明；  

---

## 如果你也是mac五笔拼音使用者,可以直接使用我这个配置

**下面操作都是以mac的squirrel配置来做测试的，其它发行版本可以参考**   
    
* 进入配置目录：`cd ～/Library/Rime`  
* 不需要其它文件干扰我测试，清空我的配置目录 `rm -ri *`  
* 克隆我的配置到配置目录：`git clone 我的项目地址`  
* 克隆五笔拼音方案相关文件：`/Library/Input\ Methods/Squirrel.app/Contents/MacOS/rime-install wubi pinyin-simp` 
* 给每个带schema.yaml的文件加上自定义文件，假设是`pinyin_simp.schema.yaml`，就创建`pinyin_simp.custom.yaml`，
同时给这个文件写上一行`patch:`，这样日志中才不会出现`找不到某某自定义文件的提示｀,我已做,除非有新schema文件出现,不做也可以不影响使用,但是会
在log中出现影响调试  
* 点击部署 
* 试用输入 

---
## 重置到安装最初状态

* 进入配置目录：`cd ～/Library/Rime`  
* 不需要其它文件干扰我测试，清空我的配置目录 `rm -ri *`  
* 创建引子文件`default.custom.yaml`,仅加一行`patch:`  
* 点击部署 
* 研究build目录
* 试用输入 

## 只有五笔拼音最官方配置  

* 进入配置目录：`cd ～/Library/Rime`  
* 不需要其它文件干扰我测试，清空我的配置目录 `rm -ri *`  
* 创建引子文件`default.custom.yaml`,加上

```yaml

patch:
  # 输入法方案
  schema_list:
    # 五笔拼音
    - schema: wubi_pinyin
``` 
 
* 克隆五笔拼音方案相关文件：`/Library/Input\ Methods/Squirrel.app/Contents/MacOS/rime-install wubi pinyin-simp` 
* 给每个带schema.yaml的文件加上自定义文件，假设是`pinyin_simp.schema.yaml`，就创建`pinyin_simp.custom.yaml`，
同时给这个文件写上一行`patch:`，这样日志中才不会出现`找不到某某自定义文件的提示｀,不做也可以不影响使用,但是会
在log中出现影响调试  
* 点击部署 
* 研究整个部署目录与build合并后官方默认值，每次修改custom点击部署后build内容变化
.dict.yaml文件属于字典，可以打开看里面就是码表，除非你想弄个6笔输入法或是红人键盘，就可以研究  
build目录中bin是rime根据.dict.yaml优化后生成的2进制文件 
build下的squirrel.yaml，就属于mac发行版本专有名字，其它系统或是其它人发行叫法可能不同。 

* 试用输入 


## 在上面基础上自定义

目前自定义只需要打开schema.yaml文件研究即可，
同时我的版本库中会有对应我研究过的键值用途、 注意事项等注释说明供参考

本配置文档研究与编写都比较匆忙，不足之处欢迎留言指正，同时也希望其它朋友贡献心得共同维护。

其它参考 https://github.com/LEOYoon-Tsaw/Rime_collections/blob/master/Rime_description.md


