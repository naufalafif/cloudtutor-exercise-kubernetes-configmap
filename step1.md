#### Apa Itu ConfigMap

ConfigMap adalah objek Kubernetes yang berisi data konfigurasi. Data konfigurasi ini dapat berupa teks, file konfigurasi, atau environment variable. Di dalam exercise ini, kita akan mempelajari bagaimana membuat, mengelola, dan menggunakan ConfigMap pada resource Kubernetes seperti Pod.

#### Struktur YAML

```{.yaml}
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

Dalam YAML di atas, terdapat objek ConfigMap dengan nama example-configmap yang berisi 2 konfigurasi aplikasi dengan nama `app_title` dan `app_config`

Value pada ConfigMap bisa berupa single line seperti pada konfigurasi `app_title` ataupun multiline seperti pada `app_config`.

Penggunaan multiline umumnya digunakan untuk konfigurasi berupa file. Value dari multiline akan di mount ke dalam Pod dalam bentuk file. Salah satu contoh penggunaan config multiline adalah konfigurasi nginx yang dimount ke directory `/etc/nginx/sites-enabled/nama_file` pada sebuah Pod.
