.. include:: ansible-variables.txt

.. _ansible-roles-common:

#############
common role
#############
.. include:: common/summary.rst


タスク一覧
#################
- :ref:`tasks_common_1`
- :ref:`tasks_common_2`
- :ref:`tasks_common_3`
- :ref:`tasks_common_4`


.. index::
    single: yum; install epel release

.. _tasks_common_1:

----------------------
install epel release
----------------------
.. admonition:: install epel release

         name={{item}} state=installed

    :module: yum
    :tags: common-packages
    :with_items:
     - epel-release


.. include:: common/tasks_desc_1.rst


.. index::
    single: yum; install common packages

.. _tasks_common_2:

-------------------------
install common packages
-------------------------
.. admonition:: install common packages

         name={{item}} enablerepo=epel state=installed

    :module: yum
    :tags: common-packages
    :with_items:
     - git
     - yum-utils
     - telnet
     - wget


.. include:: common/tasks_desc_2.rst


.. index::
    single: hostname; set-hostname

.. _tasks_common_3:

--------------
set-hostname
--------------
.. admonition:: set-hostname

         name="{{ inventory_hostname }}"

    :module: hostname


.. include:: common/tasks_desc_3.rst


.. index::
    single: selinux; Set SELinux as permissive mode.

.. _tasks_common_4:

---------------------------------
Set SELinux as permissive mode.
---------------------------------
.. admonition:: Set SELinux as permissive mode.

         policy=targeted state=permissive

    :module: selinux


.. include:: common/tasks_desc_4.rst


