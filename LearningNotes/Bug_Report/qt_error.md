# Qt 报错的解决方案

- OpenCV 的报错

  error while loading shared libraries: libopencv_core.so.3.2: cannot open shared object file: No such file or directory https://blog.csdn.net/lql0716/article/details/54434695

  

- Qt 找不到 `QApplication` 

  https://blog.csdn.net/friendbkf/article/details/45440175

  在 `.pro` 中添加

  ```cmake
  greaterThan(QT_MAJOR_VERSION, 4): QT += widgets
  ```