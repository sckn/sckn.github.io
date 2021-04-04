---
title: "Minikube Kurulumu"
date: 2021-04-04T17:38:57+03:00
draft: false
tags: [kubectl,"minikube"]
categories: [Kubernetes]
---
Merhaba,

Bu yazıda sizlere kişisel çalışma ortamınıza minikube kurulumunu ardından üzerinde örnekler yapmayı anlatmya çalışıcam.
<!--more--> 
[Orginal Referans Dökümanı](https://minikube.sigs.k8s.io/docs/start/)
________
Konu Başlıkları
1. [Kurulum](#kurulum)
2. [Minikube Cluter ile Çalışmak](#minikube-cluster-ile-çalışmak)
3. [Örnek](#örnek)

Kurulum
=======

Mac
```
brew install minikube
```

Linux Sistemler
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Windows
```
choco install minikube
```

Kurulum işlemini tamamladıktan sonra terminal emülator vasıtası ile aşağıdaki komut çalıştırılır.

```
seckin@mac > minikube start
😄  minikube v1.18.1 on Darwin 11.0.1
✨  Automatically selected the docker driver. Other choices: hyperkit, virtualbox, ssh
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.20.2 preload ...
    > preloaded-images-k8s-v9-v1....: 491.22 MiB / 491.22 MiB  100.00% 4.19 MiB
🔥  Creating docker container (CPUs=2, Memory=1990MB) ...
🐳  Preparing Kubernetes v1.20.2 on Docker 20.10.3 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v4
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

Çıktısında yukarıdaki gibi Done yazısını gördüğümüzde sorunsuz bir şekilde ayağa kalkmış demektir.

```
seckin@mac > kubectl get nodes
NAME       STATUS   ROLES                  AGE     VERSION
minikube   Ready    control-plane,master   2m23s   v1.20.2
```

kubectl ile çalışma ortamımızı kontrol edebilirsiniz.


Minikube Cluster ile Çalışmak
==============================

Hali hazırda kurulu olan sistemimizin podlarına erişmek için aşağıdaki komutu çalıştırmalıyız.

```
seckin@mac > kubectl get pods -A
NAMESPACE     NAME                               READY   STATUS    RESTARTS   AGE
kube-system   coredns-74ff55c5b-rs9j6            1/1     Running   0          22m
kube-system   etcd-minikube                      1/1     Running   0          22m
kube-system   kube-apiserver-minikube            1/1     Running   0          22m
kube-system   kube-controller-manager-minikube   1/1     Running   0          22m
kube-system   kube-proxy-c68qh                   1/1     Running   0          22m
kube-system   kube-scheduler-minikube            1/1     Running   0          22m
kube-system   storage-provisioner                1/1     Running   0          22m
```

Gördüğünüz gibi minikube çalışmak için ihtiyaç duyduğu etcd,coredns, proxy, scheduler ve storage gibi servisleri ayağa kaldırmış.

Uygulamamızın Dashboard'una erişmek için aşağıdaki komutu çalıştırmamzı yeterlidir.

```
minikube dashboard
```

Örnek
======
Bir örnek uygulama deploy edip yazımızı sonlandıralım.

```
seckin@mac > kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
deployment.apps/hello-minikube created
seckin@mac > kubectl expose deployment hello-minikube --type=NodePort --port=8080
service/hello-minikube exposed
seckin@mac > sleep 40 # 30 40 sn kadar beklememiz lazım 

seckin@mac > kubectl get services hello-minikube

NAME             TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
hello-minikube   NodePort   10.96.185.61   <none>        8080:31059/TCP   12s

seckin@mac > kubectl port-forward service/hello-minikube 7080:8080

Forwarding from 127.0.0.1:7080 -> 8080
Forwarding from [::1]:7080 -> 8080
Handling connection for 7080
Handling connection for 7080
```

Yukarıdaki adımları uygulayarak [http://localhost:7080/](http://localhost:7080/) adresinden uygulamamıza erişebilirsiniz.

Görüşmek üzere.

Seçkin.
