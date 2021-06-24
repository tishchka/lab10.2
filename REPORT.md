## Laboratory work X

Данная лабораторная работа посвещена изучению процесса создания и конфигурирования виртуальной среды разработки с использованием **Vagrant**

```sh
$ open https://www.vagrantup.com/intro/index.html
```

## Tasks

- [x] 1. Ознакомиться со ссылками учебного материала
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
# Задаем имя пользователя Github
$ export GITHUB_USERNAME=DimaZzZz101

# Устанавливаем в качестве текущего макетного менеджера brew
$ export PACKAGE_MANAGER=brew
```

```sh
# Переходим в рабочую директорию
$ cd ${GITHUB_USERNAME}/workspace

# Устанавливаем vagrant
$ ${PACKAGE_MANAGER} install vagrant

vagrant was successfully installed!

```

```sh
# Проверка версии vagrant
$ vagrant version

Installed Version: 2.2.16
Latest Version: 2.2.16
You are running an up-to-date version of Vagrant!

# Создаем новую виртуальную машину с ОС Ubuntu-19.10
$ vagrant init bento/ubuntu-19.10

A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.

# Показ содержимого файла Vagrantfile
$ less Vagrantfile
---
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "bento/ubuntu-19.10"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"
  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end

---
# Создаем новый Vagrantfile, перезаписыва изначальный (флаг -f), находящийся в текущем пути. Причем информация в нем будет в минимальном объеме, т.е. без комментариев (флаг -m)
$ vagrant init -f -m bento/ubuntu-19.10

A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.

# Показ содержимого файла Vagrantfile
$ less Vagrantfile
---

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-19.10"
end

---
```

```sh
# Создаем директорию shared
$ mkdir shared
```

```sh
# В файл Vagrantfile записываем комманды для запуска скрипта 
$ cat > Vagrantfile <<EOF
\$script = <<-SCRIPT
sudo apt install docker.io -y
sudo docker pull fastide/ubuntu:19.04
sudo docker create -ti --name fastide fastide/ubuntu:19.04 bash
sudo docker cp fastide:/home/developer /home/
sudo useradd developer
sudo usermod -aG sudo developer
echo "developer:developer" | sudo chpasswd
sudo chown -R developer /home/developer
SCRIPT
EOF
```

```sh
# В файл Vagrantfile записываем конфигурацию для виртуальной машины
# Взято с сайта Vagrant.com:
# Версии конфигурации - это механизм, с помощью которого Vagrant 1.1+ может оставаться обратно совместимым с Vagrantfiles Vagrant 1.0.x, при этом вводя существенно новые функции и параметры конфигурации.
# «2» в первой строке выше представляет версию конфигурации объекта конфигурации, которая будет использоваться для конфигурации для этого блока (раздел между do и end). Этот объект может сильно отличаться от версии к версии.
# В настоящее время поддерживаются только две версии: «1» и «2». Версия 1 представляет собой конфигурацию из Vagrant 1.0.x. «2» представляет конфигурацию от 1.1+ до 2.0.x.
# При загрузке Vagrantfiles Vagrant использует соответствующий объект конфигурации для каждой версии и правильно объединяет их, как и любую другую конфигурацию.

# vagrant-vbguest - это плагин, который автоматически обновляет гостевые дополнения VirtualBox
$ cat >> Vagrantfile <<EOF

Vagrant.configure("2") do |config|

  config.vagrant.plugins = ["vagrant-vbguest"]
EOF
```

```sh
# Продолжаем конфигурацию для виртуальной машины
# Указываем версию виртуалки: ubuntu-19.10
# Указываем настройки сети: public_network
# Указываем связующие директории: 'shared', '/vagrant', type: 'rsync'
# Указываем тип виртуальной машины, т.е. ее название: virtualbox
# Указываем, что используется графический интерфейс: vb.gui = true
# Указываем, что выделяется под виртуальную машину 2ГБ оперативной памяти
# config.vm.provision "shell" - задает встроенную команду оболочки для выполнения на удаленном компьютере
# config.ssh.extra_args - значение настроек передается непосредственно в исполняемый файл ssh

$ cat >> Vagrantfile <<EOF

  config.vm.box = "bento/ubuntu-19.10"
  config.vm.network "public_network"
  config.vm.synced_folder('shared', '/vagrant', type: 'rsync')

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "2048"
  end

  config.vm.provision "shell", inline: \$script, privileged: true

  config.ssh.extra_args = "-tt"

end
EOF
```

```sh
# Выполяем проверку Vagrantfile
$ vagrant validate

Vagrantfile validated successfully.

# Проверяем состояние машин, которыми управляет Vagrant
$ vagrant status

Current machine states:

default                   not created (virtualbox)

The environment has not yet been created. Run `vagrant up` to
create the environment. If a machine is not created, only the
default provider will be shown. So if a provider is not listed,
then the machine is not created for that environment.

