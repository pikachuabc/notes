---
description: 由于实习需要最近看了看前端的一些概念，想起以前在看IOS开发中提到的关于MVVM的概念，这里做个比较总结
---

# MVC vs. MVVM（未完成）

## MVC

![\*https://www.linkedin.com/pulse/understanding-difference-between-mvc-mvp-mvvm-design-rishabh-software](../.gitbook/assets/image%20%281%29.png)

基本上算目前比较主流应用，随着开发需求的提高，前后端变得愈加复以及独立，因此将前端（View）和后端的Model放在同一个文件中变得不太现实，取而代之的是处于中间的controller通过get/post和routing映射到后端对应的逻辑（比如RequestMapping）访问model\(JSON\)，每个controller同时负责接收和发送数据，这其实就是MVC和核心思想-分离View和Model，提高开发效率，其中根据Model是否会主动通知View数据变化分为主动和被动MVC。

### However

当一个应用的规模变大时，控制器的数量也必须随证增长，

![\*Standford CS193P - Developing Apps for iOS](../.gitbook/assets/image.png)

## MVC VS MVVM

![\*https://juejin.im/post/5ceb4a2ef265da1b6f435291](../.gitbook/assets/image%20%282%29.png)



