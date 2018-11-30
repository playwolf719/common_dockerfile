# 常用dockerfile
[相关github链接](https://github.com/playwolf719/common_dockerfile)
## 基于Ubuntu的nginx+uwsgi纯安装镜像（不用翻墙）
- 纯安装镜像，不包含启动操作，启动可通过supervisor操作。
- 之前发现github上的类似nginx+uwsgi的镜像，国内下载东西基本都会超时或翻墙，甚是麻烦，所以参考写了这个dockerfile。
## pyapi镜像（依赖上面的nginx+uwsgi纯安装镜像）
- 这部分可以理解为基于上面镜像的api应用镜像。结合两者，可以快速搭建定制的python api环境。
