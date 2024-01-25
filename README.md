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

# Ví dụ [02.TwoAppsIn2Namespaces]
==============================================================


**Tham khảo mã nguồn dựng K8S với 2 Namespaces khác nhau dùng YAML files:**
- https://github.com/trandungchien1982/k8s/tree/07.TwoPublicServicesWithDifferentNS
- Ta cần map IP address 127.0.0.1 với 2 domain: `dev.chien.com` và `stg.chien.com`

**Tạo 2 Apps khác nhau trên những namespace riêng biệt:**<br/>
- Namespace 01 có tên `dev-custom-ns` => Deploy xong sẽ ra URL: http://dev.chien.com:7100
- Namespace 02 có tên `stg-custom-ns` => Deploy xong sẽ ra URL: http://stg.chien.com:7100
- Ta sẽ dùng chung config k8s có trong `/main-service` cho 2 Namespaces ở trên
- Bản thân New Apps khi tạo ra thì ta có thể điều chỉnh file YAML và tiến hành customize New Apps dựa vào file YAML config.
```shell
  ArgoCD-NewApp-dev.yaml
  ArgoCD-NewApp-stg.yaml
```

- Kiểm tra các pods đang chạy trên namespace `dev-custom-ns`:
```shell
kubectl get pod -n dev-custom-ns
----------------------------------------------------------------------------------------------
NAME                                   READY   STATUS    RESTARTS   AGE
service1-deployment-66455d7d4f-9vjz6   1/1     Running   0          12m
```

- Kiểm tra các pods đang chạy trên namespace `stg-custom-ns`:
```shell
kubectl get pod -n stg-custom-ns
----------------------------------------------------------------------------------------------
NAME                                   READY   STATUS    RESTARTS   AGE
service1-deployment-66455d7d4f-f8gdt   1/1     Running   0          6m33s
```

**Kiểm tra toàn bộ thông tin K8S dùng K9S CLI:**
```shell
  k9s
  ----------------------------------------------------------------------------------------------
  Context: k3d-k3s-dev                              <0> all       <a>      Attach     <l>       Logs               <y> YAML                                          ____  __.________        
  Cluster: k3d-k3s-dev                              <1> default   <ctrl-d> Delete     <p>       Logs Previous                                                       |    |/ _/   __   \______ 
  User:    admin@k3d-k3s-dev                                      <d>      Describe   <shift-f> Port-Forward                                                        |      < \____    /  ___/ 
  K9s Rev: v0.27.4                                                <e>      Edit       <s>       Shell                                                               |    |  \   /    /\___ \  
  K8s Rev: v1.23.6+k3s1                                           <?>      Help       <n>       Show Node                                                           |____|__ \ /____//____  > 
  CPU:     1%                                                     <ctrl-k> Kill       <f>       Show PortForward                                                            \/            \/  
  MEM:     1%                                                                                                                                                                                 
┌────────────────────────────────────────────────────────────────────────────────────── Pods(all)[17] ──────────────────────────────────────────────────────────────────────────────────────┐
│ NAMESPACE↑      NAME                                                PF  READY   RESTARTS STATUS       CPU  MEM  %CPU/R  %CPU/L  %MEM/R  %MEM/L IP           NODE                   AGE    │
│ argocd          argocd-application-controller-0                     ●   1/1            4 Running        9   70     n/a     n/a     n/a     n/a 10.42.1.56   k3d-k3s-dev-agent-0    37h    │
│ argocd          argocd-applicationset-controller-6b5b54d544-4h8mh   ●   1/1            4 Running        1   19     n/a     n/a     n/a     n/a 10.42.1.51   k3d-k3s-dev-agent-0    37h    │
│ argocd          argocd-dex-server-7656cd55f4-jjh4p                  ●   1/1            6 Running        1   16     n/a     n/a     n/a     n/a 10.42.0.46   k3d-k3s-dev-server-0   37h    │
│ argocd          argocd-notifications-controller-746457787-zjlht     ●   1/1            4 Running        1   18     n/a     n/a     n/a     n/a 10.42.1.52   k3d-k3s-dev-agent-0    37h    │
│ argocd          argocd-redis-774dbf45f8-57v2f                       ●   1/1            4 Running        2    3     n/a     n/a     n/a     n/a 10.42.0.42   k3d-k3s-dev-server-0   37h    │
│ argocd          argocd-repo-server-76d7968f94-r75wx                 ●   1/1            4 Running        1   41     n/a     n/a     n/a     n/a 10.42.0.40   k3d-k3s-dev-server-0   37h    │
│ argocd          argocd-server-5db7487c98-qszpj                      ●   1/1            4 Running        1   27     n/a     n/a     n/a     n/a 10.42.1.49   k3d-k3s-dev-agent-0    37h    │
│ dev-custom-ns   service1-deployment-66455d7d4f-9vjz6                ●   1/1            0 Running        0    1       0       0       1       1 10.42.0.47   k3d-k3s-dev-server-0   20m    │
│ kube-system     coredns-d76bd69b-zjxkq                              ●   1/1            4 Running        3   14       3     n/a      20       8 10.42.0.41   k3d-k3s-dev-server-0   37h    │
│ kube-system     helm-install-traefik-7ksdr                          ●   0/1            0 Completed      0    0     n/a     n/a     n/a     n/a 10.42.0.4    k3d-k3s-dev-server-0   37h    │
│ kube-system     helm-install-traefik-crd-t6qk6                      ●   0/1            0 Completed      0    0     n/a     n/a     n/a     n/a 10.42.1.2    k3d-k3s-dev-agent-0    37h    │
│ kube-system     local-path-provisioner-6c79684f77-vrnx8             ●   1/1            7 Running        1    6     n/a     n/a     n/a     n/a 10.42.0.45   k3d-k3s-dev-server-0   37h    │
│ kube-system     metrics-server-7cd5fcb6b7-g7npt                     ●   1/1            4 Running        8   18       8     n/a      26     n/a 10.42.1.48   k3d-k3s-dev-agent-0    37h    │
│ kube-system     svclb-traefik-8sbgv                                 ●   2/2            8 Running        0    2     n/a     n/a     n/a     n/a 10.42.1.47   k3d-k3s-dev-agent-0    37h    │
│ kube-system     svclb-traefik-tfpcw                                 ●   2/2            8 Running        0    1     n/a     n/a     n/a     n/a 10.42.0.39   k3d-k3s-dev-server-0   37h    │
│ kube-system     traefik-df4ff85d6-dt7bn                             ●   1/1            4 Running        1   22     n/a     n/a     n/a     n/a 10.42.1.50   k3d-k3s-dev-agent-0    37h    │
│ stg-custom-ns   service1-deployment-66455d7d4f-f8gdt                ●   1/1            0 Running        0    1       0       0       1       1 10.42.1.57   k3d-k3s-dev-agent-0    13m    │
│                                                                                                                                                                                           │
└───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

**Truy cập vào các trang tương ứng:**
```shell
  http://dev.chien.com:7100 => DEV UI
  http://stg.chien.com:7100 => STG UI
```

3. Phiên bản thứ ba