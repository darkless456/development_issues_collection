```text
主要需要修改的文件为：
IDEAHome/bin/idea.properties

找到内容为 idea.config.path 与 idea.system.path 的选项，放开注释，修改为你想要的路径就好了。

    idea.config.path：IDEA 的配置文件目录，主要有安装的插件，自定义的配置等
    idea.system.path：IDEA 的缓存文件目录，主要有各个项目单独的配置，项目的索引等

    注：${idea.home.path} 代表的是 IDEA 程序的根目录。
    例如：IDEA 的文件夹是：C:/Users/rxliuli/Program/ideaIU-2018.1.6.win/。 那么，${idea.home.path}/.IntelliJIdea/ 代表的就是 C:/Users/rxliuli/Program/ideaIU-2018.1.6.win/.IntelliJIdea/

以上便是使用 IDEA 设置配置文件的位置的方法，现在可以将 IDEA 程序和配置一起打包解压即用了呢
```