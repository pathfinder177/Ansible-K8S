Роль предназначена для развертывания Harbor registry
Роль может быть развернута в среде DEV или среде TEST
Harbor registry предназначен для хранения доверенных docker-образов

* Для установки роли в DEV-среде в папке ansible необходимо выполнить команду:
ansible-playbook -i inventories/dev/hosts site.yaml --tags "harbor" --ask-vault-pass

* Для установки роли в TEST-среде в папке ansible необходимо выполнить команду:
ansible-playbook -i inventories/test/hosts site.yaml --tags "harbor" --ask-vault-pass

* Для удаления роли из кластера на deployment-server необходимо выполнить команду:
helm uninstall harbor -n harbor-system
