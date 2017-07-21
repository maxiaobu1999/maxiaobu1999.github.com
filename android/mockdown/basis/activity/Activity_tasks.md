##android 任务栈&启动模式
###任务栈
![android 任务栈](http://img-1253423006.costj.myqcloud.com/activity_tasks.png)
###启动模式
* standard      每次创建一个实例
* singleTop     栈顶复用模式 顶上有复用，调用onNewIntent()
* singleTask    栈内复用模式，栈内有挪到顶部，上面其他aty实例移除，调用onNewIntent()
* singleInstance 单独占个栈放他 其他同上
 