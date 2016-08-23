###Maven和Eclipse集成###

- 下载好maven，并配置环境变量(可配置)
- 设置好maven的repo目录，在maven目录下的conf/setting.xml，设置好localRepository如:***<localRepository>D:\aron-ruan\apache-maven-3.3.9\repo</localRepository>***
- 然后在eclipse下载相关的插件（Maven Profiles Management,另外，SVN的插件为Subclipse)
- 将安装成功的maven暴露给eclipse，在windows/preference/maven/installation下加入你安装maven的根目录

####表现####
- File=>New =>maven project=>继续下去利用.maven-archetype-quickstart即可