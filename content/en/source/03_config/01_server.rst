Server Initial and Upgrade
===============================

Initial Configuration Custom Database
--------------------------------------------
.. code-block:: bash
    
   # download sql files
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
    # Execute command,input username and password,import sql files
    cat *.sql | mysql -u<username> -h<url> -p --default-character-set=utf8mb4 dongtai_webapi


Upgrade DongTai IAST Server 
------------------------------------------
Step 1: Update installation package to latest version
++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. code-block:: bash

    # Change to DongTai directory, e.g.：/opt/DongTai
    git stash
    git pull
    git checkout release-<latest version>

Step 2: Run script to start deployment process
++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. code-block:: bash

    # Stop all DongTai IAST services in docker
    docker stop dongtai-iast_dongtai-engine-task_1 dongtai-iast_dongtai-web_1 dongtai-iast_dongtai-engine_1 dongtai-iast_dongtai-webapi_1 dongtai-iast_dongtai-openapi_1 dongtai-iast_dongtai-redis_1 dongtai-iast_dongtai-mysql_1

    cd deploy/docker-compose
    chmod u+x install.sh
    ./install.sh

Step 3: Update Database
++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. attention::

    If you are not using the previous version, please :red:`UPDATE` you database SQL update file to the previous version before execute the following step.

- Entering ``dongtai-mysql`` container:

  .. code-block:: bash

      docker exec -it dongtai-iast_dongtai-mysql_1 /bin/bash

- Navigating to :blue:`/docker-entrypoint-initdb.d`, and then download the SQL update file: :blue:`update-20211105.sql`

  .. code-block:: bash

      cd /docker-entrypoint-initdb.d
      mysql -uroot -p"dongtai-iast" -D dongtai_webapi < /docker-entrypoint-initdb.d/update-20211022.sql

