#### Membuat ConfigMap

```{.yaml .copy}
apiVersion: v1
kind: ConfigMap
metadata:
  name: example-configmap
data:
  app_title: API Authentikasi
  app_config: |
    {
      "host": "example.com",
      "port": "8080"
    }
```

Untuk membuat ConfigMap, buat file `configmap.yaml` dan copy definisi YAML di atas. Kemudian, jalankan perintah berikut:

```{.bash .copy}
kubectl create -f configmap.yaml
```

Setelah berhasil membuat ConfigMap, kita dapat melihat daftar ConfigMap yang sudah dibuat dengan menjalankan perintah berikut:

```{.bash .copy}
kubectl get configmap
```

Untuk menampilkan detail isi dari ConfigMap, gunakan perintah berikut

```{.bash .copy}
kubectl describe configmaps example-configmap
```

```{.bash}
Name:         example-configmap
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
app_title:
----
API Authentikasi
app_config:
----
{
  "host": "example.com",
  "port": "8080"
}


BinaryData
====

Events:  <none>
```

#### Menggunakan ConfigMap Env Pada Pod

Setelah berhasil membuat ConfigMap, kita dapat menggunakan ConfigMap pada resource Kubernetes seperti Pod.

Untuk menggunakan ConfigMap pada Pod, buat file `pod.yml` dan tambahkan konfigurasi berikut:

```{.yaml .copy}
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: example-container
    image: nginx
    env:
    - name: HOST_TITLE
      valueFrom:
        configMapKeyRef:
          name: example-configmap
          key: app_title
```

Konfigurasi di atas akan membuat sebuah Pod dengan nama `example-pod` yang menggunakan config `app_title` pada ConfigMap `example-configmap`
Kemudian, jalankan perintah berikut untuk membuat Pod:

```{.bash .copy}
kubectl create -f pod.yaml
```

> Tunggu hingga Pod ready

Mari cek apakah configmap `app_title` berhasil ditambahkan pada environment variable pada Pod. Tunggu hingga Pod berjalan lalu eksekusi perintah berikut

```{.bash .copy}
kubectl exec example-pod -- env | grep HOST_TITLE
```

Perintah diatas akan menampilkan value dari `HOST_TITLE` yaitu

```{.txt}
API Authentikasi
```

#### Menggunakan ConfigMap File Pada Pod

```{.yaml .copy}
apiVersion: v1
kind: Pod
metadata:
  name: example-pod-2
spec:
  containers:
  - name: example-container
    image: nginx
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: example-configmap
        items:
        - key: app_config
          path: app_config
```

Konfigurasi di atas akan membuat sebuah Pod dengan nama `example-pod-2` yang membuat file baru pada `/etc/config` yang berisi config dari `app_config`

Buat file `pod-2.yaml` menggunakan konfigurasi diatas dan jalankan Pod menggunakan perintah berikut

```{.bash .copy}
kubectl create -f pod-2.yaml
```

> Tunggu hingga Pod ready

Mari cek apakah config berhasil ditambahkan dalam bentuk environment variable

```{.bash .copy}
kubectl exec example-pod-2 -- cat /etc/config/app_config
```

Perintah tersebut akan menampilkan value yang sama dengan config `app_config`

```{.bash}
{
  "host": "example.com",
  "port": "8080"
}
```
