### maven安装
下载好相应的压缩包，解压到自己想放的目录。然后将该目录设置到环境变量中去
使用`mvn -v`查看环境变量是不是设置好安装好了

### quick-start
`mvn archetype:generate -DgroupId=com.jiabao.app -DartifactId=my-app -DarchetyprArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false`
`mvn archetype:generate "-DgroupId=com.companyname.bank" "-DartifactId=consumerBanking" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`
这一行命令就是用来创建项目的命令

### 编译打包命令
`mvn package`

### 运行打包好的jar
`java -cp tartget/my-app-1.0-SNAPSHOT.jar com.jiabao.app.App`