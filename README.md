### Работа с картами конфигураций через утилиту kubectl в установленном minikube

- Как создать карту конфигураций?
```
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl create configmap nginx.conf --from-file=nginx.conf 
configmap/nginx.conf created
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl create configmap domain --from-literal=name=netology.ru
configmap/domain created
```

- Как просмотреть список карт конфигураций?
```
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl get configmaps
NAME               DATA   AGE
domain             1      8s
kube-root-ca.crt   1      128m
nginx.conf         1      55s
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl get configmap
NAME               DATA   AGE
domain             1      15s
kube-root-ca.crt   1      128m
nginx.conf         1      62s
```

- Как просмотреть карту конфигурации?
```
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl get configmaps nginx.conf 
NAME         DATA   AGE
nginx.conf   1      3m35s
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl describe configmaps domain 
Name:         domain
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
name:
----
netology.ru

BinaryData
====

Events:  <none>
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ 
```
- Как получить информацию в формате YAML и/или JSON?

```
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl get configmaps nginx.conf -o yaml
apiVersion: v1
data:
  nginx.conf: |
    server {
        listen 80;
        server_name  netology.ru www.netology.ru;
        access_log  /var/log/nginx/domains/netology.ru-access.log  main;
        error_log   /var/log/nginx/domains/netology.ru-error.log info;
        location / {
            include proxy_params;
            proxy_pass http://10.10.10.10:8080/;
        }
    }
kind: ConfigMap
metadata:
  creationTimestamp: "2022-11-21T06:51:48Z"
  name: nginx.conf
  namespace: default
  resourceVersion: "13887"
  uid: 668f0566-e5d2-4c30-98f7-19cb6e3d2477
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl get configmaps nginx.conf -o json
{
    "apiVersion": "v1",
    "data": {
        "nginx.conf": "server {\n    listen 80;\n    server_name  netology.ru www.netology.ru;\n    access_log  /var/log/nginx/domains/netology.ru-access.log  main;\n    error_log   /var/log/nginx/domains/netology.ru-error.log info;\n    location / {\n        include proxy_params;\n        proxy_pass http://10.10.10.10:8080/;\n    }\n}\n"
    },
    "kind": "ConfigMap",
    "metadata": {
        "creationTimestamp": "2022-11-21T06:51:48Z",
        "name": "nginx.conf",
        "namespace": "default",
        "resourceVersion": "13887",
        "uid": "668f0566-e5d2-4c30-98f7-19cb6e3d2477"
    }
}
```

- Как выгрузить карту конфигурации и сохранить его в файл?

```
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl get configmaps -o json > configmaps.json
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl get configmap nginx.conf -o yaml > nginx-config.yml
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ ls -l | grep '11:57'
-rw-r--r-- 1 vlad vlad 3214 ноя 21 11:57 configmaps.json
-rw-r--r-- 1 vlad vlad  564 ноя 21 11:57 nginx-config.yml
```

- Как удалить карту конфигурации?
```
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl delete configmap nginx.conf 
configmap "nginx.conf" deleted
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl get configmaps
NAME               DATA   AGE
domain             1      6m38s
kube-root-ca.crt   1      134m
```

- Как загрузить карту конфигурации из файла?
```
lad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl apply -f nginx-config.yml
configmap/nginx.conf created
vlad@home:~/Documents/Netology/DevOps/netology-DevOps-dz_67$ kubectl get configmaps
NAME               DATA   AGE
domain             1      7m25s
kube-root-ca.crt   1      135m
nginx.conf         1      2s
```