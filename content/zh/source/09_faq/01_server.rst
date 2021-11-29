Server
==========
部署相关
---------

- **服务端是否支持docker部署？**

  支持，:doc:`请参考部署指南 <../02_start/02_deploy>`

- **使用本地数据库时，如何初始化数据库？**

  :doc:`请参考部署指南 <../03_config/01_server>`

- **部署时拉取镜像或者代码失败的原因？**

  - 拉取镜像：检查网络是否能访问 Docker Hub

  - 拉取代码：检查网络是否能访问 GitHub

- **本地安装对服务器资源有什么要求吗？**

  建议至少 4 Core 8 GB, :doc:`请参考部署指南 <../02_start/02_deploy>` 中各部署方式之系统需求


- **如何部署多个openAPI服务节点？(基于docker-compose方式)**
  
  使用以下命令将openapi扩容到number个
  
  sudo docker-compose -p dongtai-iast up -d --scale dongtai-openapi=<number> --no-recreate

- **Centos7 部署报chown mod /var/lib/mysql permission denied，如何解决?**

  - ``getenforce`` 查看 ``selinux`` 是否开启

  - 若开启使用 ``setenforce 0`` 关闭 ``getenforce``

  - 仍不行就给需要映射文件的容器加上 ``--``






