## php composer dockerfile

php composer镜像，包含swoole扩展

使用方法

* 构建镜像
```bash
cd composer
docker build -t composer .
```

* 使用

/your_path换成你的工作目录
```bash
docker run --rm --net=host -v /your_path:/data:rw -it composer bash
```