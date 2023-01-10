# Docker常用command
1. no cache build
```
docker build --no-cache
```

2. 单独运行一个image调试   
```
docker run -ti <image>
```
-t 启动[Pseudoterminal](https://en.wikipedia.org/wiki/Pseudoterminal)  
-i 启动交互 

3.  