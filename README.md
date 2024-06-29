### Домашнее задание к занятию "08.02 Работа с Playbook"
#### Подготовка к выполнению
1. Создайте свой собственный (или используйте старый) публичный репозиторий на github с 
произвольным именем. [github repository](https://github.com/Bora2k3/08-ansible-02-playbook) 
2. Скачайте [playbook](./playbook/) из репозитория с домашним заданием и перенесите его в 
свой репозиторий. 3. Подготовьте хосты в соотвтествии с группами из предподготовленного 
playbook. ```bash version: '3' services:
  elastic: image: pycontribs/ubuntu container_name: elastic restart: unless-stopped 
    entrypoint: "sleep infinity"
  kibana: image: pycontribs/ubuntu container_name: kibana restart: unless-stopped 
    entrypoint: "sleep infinity"
``` 4. Скачайте дистрибутив 
[java](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html) и положите его 
в директорию `playbook/files/`.
#### Основная часть
1. Приготовьте свой собственный inventory файл `prod.yml`. ```yaml --- elasticsearch: 
    hosts:
      elastic: ansible_connection: docker kibana: hosts: kibana: ansible_connection: docker 
``` 2. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает 
kibana. 3. При создании tasks рекомендую использовать модули: `get_url`, `template`, 
`unarchive`, `file`. 4. Tasks должны: скачать нужной версии дистрибутив, выполнить 
распаковку в выбранную директорию, сгенерировать конфигурацию с параметрами. 5. Запустите 
`ansible-lint site.yml` и исправьте ошибки, если они есть. 6. Попробуйте запустить playbook 
на этом окружении с флагом `--check`. ```bash $ sudo ansible-playbook -i 
./inventory/prod.yml site.yml --check [WARNING]: Found both group and host with same name: 
kibana PLAY [Install Java] 
********************************************************************************************************************************************************************************************************** 
TASK [Gathering Facts] 
******************************************************************************************************************************************************************************************************* 
ok: [kibana] ok: [elastic] TASK [Set facts for Java 11 vars] 
******************************************************************************************************************************************************************************************** 
ok: [elastic] ok: [kibana] TASK [Upload .tar.gz file containing binaries from local 
storage] 
************************************************************************************************************************************************************ 
changed: [kibana] changed: [elastic] TASK [Ensure installation dir exists] 
**************************************************************************************************************************************************************************************** 
changed: [elastic] changed: [kibana] TASK [Extract java in the installation directory] 
**************************************************************************************************************************************************************************** 
An exception occurred during task execution. To see the full traceback, use -vvv. The error 
was: NoneType: None fatal: [elastic]: FAILED! => {"changed": false, "msg": "dest 
'/opt/jdk/11.0.15' must be an existing dir"} An exception occurred during task execution. 
To see the full traceback, use -vvv. The error was: NoneType: None fatal: [kibana]: FAILED! 
=> {"changed": false, "msg": "dest '/opt/jdk/11.0.15' must be an existing dir"} PLAY RECAP 
******************************************************************************************************************************************************************************************************************* 
elastic : ok=4 changed=2 unreachable=0 failed=1 skipped=0 rescued=0 ignored=0 kibana : ok=4 
changed=2 unreachable=0 failed=1 skipped=0 rescued=0 ignored=0 ``` 7. Запустите playbook на 
`prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены. 8. 
Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен. 9. 
Подготовьте README.md файл по своему playbook. В нём должно быть описано: что делает 
playbook, какие у него есть параметры и теги. 
[README.md](https://github.com/Bora2k3/08-ansible-02-playbook/blob/main/playbook/README.md) 
10. Готовый playbook выложите в свой репозиторий, в ответ предоставьте ссылку на него.
[playbook](https://github.com/Bora2k3/08-ansible-02-playbook/tree/main/playbook)
