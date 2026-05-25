
# Домашнее задание к занятию 2. «Применение принципов IaaC в работе с виртуальными машинами»

#### Это задание для самостоятельной отработки навыков и не предполагает обратной связи от преподавателя. Его выполнение не влияет на завершение модуля. Но мы рекомендуем его выполнить, чтобы закрепить полученные знания. Все вопросы, возникающие в процессе выполнения заданий, пишите в раздел "Вопросы по заданиям" в личном кабинете.
---
## Важно

**Перед началом работы над заданием изучите [Инструкцию по экономии облачных ресурсов](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD).**
Перед отправкой работы на проверку удаляйте неиспользуемые ресурсы.
Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
Подробные рекомендации [здесь](https://github.com/netology-code/virt-homeworks/blob/virt-11/r/README.md).

---

### Цели задания

1. Научиться создвать виртуальные машины в Virtualbox с помощью Vagrant.
2. Научиться базовому использованию packer в yandex cloud.

   
## Задача 1
Установите на личный Linux-компьютер или учебную **локальную** ВМ с Linux следующие сервисы(желательно ОС ubuntu 20.04):

- [VirtualBox](https://www.virtualbox.org/),
- [Vagrant](https://github.com/netology-code/devops-materials), рекомендуем версию 2.3.4
- [Packer](https://github.com/netology-code/devops-materials/blob/master/README.md) версии 1.9.х + плагин от Яндекс Облако по [инструкции](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/packer-quickstart)
- [уandex cloud cli](https://cloud.yandex.com/ru/docs/cli/quickstart) Так же инициализируйте профиль с помощью ```yc init``` .


Примечание: Облачная ВМ с Linux в данной задаче не подойдёт из-за ограничений облачного провайдера. У вас просто не установится virtualbox.

## Решение
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/01_virtualbox.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/02_vagrant.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/03_packer.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/04_yc.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/05_yc_plugin.png)

## Задача 2

1. Убедитесь, что у вас есть ssh ключ в ОС или создайте его с помощью команды ```ssh-keygen -t ed25519```
2. Создайте виртуальную машину Virtualbox с помощью Vagrant и  [Vagrantfile](https://github.com/netology-code/virtd-homeworks/blob/shvirtd-1/05-virt-02-iaac/src/Vagrantfile) в директории src.
3. Зайдите внутрь ВМ и убедитесь, что Docker установлен с помощью команды:
```
docker version && docker compose version
```

3. Если Vagrant выдаёт ошибку (блокировка трафика):
```
URL: ["https://vagrantcloud.com/bento/ubuntu-20.04"]     
Error: The requested URL returned error: 404:
```

Выполните следующие действия:

- Используйте [зеркало](https://vagrant.elab.pro/downloads/) файл-образ "bento/ubuntu-24.04".

**Важно:**    
- Если ваша хостовая рабочая станция - это windows ОС, то у вас могут возникнуть проблемы со вложенной виртуализацией. Ознакомиться со cпособами решения можно [по ссылке](https://www.comss.ru/page.php?id=7726).

- Если вы устанавливали hyper-v или docker desktop, то  все равно может возникать ошибка:  
`Stderr: VBoxManage: error: AMD-V VT-X is not available (VERR_SVM_NO_SVM)`   
 Попробуйте в этом случае выполнить в Windows от администратора команду `bcdedit /set hypervisorlaunchtype off` и перезагрузиться.

- Если ваша рабочая станция в меру различных факторов не может запустить вложенную виртуализацию - допускается неполное выполнение(до ошибки запуска ВМ)

## Решение

![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/06_ubuntu-20.04.box.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/07_ubuntu-20.04.box.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/08_ubuntu-20.04.box.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/09_vm_ubuntu.png)


## Задача 3

1. Отредактируйте файл    [mydebian.json.pkr.hcl](https://github.com/netology-code/virtd-homeworks/blob/shvirtd-1/05-virt-02-iaac/src/mydebian.json.pkr.hcl)  или [mydebian.jsonl](https://github.com/netology-code/virtd-homeworks/blob/shvirtd-1/05-virt-02-iaac/src/mydebian.json) в директории src (packer умеет и в json, и в hcl форматы):
   - добавьте в скрипт установку docker. Возьмите скрипт установки для debian из  [документации](https://docs.docker.com/engine/install/debian/)  к docker, 
   - дополнительно установите в данном образе htop и tmux.(не забудьте про ключ автоматического подтверждения установки для apt)
3. Найдите свой образ в web консоли yandex_cloud
4. Необязательное задание(*): найдите в документации yandex cloud как найти свой образ с помощью утилиты командной строки "yc cli".
5. Создайте новую ВМ (минимальные параметры) в облаке, используя данный образ.
6. Подключитесь по ssh и убедитесь в наличии установленного docker.
7. Удалите ВМ и образ.
8. **ВНИМАНИЕ!** Никогда не выкладываете oauth token от облака в git-репозиторий! Утечка секретного токена может привести к финансовым потерям. После выполнения задания обязательно удалите секретные данные из файла mydebian.json и mydebian.json.pkr.hcl. (замените содержимое токена на  "ххххх")
9. В качестве ответа на задание  загрузите результирующий файл в ваш ЛК.

## Решение
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/10_yc_packer.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/11_yc_packer2.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/12_yc_image.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/13_vm_image_yc.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/14_vm_yc.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/15_vm_yc2.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/16_vm_yc3.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/17_vm_yc4.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/18_yc_image.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/19_yc_image2.png)
![vagrant](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/screen/20_vm_yc_ubuntu.png)

![mydebian.json.pkr.hcl.clean](https://github.com/valdemar-2502/Homework-Applying-IaaC-Principles-to-Virtual-Machines/blob/main/mydebian.json.pkr.hcl.clean)


