# golang web 项目怎么加载配置文件

https://medium.com/@boobo94/web-service-architecture-for-golang-developers-a4147b8141ff

* 将config对象作为变量从main.go传递给最终函数，我需要在其中使用它。这肯定是一个好主意，因为我只是为那些需要它的实例传递该变量，所以这样我就不会影响速度质量。但这对于开发或重构来说非常耗时，因为我需要一直将配置从一个功能传递到另一个功能，所以最后，你想要自杀，meeh ..，也许不是，但我仍然不喜欢不喜欢它。

* 声明一个全局变量并在我需要的任何地方使用该实例。但是在我看来这根本不是最好的选择，因为我必须声明一个变量，例如在main.go文件中，稍后在我需要Unmarshal()的JSON文件的主函数中，将该内容放入变量对象中宣布为全球性的。但是猜猜看，也许我正在尝试在初始化准备好之前调用该对象，因此我将有一个空对象，没有实际值，所以在这种情况下我的应用程序将崩溃。

* 在我需要的地方直接注入配置对象，是的，这是我最好的选择，完全适合我。在config.go文件中，在它的末尾，我声明以下行：

```
var Main = (func() Configuration {
var conf Configuration
 if err := configor.Load(&conf, "PATH_TO_CONFIG_FILE"); err != nil {
  panic(err.Error())
 }
 return conf
})()
```

你需要知道的是这个实现是我使用一个名为Configor的库，它解组一个文件，在我们的例子中是一个JSON，然后将它加载到一个变量中conf，该变量被返回。

任何时候当你需要使用配置中的东西时，只需键入包名称，即config调用变量Main，就像下面的示例一样，它检索数据库的配置：

```
var myDBConf = config.Main.Database
```

!!!提示：正如您所看到的，您必须在那里插入配置文件的路径，但由于您希望为不同的环境设置不同的文件，因此您可以设置一个名为的环境变量CONFIG_PATH。将其定义为env变量，或者在运行之前将其设置为：

```
$ CONFIG_PATH=home/username/.../config.local.json go run cmd/main.go
```
