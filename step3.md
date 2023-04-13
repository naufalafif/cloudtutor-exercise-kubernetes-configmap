#### Mengubah ConfigMap

Untuk mengubah ConfigMap, cukup ubah value dari file YAML lalu jalankan perintah `apply`

```{.bash}
kubectl apply -f configmap.yml
```

**Perhatian**: Perubahan pada ConfigMap tidak otomatis mengubah config pada Pod, diperlukan restart untuk bisa menerima update.

Restart pada Pod dapat dilakukan dengan mengubah isi dari file YAML Pod lalu meng-apply perubahan tersebut dengan perintah

```
kubectl apply -f pod.yml
```

#### Menghapus ConfigMap

Menghapus ConfigMap dapat mengakibatkan error pada resource yang menggunakannya, seperti Pod ataupun Deployment. Pastikan semua resource tadi tidak menggunakan ConfigMap yang akan dihapus.

Pertama delete pod yang menggunakan ConfigMap `example-configmap`

```{.bash .copy}
kubectl delete --all pods
```

Selanjutnya hapus ConfigMap dengan menjalankan perintah berikut:

```{.bash .copy}
kubectl delete configmap example-configmap
```

#### Membuat ConfigMap (Kubectl)

Selain menggunakan YAML, kita juga bisa membuat ConfigMap menggunakan `Kubectl create`.

Untuk membuat ConfigMap baru dengan kubectl create, gunakan perintah dengan format seperti dibawah:

```{.bash}
kubectl create configmap <nama> <sumber-data>
```

`<nama>` adalah nama ConfigMap. <sumber-data> menunjukkan file atau value dari mana data ConfigMap diperoleh.

Kita dapat membuat ConfigMap berdasarkan satu file, beberapa file, direktori, atau env-files. Nama dasar setiap file digunakan sebagai key, dan isi file menjadi value.

| Sumber Data    | Example Command                                                                                                                                    |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Single file    | `kubectl create configmap config-single --from-file=app-container/settings/app.properties`                                                         |
| Multiple files | `kubectl create configmap config-multiple --from-file=app-container/settings/app.properties --from-file=app-container/settings/backend.properties` |
| Env File       | `kubectl create configmap config-env --from-env-file=app-container/settings/app-env-file.properties`                                               |
| Directory      | `kubectl create configmap config-dir --from-file=app-container/settings/`                                                                          |
