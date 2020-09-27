Документ описывает установку и настройку cloud provider для kubernetes

!!!Смена пароля для технического пользователя cloud provider!!!
* Заменить пароль в group_vars/vault.yaml
* Запустить playbook site.yaml с параметром --tags "tech_password_changed" 

Установка vsphere_cp
* Запустить playbook site.yaml с параметром --tags "vsphere_cp"

Как проверить - kubectl get pods -n kube-system
* Pods vsphere* должны быть в состоянии Running

Как проверить работоспособсность с созданием тестовых ресурсов
Перейти в папку /opt/formr/ansible/roles/vsphere_cp/files/test_cp, запустить скрипт cloud_provider.sh
По итогу в вышеуказанной папке появится файл importantPage2.html

Cloud Provider позволяет использовать VMware Storages в Kubernetes для запуска Statefulset приложений
ОПИСАНИЕ РОЛИ:
Cloud Provider состоит из двух компонентов, которые устанавливаются последовательно.

Cloud Provider Interface - инициализирует ВМ для использования Cloud Provider'ом через taints и VM UUID, и взаимодействует с ресурсами vCenter.
* Taints должны быть назначены до установки CPI.
* Других taints на нодах в момент установки манифестов CPI быть не должно.
* После установки манифестов CPI taints должны удалиться.
* Манифесты nodes в kubernetes должны быть пропатчены до установки CPI.
* Вышеописанные шаги реализованы в роли.
* Прочие шаги описаны в документации.
После успешной установки CPI нужно установить Container Storage Interface.
* CSI обеспечивает интерфейс взаимодействия между контейнерами и persistent volumes.
* Установка CSI описана в документации.

Последняя актуальная версия документации на 01.2020
https://cloud-provider-vsphere.sigs.k8s.io/tutorials/kubernetes-on-vsphere-with-kubeadm.html

Необходимые шаги, не указанные в документации:
1) Пользователь, под которым работает cloud provider, должен иметь следующие права для реализации dynamic provisioning:
https://vmware.github.io/vsphere-storage-for-kubernetes/documentation/vcp-roles.html#dynamic-provisioning

2) Storage Policy для cloud provider - создавалась для взаимодействия с vmdk, в оф. доке есть взаимоисключающие параметры.
На странице Policy structure выбрать "Использовать настройки хоста" 
Если Storage Policy создается для vmdk, то vSan настроек в Storage Policy быть не должно.

3) Для kubelet на каждой ноде кластера необходимо указать переменную окружения "--cloud-provider=external" в /etc/sysconfig/kubelet

4) Для использования ansible-модуля для vmware необходимо установить python2-pyvmomi-6.7.3-4.el7
