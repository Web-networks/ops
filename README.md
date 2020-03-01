# ops

## Ansible
Пререквизиты: `ssh bigone.demist.ru` должен отрабатывать без требований пользовательского ввода.
Для этого заполните себе файлик `.ssh/config` (на своем ноуте).
Для примера, в моем `~/.ssh/config` на маке:
```ssh-config
Host *
    UseKeychain yes
    AddKeysToAgent yes
    IdentityFile ~/.ssh/id_rsa

Host bigone bigone.demist.ru
    User sverdlov
    Hostname bigone.demist.ru
```

### Kubespray
Для того чтобы работал kubespray нужно положить в `~/.ssh/config`:
```ssh-config
Host neuroide-kube-1
	Hostname 127.0.0.1
    User vagrant
    Port 2202
    ForwardAgent yes
    ProxyCommand ssh -o ForwardAgent=yes -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no bigone.demist.ru 'ssh-add /home/sverdlov/.vagrant.d/insecure_private_key && nc %h %p'

Host neuroide-kube-2
	Hostname 127.0.0.1
    User vagrant
    Port 2203
    ProxyCommand ssh -o ForwardAgent=yes -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -W %h:%p bigone.demist.ru -i ~/.vagrant.d/insecure_private_key

Host neuroide-kube-3
	Hostname 127.0.0.1
    User vagrant
    Port 2204
    ProxyCommand ssh -o ForwardAgent=yes -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -W %h:%p bigone.demist.ru -i ~/.vagrant.d/insecure_private_key
```

# Бутстрап машины
```bash
cd ./ansible
ansible-playbook -i hosts bootstrap.yml
```

# Создание vagrant машин
```bash
cd ./ansible
ansible-playbook -i hosts vagrant.yml
```

**WARNING**: т.к. ansible не умеет в интерактивный stdout, а команда `vagrant up` занимает продолжительное время, после запуска плейбука необходимо произвести ручные действия. На всякий случай опишу их здесь (они так также выводятся в последней таске плейбука):
```bash
ssh bigone.demist.ru
cd base
vagrant up # долгая команда
vagrant ssh-config > ~/.ssh/config
```
Потом посмотреть в получившийся ssh конфиг, и поправить локальный. В локальном нужно будет восстановить соответствие портов и имен слейвов.
