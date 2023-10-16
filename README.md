# argocd
- Series về học ArgoCD/ Demo thực tế
- Các ví dụ liên quan đến ArgoCD từ cơ bản đến nâng cao<br/>

# Môi trường phát triển
- Ubuntu 20.04 LTS
- K8S được xây dựng dựa trên K3S + K3D (cluster có 1 node)
  ```shell
  k3d cluster create k3s-dev --port 7100:80@loadbalancer --port 7143:443@loadbalancer --api-port 6443 --servers 1 --agents 1 --network common-network --token K3D_K3S_CHIEN_TOKEN
  ```
- Tham khảo cách xây dựng Cluster K8S:
  ```shell
  https://github.com/trandungchien1982/k8s/tree/01.HelloWorld
  ```
  
# Folder liên quan trên Windows
```
D:\Projects\argocd
```

==============================================================

# Ví dụ [01.HelloWorld]
==============================================================


**Tham khảo**
- https://viblo.asia/p/kubernetes-practice-trien-khai-nodejs-microservice-tren-kubernetes-phan-2-automatic-update-config-with-argocd-Qbq5QBMJKD8#_argocd-3
- https://argo-cd.readthedocs.io/en/stable/cli_installation/


**Tiến hành cài đặt ArgoCD trên localhost để thực hành:**<br/>
- Cài đặt ArgoCD bên trong 1 Kubernetes Cluster với namespace `argocd`
```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
- Cài đặt ArgoCD CLI
```shell
curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
chmod +x /usr/local/bin/argocd
```

- Kiểm tra các Pods liên quan đến ArgoCD đã chạy thành công hay chưa
```shell
kubectl get pod -n argocd
----------------------------------------------------------------------------------------------
NAME                                                READY   STATUS    RESTARTS      AGE
argocd-applicationset-controller-6b5b54d544-4h8mh   1/1     Running   1 (80m ago)   13h
argocd-notifications-controller-746457787-zjlht     1/1     Running   1 (80m ago)   13h
argocd-application-controller-0                     1/1     Running   1 (80m ago)   13h
argocd-dex-server-7656cd55f4-jjh4p                  1/1     Running   3 (80m ago)   13h
argocd-server-5db7487c98-qszpj                      1/1     Running   1 (80m ago)   13h
argocd-redis-774dbf45f8-57v2f                       1/1     Running   1 (80m ago)   13h
argocd-repo-server-76d7968f94-r75wx                 1/1     Running   1 (80m ago)   13h
```

- Lấy password mặc định của `admin` 
```shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
-----------------------------------------------------------------------------------------------------
sQWSd6mQzvxBuQBt
```

- Tiến hành Port Forwarding để truy cập vào trang chủ ArgoCD.<br>
Do ta dùng **https** trên **localhost** nên cần chọn _proceed to localhost(unsafe)_
```shell
kubectl port-forward svc/argocd-server -n argocd 8080:443
----------------------------------------------------------------
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```

**Truy cập trang quản lý ArgoCD**<br/>
```shell
http://localhost:8080
User: admin
Password: {mặc định, lấy từ script CLI ở trên: sQWSd6mQzvxBuQBt}
(Trong AdminUI, ta có chỗ để change password admin)
```
**Tiến hành tạo 1 New App trên ArgoCD để tự động hóa quy trình deploy ví dụ HelloWorld của K8S**
- File K8S config tham khảo ở đây: https://github.com/trandungchien1982/k8s/tree/01.HelloWorld
```shell
https://github.com/trandungchien1982/k8s/blob/01.HelloWorld/deploy-nginx-hello-world.yaml
```
