# idea目录结构

## structure

![在这里插入图片描述](https://i.loli.net/2021/03/09/rR2iTBnUQ1DSGCH.jpg)



### project

![在这里插入图片描述](https://i.loli.net/2021/03/09/W79wrbfo4iMuaLl.jpg)

### Modules

![在这里插入图片描述](https://i.loli.net/2021/03/09/5ZSHyKfrau4N2JF.jpg)

- Name：项目名称
- Souces：这里对Module的开发目录进行文件夹分类，就是说这个module里有什么内容，说明了不同性质的内容放在哪里。
  注意，这些不同内容的标记代表了一个标准Java工程的各项内容，IntelliJ就是根据这些标记来识别一个Java工程的各项内容的，比如，它会用javac去编译标记为Sources的源码，打包的时候会把标记为Resources的资源拷贝到jar包中，并且忽略标记为Exluded的内容。左边显示的是在选中内容的预览。
- Paths：为模块配置编译器输出路径，还可以指定与模块关联的外部JavaDocs和外部注释的位置。
- Dependencies：在此选项卡上，您可以定义模块SDK并形成模块依赖关系列表。

#### Source

对module的开发目录进行文件夹分类，以让idea明白怎么去对待他们，明确哪些是存放源代码的文件夹，哪些是存放静态文件的文件夹，哪些是存放测试代码的文件夹，哪些是被排除编译的文件夹。
![在这里插入图片描述](https://i.loli.net/2021/03/09/6gy1fzThc4CoXs3.jpg)
Language level：语言级别列表，使用此列表为模块选择Java语言级别。可用选项对应于JDK版本。
![在这里插入图片描述](https://www.pianshen.com/images/849/d2d232e0fca839d78e0133d63951c9c9.png)

- Sources：源代码存放的文件，蓝色。
- Tests：设置测试代码存放的文件件，绿色。
- Resources：一般对应着Sources文件，一般放配置文件，如：log4j.properties，application.yml。
- Test Resources：这个对应着Tests文件夹，存放着Tests代码的配置文件。
- Excluded：设置配出编译检查的文件，例如我们在project模块设置的out文件夹。

#### Paths

![在这里插入图片描述](https://www.pianshen.com/images/930/87958fcc4ff38c4a22bddaf664046a92.png)

- Compiler output：编译输出路径。

1. Inherit project compile output path：继承项目编译输出路径 选择此选项以使用为项目指定的路径。即上面在Project选项中设置的out文件路径。
2. Use module compile output path:使用模块编译输出路径。
   Output path：编译输出路径。
   Test output path：测试代码编译输出路径。
   Exclude output paths： 排除输出路径，选中此复选框可以排除输出目录。

- JavaDoc：使用可用控件组合与模块关联的外部JavaDocs存储位置的列表。

- External Annotations：外部注释。使用新 和删除 管理与模块关联的外部注释的位置（目录）列表。

#### Dependencies

在此选项卡上，您可以定义模块SDK并形成模块依赖关系列表。
![在这里插入图片描述](https://i.loli.net/2021/03/09/Qn9DJOvdmiLbGX6.jpg)

- Module SDK：模块SDK，选择模块SDK。
  （要将项目SDK与模块相关联，请选择Project SDK。请注意，如果稍后更改了项目SDK，模块SDK将相应更改。
  如果所需SDK不在列表中，请单击“ 新建”，然后选择所需的SDK类型。然后，在打开的对话框中，选择SDK主目录，然后单击确定。
  要查看或编辑所选SDK的名称和内容，请单击编辑。（SDK页面将打开。）
- 依赖列表(勾选表示依赖会传递，即引用该modules的modules也会拥有此依赖)

### Libraries

在此选项卡上，您可以定义模块SDK并形成模块依赖关系列表。
![在这里插入图片描述](https://i.loli.net/2021/03/09/qSTMyKz1OFBRjU4.jpg)

### Facets

表示这个 module 有什么特征，比如 Web，Spring 和 Hibernate 等；
![在这里插入图片描述](https://www.pianshen.com/images/371/aeacf4e86678faf641fbcf5c949ead6b.png)

### Artifacts

Artifact 是 maven 中的一个概念，表示某个 module 要如何打包，例如 war exploded、war、jar、ear 等等这种打包形式；
一个 module 有了 Artifacts 就可以部署到应用服务器中了！
在给项目配置 Artifacts 的时候有好多个 type 的选项，exploed 是什么意思？
explode 在这里你可以理解为展开，不压缩的意思。也就是 war、jar 等产出物没压缩前的目录结构。建议在开发的时候使用这种模式，便于修改了文件的效果立刻显现出来。默认情况下，IDEA 的 Modules 和 Artifacts 的 output 目录 已经设置好了，不需要更改，
打成 war 包 的时候会自动在 WEB-INF 目录 下生产 classes 目录 ，然后把编译后的文件放进去。
![在这里插入图片描述](https://i.loli.net/2021/03/09/9YhG2NlVq5rzxHg.jpg)

### SDKS

系统开发工具 ，全局 SDK 配置 。
![在这里插入图片描述](https://www.pianshen.com/images/512/b2441304e9045ccf01940a3747fa69e8.png)

### Global libraries

全局类库，可以配置一些常用的类库。
![在这里插入图片描述](https://i.loli.net/2021/03/09/era6icvbgM9NdUj.jpg)

### Problems

问题，在项目异常的时候很有用，可以根据提示进行项目修复（FIXED）。
![在这里插入图片描述](https://www.pianshen.com/images/293/2937635da2992fdedd33e47cb74ed23d.png)

### idea的部署方式

- 1.编译，IDEA在保存/自动保存后不会做编译，不像Eclipse的保存即编译，因此在运行server前会做一次编译。编译后class文件存放在指定的module编译输出目录下。
- 2.根据artifact中的设定对目录结构进行创建；
- 3.拷贝web资源的根目录下的所有文件到artifact的目录下；
- 4.拷贝编译输出目录下的classes目录到artifact下的WEB-INF下；
- 5.拷贝lib目录下所需的jar包到artifact下的WEB_INF下；
- 6.运行server，运行成功后，如有需要，会自动打开浏览器访问指定url。