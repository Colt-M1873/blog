<!-- 初版时间 2023年07月20日 16:36:25 
链接：http://3ms.huawei.com/km/blogs/details/14492391
-->



**问题：在IDEA中点击使用Maven组件能执行各项命令，但在Terminal中手动输入mvn不能正常执行：**

--------------

**解决：**在IDEA正确配置Maven的setting.xml后，在IDEA中点击使用Maven组件能执行各项命令，但在Terminal中mvn不能正常执行，原因是IDE的maven和命令行maven使用的不是同一套配置，IDEA的配置仅限于IDEA内部，在IDEA内部点击Maven组件时会使用你指定的settings.xml，而在终端里，你的配置仍然是maven的默认配置，没有修改，因此IDEA能用而Terminal不能正常用。

idea能手动指定settings.xml的路径，而命令行中使用mvn则会到以上两个默认路径中去寻找配置文件。

在不想修改默认路径中的配置文件的前提下，
上述问题解决方法是在命令行中手动指定和配置setting.xml文件。

`mvn install --settings D:\someproject\setting.xml`

--------------


**参考：[Stackoverflow](https://stackoverflow.com/questions/1261215/maven-command-to-determine-which-settings-xml-file-maven-is-using)**
> Maven always uses either one or two settings files. The **global settings** defined in (`${M2_HOME}/conf/settings.xml`) is always required. The **user settings **file (defined in `${user.home}/.m2/settings.xml`) is optional. Any settings defined in the user settings take precedence over the corresponding global settings.
You can override the location of the global and user settings from the command line, the following example will set the global settings to c:\global\settings.xml and the user settings to c:\user\settings.xml:

```
mvn install --settings c:\user\settings.xml 
     --global-settings c:\global\settings.xml
```
> Currently there is no property or means to establish what user and global settings files were used from with Maven. To access these values, you would have to modify MavenCli and/or DefaultMavenSettingsBuilder to inject the file locations into the resolved Settings object.


另： `mvn help:effective-settings` 即命令行查看当前生效的setting，但只在maven项目文件夹下有用，很局限。
