# ops

## Ansible
Пререквизиты: `ssh bigone.demist.ru` должен отрабатывать без требований пользовательского ввода.
Для этого заполните себе файлик `.ssh/config` (на своем ноуте).
Для примера, в моем ~/.ssh/config на маке:
```
Host *
  UseKeychain yes
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_rsa

Host bigone bigone.demist.ru
	User sverdlov
	Hostname bigone.demist.ru
```

Пример запуска плейбука для первоначального бутстрапа машины:
```bash
$ cd ./ansible
$ ansible-playbook -i hosts bootstrap.yml
```
