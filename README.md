# Домашнее задание к занятию "13.2 разделы и монтирование"


## Задание 1: подключить для тестового конфига общую папку
### Конфигурация тестовой среды для фронтенда и бэкенда - [app-deploy.yaml](app-deploy.yaml)

```
rimm@cp1:~$ kubectl exec firstapp-689f56df87-n6fxn -c debian -- touch /tmp/123.txt
rimm@cp1:~$ kubectl exec firstapp-689f56df87-n6fxn -c debian -- ls /tmp/
123.txt
rimm@cp1:~$ kubectl exec firstapp-689f56df87-n6fxn -c frontend -- ls /static
123.txt
```
## Задание 2: подключить общую папку для прода
### Конфигурация PV - [pv.yaml](pv.yaml), Конфигурация PVC - [pvc.yaml](pvc.yaml), фронтенда - [frontend.yaml](frontend.yaml), бэкенда - [backend.yaml](backend.yaml) 

После запуска всего, поды с фронтом и бэком не запускались
дескрайб выдал
```
Mounting command: mount
Mounting arguments: -t nfs 10.0.10.3:/nfs /var/lib/kubelet/pods/dcb49a3f-4c50-4f28-88bb-e9eedcf06699/volumes/kubernetes.io~nfs/nfs-pv
Output: mount: /var/lib/kubelet/pods/dcb49a3f-4c50-4f28-88bb-e9eedcf06699/volumes/kubernetes.io~nfs/nfs-pv: bad option; for several filesystems (e.g. nfs, cifs) you might need a /sbin/mount.<type> helper program.
```
начал гуглить ошибку, говорят надо поставить пакет nfs-common на ноде, но это не помогло
попробовал переставить инструкции nfs-server на control plane
```
установить helm: curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
добавить репозиторий чартов: helm repo add stable https://charts.helm.sh/stable && helm repo update
установить nfs-server через helm: helm install nfs-server stable/nfs-server-provisioner
```

после переустановки nfs-server на control plane в дескрайбе 
```
Mounting command: mount
Mounting arguments: -t nfs 10.0.10.3:/nfs /var/lib/kubelet/pods/703dd08c-1c42-49ee-9b50-21a7947d567a/volumes/kubernetes.io~nfs/nfs-pv
Output: mount.nfs: requested NFS version or transport protocol is not supported
```
тут у меня опустились руки.
