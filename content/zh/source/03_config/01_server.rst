洞态 IAST Server 初始化 & 升级
===============================

初始化自定义数据库
--------------------------------------------
.. code-block:: bash
    
    # 下载数据库文件
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/db.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/rule.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/sca.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/update-20210731-release-1.0.0.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/update-20210817-release-1.0.2.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/update-20210831-release-1.0.3.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/update-20210918-release-1.0.4.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/update-20211009-release-1.0.5.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/update-20211022-release-1.0.6.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/update-20211105-release-1.1.0.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/update-20211120-release-1.1.1.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/update-20211123-release-1.1.2.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/update-20211203-release-1.1.3.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/update-20211218-release-1.1.4.sql
    wget https://huoqi-public.oss-cn-beijing.aliyuncs.com/iast/sql/update-20211230-release-1.2.0.sql

    # 执行导入命令，输入数据库密码，完成刚刚下载的数据导入
    cat *.sql | mysql -uroot -p --default-character-set=utf8mb4 dongtai_webapi
    



升级洞态 IAST Server 端
------------------------------------------
步骤 1: 更新安装包至最新版本
++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. code-block:: bash

    # 切换代码所在的目录，如：/opt/DongTai
    git stash
    git pull
    git checkout release-<latest version>

步骤 2: 运行自动部署脚本
++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. code-block:: bash

    # 停止当前 docker 中所有的的洞态服务
    docker stop dongtai-iast_dongtai-engine-task_1 dongtai-iast_dongtai-web_1 dongtai-iast_dongtai-engine_1 dongtai-iast_dongtai-webapi_1 dongtai-iast_dongtai-openapi_1 dongtai-iast_dongtai-redis_1 dongtai-iast_dongtai-mysql_1

    cd deploy/docker-compose
    chmod u+x install.sh
    ./install.sh

步骤 3: 更新数据库文件
++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. attention::

    若您不是使用上一版本，:red:`务必` 将数据库文件升级至上一版再执行下列步骤。

- 进入 ``dongtai-mysql`` 的容器:

  .. code-block:: bash

      docker exec -it dongtai-iast_dongtai-mysql_1 /bin/bash

- 进入 :blue:`/docker-entrypoint-initdb.d` 目录, 下载数据库更新文件: :blue:`update-20211105.sql`

  .. code-block:: bash

      cd /docker-entrypoint-initdb.d
      mysql -uroot -p"dongtai-iast" -D dongtai_webapi < /docker-entrypoint-initdb.d/update-20211022.sql

