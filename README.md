# 建立安裝 team 服務器 - 以 nexus3-oss 為例 #

最後結果 [http://35.229.249.41](http://35.229.249.41)

## 設定當前專案 ##

1. 建立一個新GCP專案並切換至該專案底下，本範例的專案名稱為 *nexus3-oss-demo*

2. 安裝好本機 gcloud 指令並登入後即可設定當前預設專案

    gcloud config set project nexus3-oss-demo

## 建立 一個 k8s 叢集 與 NTFS 服務 ##

1. 建立 k8s 叢集服務

    gcloud container clusters create k8s-service

2. 建立一個 PD 空間
  
    gcloud compute disks create --size=100GB gce-nfs-disk

3. 建立 NFS 服務

    kubectl apply -f demo-testing-nfs-server.yaml

4. 建立服務對外連結

    kubectl apply -f demo-testing-nfs-server-service.yaml

5. 建立服務取用權限
  
    kubectl apply -f demo-testing-nfs-server-pv-pvc.yaml

6. 確認當前 pods name

    kubectl get pods

7. 進入 NFS Server 節點並給予對應資料夾權限讓資料可以成功寫入
  
    kubectl exec -it nfs-server-66fcdd7546-c7lfb -c nfs-server bash
    chmod -R 777 exports

## 建立 nexus3-oss 服務 ##

1. 將做好的image 加入特定格式 tag 後 push 進入 gcr.io(專案名稱內的-換_)

    docker tag sonatype/nexus3 gcr.io/nexus3_oss_demo/sonatype_nexus3

    docker push gcr.io/nexus3_oss_demo/sonatype_nexus3

  接著前往專案底下的 images registry 即可看到[https://console.cloud.google.com/gcr/images/nexus3-oss-demo?project=nexus3-oss-demo](https://console.cloud.google.com/gcr/images/nexus3-oss-demo?project=nexus3-oss-demo) 即可看到

2. 建立 nexus3-oss 服務

    kubectl apply -f demo-testing.yaml

3. 進入cluster 外部ip 端口即可

## 設定 nexus3-oss 登入權限 ##

參考 [One More Universal Package Manager - Nexus Repository Manager](https://blackie1019.github.io/tags/Nexus-Repository-Manager/)

## 設定 nuget 套件與上傳 ##

參考 nuget-setting.md