# Создаем и настраиваем гостевые машины в соответствии с вашим Vagrant
# [name|id] - если машин несколько
$ vagrant up # --provider virtualbox

default: Machine booted and ready!

# Выводим полный список гостевых портов, сопоставленных с портами хост-компьютера
$ vagrant port

22 (guest) => 2222 (host)

# Проверяем состояние машин, которыми управляет Vagrant
$ vagrant status

Current machine states:

default                   running (virtualbox)

# Подключаем SSH к работающей машине Vagrant. Получаем доступ к оболочке. В простом Vagrant проекте созданный экземпляр виртуальной машины будет называться default.
$ vagrant ssh

# login: vagrant
# password: vagrant

Welcome to Ubuntu 19.10 (GNU/Linux 5.3.0-64-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu 06 May 2021 01:35:44 AM UTC

  System load:  0.05              Processes:           106
  Usage of /:   2.6% of 61.80GB   Users logged in:     1
  Memory usage: 6%                IP address for eth0: 10.0.2.15
  Swap usage:   0%

 * Pure upstream Kubernetes 1.21, smallest, simplest cluster ops!

     https://microk8s.io/

0 updates can be installed immediately.
0 of these updates are security updates.


The list of available updates is more than a week old.
To check for new updates run: sudo apt update


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Thu May  6 01:33:25 2021

# Выводим все сделанные снимки (образы)
$ vagrant snapshot list

default: No snapshots have been taken yet!

# Добавляет снимок (образ) в стек снимков (образов)
$ vagrant snapshot push

==> default: Snapshotting the machine as 'push_1620265067_5072'...
==> default: Snapshot saved! You can restore the snapshot at any time by
==> default: using `vagrant snapshot restore`. You can delete it using
==> default: `vagrant snapshot delete`.

# Выводим все сделанные снимки (образы)
$ vagrant snapshot list

==> default: 
push_1620265067_5072

# Выключаем запущенную виртуальную машину
# [name|id] - если машин несколько
$ vagrant halt

==> default: Attempting graceful shutdown of VM...

# Убираем снимок (образ) из стека снимков (образов)
$ vagrant snapshot pop

==> default: Restoring the snapshot 'push_1620265067_5072'...
==> default: Deleting the snapshot 'push_1620265067_5072'...
==> default: Snapshot deleted!
```

```ruby
# Взято: https://github.com/josenk/vagrant-vmware-esxi

# Это плагин Vagrant, который добавляет поддержку провайдера VMware ESXi. Это позволяет Vagrant контролировать и настраивать виртуальные машины непосредственно на гипервизоре ESXi без необходимости использования vCenter или VShpere. Гипервизор ESXi можно бесплатно загрузить с сайта VMware.

# Выполняем конфигурацию 
  config.vm.provider :vmware_esxi do |esxi|

    esxi.esxi_hostname = '<exsi_hostname>'
    esxi.esxi_username = 'root'
    esxi.esxi_password = 'vagrant'

    esxi.esxi_hostport = 22

    esxi.guest_name = '${GITHUB_USERNAME}'

    esxi.guest_username = 'vagrant'
    esxi.guest_memsize = '2048'
    esxi.guest_numvcpus = '2'
    esxi.guest_disk_type = 'thin'
  end
```

```sh
# Устанавливаем плагин vmware-esxi
$ vagrant plugin install vagrant-vmware-esxi

Installed the plugin 'vagrant-vmware-esxi (2.5.2)'!

# Выведем в консоль все установленные плагины
$ vagrant plugin list

vagrant-vbguest (0.29.0, global)
vagrant-vmware-esxi (2.5.2, global)

# Создаем гостевую машину в соответствии с вашим Vagrant, в данном случае не VirtulBox, а VMWare
$ vagrant up --provider=vmware_esxi

# В консоль выводит следующее, т.к. у меня нет установленного VMWare, бесплатной версии нет: -bash: vmware: command not found
root@<exsi_hostname> password:
==> default: --- ESXi host access : Password incorrect.
There was an error talking to ESXi.
  Unable to connect to ESXi host! Error: getaddrinfo: nodename nor servname provided, or not known
```

## Report

```sh
# Создание отчета 
$ cd ~/workspace/
$ export LAB_NUMBER=10
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER}.git tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Links

- [VirualBox](https://www.virtualbox.org/)
- [Vagrant providers](https://github.com/hashicorp/vagrant/wiki/Available-Vagrant-Plugins#providers)
- [Vagrant vbguest plugin](https://github.com/dotless-de/vagrant-vbguest)
- [Vagrant disksize plugin](https://github.com/sprotheroe/vagrant-disksize)
- [Vagrant vmware esxi plugin](https://github.com/josenk/vagrant-vmware-esxi)

```
Copyright (c) 2015-2021 The ISC Authors
```
