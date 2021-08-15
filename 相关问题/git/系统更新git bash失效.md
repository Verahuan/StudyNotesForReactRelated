## 问题：升级windows系统导致git bash 右键不出现

**解决思路：**需要重新配置快捷键
**具体过程：**点击windows图标–输入regedit–打开注册表编辑器–找到： 计算机\HKEY_CLASSES_ROOT\Directory\Background\shell 路径–在shell上右键新建项----open in git–双击默认修改其值（Git bash here）–下一行创建图标（字符串）–双击匹配到图标路径

在open in git上新建项command–双击默认–管理bash.exe位置

如图所示：

![image-20210407095027612](D:\typora\images\image-20210407095027612.png)

![image-20210407095040001](D:\typora\images\image-20210407095040001.png)