## 1. 在 arch 环境下无法找到fontconfig与gtk包

- fontconfig 与 gtk 没有 `.cmake` 文件，所以修改`{cocos-2d}/cmake/Modules/CocosConfigDepend.cmake` 文件
- 将 `find_package` 与 `cocos_uss_pkg` 注释掉改为直接链接库
![](attachments/Pasted%20image%2020221109132516.png)
![](attachments/Pasted%20image%2020221109132530.png)


## 2. 没有 gtk头文件

- 是因为头文件结构变了, `gtk.h` 位于 `gtk-3.0/gtk/gtk.h`
- 修改包含路径
![](attachments/Pasted%20image%2020221109133831.png)

