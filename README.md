# 建立安裝 team 服務器 - 以 nexus3-oss 為例 #

最後結果 [http://35.229.249.41](http://35.229.249.41)

## 建立 一個 k8s 叢集 與 NTFS 服務 ##

1. 建立 k8s 叢集服務

  gcloud container clusters create k8s-service

2. 建立一個 PD 空間
  
  gcloud compute disks create --size=100GB gce-nfs-disk

3. 建立 NFS 服務

  kubectl apply -f demo-testing-nfs.yaml

4. 建立服務對外連結

  kubectl apply -f demo-testing-nfs-service.yaml

5. 建立服務取用權限
  
  kubectl apply -f demo-testing-nfs-server-pv-pvc.yaml

6. 進入 NFS Server 節點並給予對應資料夾權限讓資料可以成功寫入
  
  kubectl exec -it nfs-server-66fcdd7546-j6jhs -c nfs-server bash
  
  chmod -R 777 exports

## 建立 nexus3-oss 服務 ##

1. 將做好的image 加入特定格式 tag 後 push 進入 gcr.io

  docker push gcr.io/qatar_testing/sonatype_nexus3

2. 建立 nexus3-oss 服務

  kubectl apply -f demo-testing.yaml

3. 進入cluster 外部ip 端口即可

## 設定 nexus3-oss 登入權限 ##

參考 [One More Universal Package Manager - Nexus Repository Manager](https://blackie1019.github.io/tags/Nexus-Repository-Manager/)

## 設定 nuget 套件與上傳 ##

參考 nuget-setting.md