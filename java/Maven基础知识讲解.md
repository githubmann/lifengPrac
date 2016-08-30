###Maven和Eclipse集成###

- **注意事项**:jdk请使用1.7以上，如果使用1.6会无法生成项目结构
- 下载好maven，并配置环境变量(可配置)，主要是M2_HOME和path=%M2_HOME%/bin
- 设置好maven的repo目录，在maven目录下的conf/setting.xml，设置好localRepository如:***<localRepository>D:\aron-ruan\apache-maven-3.3.9\repo</localRepository>***
- 然后在eclipse下载相关的插件（Maven Profiles Management,另外，SVN的插件为Subclipse)
- 将安装成功的maven暴露给eclipse，在windows/preference/maven/installation下加入你安装maven的根目录

####表现####
- File=>New =>maven project=>继续下去利用.maven-archetype-quickstart即可

###简单的概念解释###
- pom:project object model
- Coordinate:maven给我们定义了4个坐标groudId,artifactId,version,packaging (参考资料)[http://www.yiibai.com/maven/maven-dependency-to-download-library.html],packaging可以缺省