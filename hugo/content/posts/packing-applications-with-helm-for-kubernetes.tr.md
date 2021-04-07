---
title: "Helm ile Kubernetes Uygulamalarını Paketlemek"
date: 2021-04-07T22:29:57+03:00
draft: false
tags: ["kubernetes","helm"]
categories: [Kubernetes]
summary: "Helm ile Kubernetes Uygulamalarını Paketlemek"
---
Merhaba,

Bu yazıda sizlere elimden geldiğince helm anlatacağım.
<!--more--> 

________
Konu Başlıkları

1. [Helm](#helm)
    1. [Helm Nedir?](#helm-nedir)
    2. [Neden Helm Kullanmalıyız?](#neden-helm-kullanmalıyız)

2. [Kurulum](#kurulum)
    1. [Kubernetes ve Kubectl Kurulumu](#kubernetes-ve-kubectl-kurulumu)
    2. [Helm Kurulumu](#helm-kurulumu)

3. [Helm Alıştırmaları](#helm-alıştırmaları)
    1. [Install](#install)
    2. [Uninstall](#uninstall)

4. Helm Chart Oluşturma
    1. Helm Chart Yapısı
    2. Helm Chart Oluşturma
    3. Demo : Helm Chart ile Kurulum

5. İleri Seviye Kullanım
    1. Helm Template
    2. Helm Template Data

    [Liste Güncellenecek]
    



# Helm
## Helm Nedir?
Helm, Kubernetes üzerinde uygulama tanımlamak,kurmak, yükseltmek, geri almak ve paketlemek için Cloud Native Computing Foundation tarafından tanınmış bir paket yöneticisidir.
## Neden Helm Kullanmalıyız?
> Helm allows you to define, install and upgrade even the most complex deployments.

Complex deployment ile ifade edilen, ConfigMap, Secret, PV, PVC, Service, Deployment, DeaemonSet, Ingress gibi Kubernetes kaynak türleridir(Kind).
Helm tam olarak burada tüm türleri "Chart" adındaki paket formatıyla paketler. 

Kurulum, versiyon yükseltme, rollback gibi bir çok işlemi helm ile kolayca yapabilirsiniz.

# Kurulum
## Kubernetes ve Kubectl Kurulumu
[Minikube Kurulumu](https://sckn.dev/tr/posts/minikube-installation/)

Kubectl kurulumu için aşağıdaki yönergeleri takip edebilirsiniz.

Linux
````
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
````
Mac
````
brew install kubectl 
````
Windows
````
choco install kubernetes-cli
````

## Helm Kurulumu
Linux
````
choco install kubernetes-helm
````
Mac
````
brew install helm
````
Windows
````
choco install kubernetes-helm
````
Kurulum yaptıktan sonra 
```
seckin@mac ➜ helm version
version.BuildInfo{Version:"v3.5.0", GitCommit:"32c22239423b3b4ba6706d450bd044baffdcf9e6", GitTreeState:"dirty", GoVersion:"go1.15.6"}

seckin@mac ➜ #kısa öz olarak
seckin@mac ➜ helm version --short
v3.5.0+g32c2223
```

ile helm versiyon kontrolü yapabilirsiniz.


# Helm Alıştırmaları
Kurulum Örneklerine geçmeden önce stabil repoyu ekleyelim
````
helm repo add stable https://charts.helm.sh/stable
````

## Install
Örnek olarak mysql kurulumu yapalım.
```
seckin@mac ➜  helm install demo-mysql stable/mysql
WARNING: This chart is deprecated
NAME: demo-mysql
LAST DEPLOYED: Wed Apr  7 23:34:45 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
MySQL can be accessed via port 3306 on the following DNS name from within your cluster:
demo-mysql.default.svc.cluster.local

To get your root password run:

    MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default demo-mysql -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)

To connect to your database:

1. Run an Ubuntu pod that you can use as a client:

    kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il

2. Install the mysql client:

    $ apt-get update && apt-get install mysql-client -y

3. Connect using the mysql cli, then provide your password:
    $ mysql -h demo-mysql -p

To connect to your database directly from outside the K8s cluster:
    MYSQL_HOST=127.0.0.1
    MYSQL_PORT=3306

    # Execute the following command to route the connection:
    kubectl port-forward svc/demo-mysql 3306

    mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}
```

Kontrolümüzü yapalım
```
seckin@mac ➜ kubectl get all|grep mysql
pod/demo-mysql-8674bf7d9b-lpgp2   1/1     Running   0          72s
service/demo-mysql   ClusterIP   10.108.51.147   <none>        3306/TCP   72s
deployment.apps/demo-mysql   1/1     1            1           72s
replicaset.apps/demo-mysql-8674bf7d9b   1         1         1       72s
```


## Uninstall
```
seckin@mac ➜ helm uninstall demo-mysql
release "demo-mysql" uninstalled
```
```
seckin@mac ➜ kubectl get all|grep mysql
```

