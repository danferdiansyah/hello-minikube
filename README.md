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
