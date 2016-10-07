.. include:: ansible-variables.txt

.. _ansible-roles-crowi:

############
crowi role
############
.. include:: crowi/summary.rst


タスク一覧
#################
- :ref:`tasks_crowi_1`
- :ref:`tasks_crowi_2`
- :ref:`tasks_crowi_3`
- :ref:`tasks_crowi_4`
- :ref:`tasks_crowi_5`
- :ref:`tasks_crowi_6`
- :ref:`tasks_crowi_7`
- :ref:`tasks_crowi_8`
- :ref:`tasks_crowi_9`
- :ref:`tasks_crowi_10`
- :ref:`tasks_crowi_11`
- :ref:`tasks_crowi_12`
- :ref:`tasks_crowi_13`
- :ref:`tasks_crowi_14`


.. index::
    single: yum; install packages

.. _tasks_crowi_1:

------------------
install packages
------------------
.. admonition:: install packages

         name={{ item }} enablerepo=epel state=installed

    :module: yum
    :with_items:
     - curl
     - mongodb-server
     - python-virtualenv
     - python-pip
     - gcc
     - gcc-c++
     - python-devel
     - mongodb


.. include:: crowi/tasks_desc_1.rst


.. index::
    single: service; mongod は先に動かしておく(自動起動もon)

.. _tasks_crowi_2:

-------------------------------------------------------
mongod は先に動かしておく(自動起動もon)
-------------------------------------------------------
.. admonition:: mongod は先に動かしておく(自動起動もon)

         name=mongod state=started enabled=yes

    :module: service


.. include:: crowi/tasks_desc_2.rst


.. index::
    single: pip; ansible動作用pymongoは pip で入れる(packageは古いので）

.. _tasks_crowi_3:

-----------------------------------------------------------------------
ansible動作用pymongoは pip で入れる(packageは古いので）
-----------------------------------------------------------------------
.. admonition:: ansible動作用pymongoは pip で入れる(packageは古いので）

         name=pymongo

    :module: pip


.. include:: crowi/tasks_desc_3.rst


.. index::
    single: mongodb_user; mongod に adminユーザにパスワードを設定

.. _tasks_crowi_4:

------------------------------------------------------
mongod に adminユーザにパスワードを設定
------------------------------------------------------
.. admonition:: mongod に adminユーザにパスワードを設定

     state=present password={{ crowi_password }} name=admin database=admin
    :module: mongodb_user


.. include:: crowi/tasks_desc_4.rst


.. index::
    single: mongodb_user; mongod に crowi 用ユーザを作成

.. _tasks_crowi_5:

----------------------------------------
mongod に crowi 用ユーザを作成
----------------------------------------
.. admonition:: mongod に crowi 用ユーザを作成

     state=present password={{ crowi_password }} name={{ crowi_user }} roles={'db': '{{crowi_database}}', 'role': 'readWrite'} database={{ crowi_database }} login_user=admin login_password={{ crowi_password }}
    :module: mongodb_user


.. include:: crowi/tasks_desc_5.rst


.. index::
    single: yum; install nodejs

.. _tasks_crowi_6:

----------------
install nodejs
----------------
.. admonition:: install nodejs

         name=https://rpm.nodesource.com/pub_4.x/el/7/x86_64/nodejs-{{ nodejs_version }}nodesource.el7.centos.x86_64.rpm state=present

    :module: yum


.. include:: crowi/tasks_desc_6.rst


.. index::
    single: file; src dir permission

.. _tasks_crowi_7:

--------------------
src dir permission
--------------------
.. admonition:: src dir permission

         path=/usr/local/src/crowi mode=755 state=directory owner={{ ansible_user_id }}

    :module: file


.. include:: crowi/tasks_desc_7.rst


.. index::
    single: git; git clone

.. _tasks_crowi_8:

-----------
git clone
-----------
.. admonition:: git clone

         repo=https://github.com/crowi/crowi dest=/usr/local/src/crowi update=no

    :module: git


.. include:: crowi/tasks_desc_8.rst


.. index::
    single: stat; npm install が実行済かを確認

.. _tasks_crowi_9:

--------------------------------------
npm install が実行済かを確認
--------------------------------------
.. admonition:: npm install が実行済かを確認

         path=/usr/local/src/crowi/node_modules

    :module: stat
    :register: node_modules


.. include:: crowi/tasks_desc_9.rst


.. index::
    single: shell; npm install を実行

.. _tasks_crowi_10:

-----------------------
npm install を実行
-----------------------
.. admonition:: npm install を実行

         npm install

    :module: shell
    :when: not node_modules.stat.exists


.. include:: crowi/tasks_desc_10.rst


.. index::
    single: copy; systemd service file をアップロード

.. _tasks_crowi_11:

--------------------------------------------
systemd service file をアップロード
--------------------------------------------
.. admonition:: systemd service file をアップロード

         src=crowi.service dest=/etc/systemd/system mode=0644

    :module: copy


.. include:: crowi/tasks_desc_11.rst


.. index::
    single: template; put env file

.. _tasks_crowi_12:

--------------
put env file
--------------
.. admonition:: put env file

         src=crowi.sysconfig.j2 dest=/etc/sysconfig/crowi owner=root mode=0600

    :module: template
    :notify: :ref:`system_daemon_reload`


.. include:: crowi/tasks_desc_12.rst


.. index::
    single: service; crowiを起動

.. _tasks_crowi_13:

----------------
crowiを起動
----------------
.. admonition:: crowiを起動

         name=crowi state=started enabled=yes

    :module: service


.. include:: crowi/tasks_desc_13.rst


.. index::
    single: firewalld; crowi用の 3000番ポートを open

.. _tasks_crowi_14:

--------------------------------------
crowi用の 3000番ポートを open
--------------------------------------
.. admonition:: crowi用の 3000番ポートを open

         zone=public port=3000/tcp state=enabled  permanent=true immediate=yes

    :module: firewalld


.. include:: crowi/tasks_desc_14.rst


Notify一覧
##############
- :ref:`handlers_crowi_0`
.. index::
    single: shell; system daemon reload

.. _handlers_crowi_0:

----------------------
system daemon reload
----------------------
.. admonition:: system daemon reload

         systemctl --system daemon-reload

    :module: shell


.. include:: crowi/handlers_desc_0.rst


