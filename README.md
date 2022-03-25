# SingleInstance
.net core 5及以上可用的单个wpf应用实例


原官方给出的单个实例是基于framework的  
[实现参考](https://codereview.stackexchange.com/questions/20871/single-instance-wpf-application)  

.net core没有对应的IPC实现,所以进程通讯这个改成了命名管道实现  

且启动参数哪里只传递了一个

[命名管道](https://docs.microsoft.com/zh-cn/dotnet/standard/io/how-to-use-named-pipes-for-network-interprocess-communication)  

# step
1. 所有步骤均和原有官方步骤一致
  
2. 有个需要注意的地方是App修改为page时会提示错误并被项目排除,需要再把文件包括在项目中

3. 另外管道,需要NuGet添加包`System.IO.Pipes`  
  
4. SignalExternalCommandLineArgs的回调非UI线程,需要注意
~~~
public bool SignalExternalCommandLineArgs(string arg)
{
  this.Dispatcher.BeginInvoke(new Action(() =>
  {
    
  }));
  // Handle command line arguments of second instance
  return true;
  }
~~~
