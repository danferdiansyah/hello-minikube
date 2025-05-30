<div align="center">

## Reflection on Hello Minikube

**Daniel Ferdiansyah, 2306275052**

</div>

---
1. **Comparation before and after exposed the minikube as a Service**

    **Sebelum di-expose sebagai Service**:
    
    Ketika menjalankan kubectl logs hello-node-c74958b5d-d656h, log hanya menunjukkan pesan startup dari agnhost, yaitu
    ```shell
    PS C:\WINDOWS\system32> kubectl logs hello-node-c74958b5d-d656h
    I0526 08:29:03.423123       1 log.go:195] Started HTTP server on port 8080
    I0526 08:29:03.423456       1 log.go:195] Started UDP server on port  8081
    ```
    
    **Setelah di-expose sebagai Service dan diakses**:
    
    Setelah menjalankan `kubectl expose deployment hello-node --type=LoadBalancer --port=8080`  dan kemudian `minikube service hello-node`, muncul sebuah tunnel yang dapat digunakan untuk mengakses Service tersebut dari browser. Dan, ya, setiap kali membuka atau me-refresh URL aplikasi di browser (pada kasus saya `http://127.0.0.1:60332/`), aplikasi di dalam Pod akan menerima request HTTP. Log baru juga muncul tiap saya mengakses aplikasi. Berikut log setelah saya coba buka aplikasi sebanyak dua kali
    ```shell
    PS C:\WINDOWS\system32> kubectl logs hello-node-c74958b5d-d656h
    I0526 08:29:03.423123       1 log.go:195] Started HTTP server on port 8080
    I0526 08:29:03.423456       1 log.go:195] Started UDP server on port  8081
    I0526 08:30:38.200835       1 log.go:195] GET /
    I0526 08:30:38.907298       1 log.go:195] GET /
    I0526 08:35:59.472832       1 log.go:195] GET /
    I0526 08:35:59.562443       1 log.go:195] GET /
    ```

2. **Purpose of `n`**

      `n` atau `--namespace` pada command `kubectl` digunakan untuk menentukan namespace di mana perintah tersebut akan dieksekusi pada sumber daya yang akan dicari atau dimanipulasi.
      
      Ketika kamu menjalankan perintah seperti kubectl get deployments atau kubectl get pods tanpa opsi -n (seperti pada langkah melihat deployment hello-node ), kubectl secara default menargetkan namespace default. Pod hello-node yang kamu buat juga berada di namespace default karena tidak ada namespace lain yang ditentukan saat pembuatan.
      
      Namespace kube-system adalah tempat di mana komponen-komponen inti dan add-on dari sistem Kubernetes itu sendiri berjalan. Contohnya adalah DNS cluster (seperti CoreDNS), proxy jaringan, dan dalam tutorial ini, metrics-server.
      
      Jadi, ketika menjalankan `kubectl get pods -n kube-system` atau `kubectl get services -n kube-system`, sistem meminta daftar pod atau service yang ada khusus di dalam namespace kube-system.
      Outputnya tidak mencantumkan pod (hello-node-xxxxx) atau service (hello-node) yang dibuat secara eksplisit karena pod dan service tersebut berada di namespace default, bukan kube-system dan setiap namespace memiliki lingkup sumber dayanya sendiri.

---

<div align="center">
   
## Reflection on Rolling Update and Kubernetes Manifest File
   
</div>

1. **What is the difference between Rolling Update and Recreate deployment strategy?**

      Rolling update merupakan approach untuk memperbarui aplikasi secara bertahap dengan mengganti satu per satu pod lama dengan pod baru, sehingga service tetap berjalan ketika proses berlangsung. Sedangkan, Recreate merupakan strategi untuk menghentikan semua pod lama, lalu launch pod baru, sehingga ada downtime Dengan recreate deployment strategy, dipastikan hanya ada satu versi aplikasi yang aktif dalam satu waktu.

2. **Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt. reset dan start**

      Start minikube dan buat deployment
      ![image](https://github.com/user-attachments/assets/8b5972c4-c98a-4e78-9d17-331217ee24bc)
      
      Verify deployment dan pods ready
      ![image](https://github.com/user-attachments/assets/82716ab7-f621-4284-b6a8-120515b12abe)
      ![image](https://github.com/user-attachments/assets/a999d6c4-d515-4a6d-995f-97f7c60ca3c9)
      
      Expose deployment dan get services
      ![image](https://github.com/user-attachments/assets/3f0de85f-cb4e-4884-957d-7b5579c96472)
      
      Jalankan aplikasi
      ![Screenshot 2025-05-30 194910](https://github.com/user-attachments/assets/ca4b5aba-760e-477b-9557-5d5f7b14ec8c)

      Scale menjadi 4 replicas dan pastikan semuanya running
      ![image](https://github.com/user-attachments/assets/b7dfacee-5ea4-4e36-b306-79ec15295454)


3. **Prepare different manifest files for Executing Recreate Deployment Strategy**
      ![image](https://github.com/user-attachments/assets/97792708-0539-47c0-8d22-440b265d726a)

4. **What do you think are the benefits of using Kubernetes manifest files?**
   
      Dengan manifest file, saya bisa simpan konfigurasi dari deployment yang telah dibuat dan menggunakannya ulang ke depannya. Hal ini saya pikir semacam *save game* untuk minikube.
 


