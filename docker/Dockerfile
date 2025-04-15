# Have tested with a custom Ubuntu-1804 / Python 3.7 / Tensorflow 2.5.0 Base Image
# Not tested with this image. 
FROM tensorflow/tensorflow:2.5.0
RUN apt update && \
    apt-get install git -y

# 设置工作目录为/app。
WORKDIR /app/

# 将当前目录下的所有文件复制到容器的/app目录。
COPY . /app/    

# 克隆mesh-transformer-jax仓库
# 安装该仓库的Python依赖
# 安装特定版本(0.2.12)的jax库
# 安装FastAPI等Web服务相关库
# 创建start.sh的软链接到根目录

# ln -s /app/start.sh /start.sh 的作用是创建一个符号链接（软链接），具体解释如下：

# ln 是 Linux 的链接命令
#     -s 参数表示创建符号链接（软链接），而不是硬链接
#     /app/start.sh 是源文件路径（容器内/app目录下的start.sh脚本）
#     /start.sh 是链接文件路径（容器根目录下的start.sh）
# 实际效果：
#     在容器的根目录 / 下创建一个名为 start.sh 的快捷方式
#     这个快捷方式指向 /app/start.sh 这个实际文件
#     当执行 /start.sh 时，实际上执行的是 /app/start.sh

# 为什么需要这样做：
#     方便性：可以直接在根目录访问启动脚本，而不需要输入完整路径
#     兼容性：某些系统或工具可能期望启动脚本位于根目录
#     标准化：保持与其他容器或部署方式的路径一致性

# 与硬链接的区别：
#     符号链接类似于Windows的快捷方式，删除源文件后链接会失效
#     硬链接则是直接指向文件数据块，删除源文件不影响链接


RUN git clone https://github.com/kingoflolz/mesh-transformer-jax && \
    pip install -r mesh-transformer-jax/requirements.txt && \
    pip install mesh-transformer-jax/ jax==0.2.12 && \
    pip install fastapi uvicorn requests aiofiles aiohttp && \
    ln -s /app/start.sh /start.sh

# 用冒号(:)分隔的三个路径

# 1. 当 Python 程序运行时，解释器会依次在这些路径中查找导入的模块
# 2. 优先级顺序：从左到右（先查/app，再查mesh-transformer-jax，最后查Python标准库路径）
# 3. 使得容器内的 Python 程序可以：
#     - 直接访问/app下的自定义模块
#     - 使用mesh-transformer-jax项目中的代码
#     - 同时保留对Python标准库的访问
ENV PYTHONPATH /app:/app/mesh-transformer-jax:/usr/local/bin/python3
ENV PATH $PYTHONPATH:$PATH
ENV TOKENIZERS_PARALLELISM=true
EXPOSE 80

CMD ["/start.sh"]