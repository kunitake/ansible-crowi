使い方
=====================
手元で試すための、Vagrantfile を含んでいます(事前に CentOS7 の box を導入している必要があります)

```
$ vagrant up
$ vagrant ssh-config > ssh.config
$ ansible-playbook -i inventories/stage master.yml
```

で構築可能。

