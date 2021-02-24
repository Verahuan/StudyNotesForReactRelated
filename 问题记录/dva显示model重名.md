# app.model: namespace should be unique 

问题：显示model重名，但是实际上我只在crud文件夹下面使用了一个model.ts文件，namespace为“crud”

## 解决方法

src/.umi/plugin-dva/dva.ts

 文件中多次导入和使用了model

model0和model1 删除重复的内容即可

可能是缓存问题，不知道为什么会出现这个情况

## 参考

https://github.com/dvajs/dva/issues/746