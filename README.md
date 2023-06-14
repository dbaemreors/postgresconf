Bu kısma kadar bir linux makineye postgresql kurup diskleri bağladınığınızı varsayarak postgres üzerinde yapılacak konfigüsrasyon işlemleri anlatılmaktadır. Önceki aşamalar için bu iki nota bakabilirsiniz:
```
lvm ile disk mount işlemleri
https://dust-battery-476.notion.site/lvm-disk-mount-i-lemi-2a10e15ecf204e97876049bfd9fdae74?pvs=4

yum rpm ile postgres kurulumu
https://dust-battery-476.notion.site/oracle-linux-7-9-postgresql-13-install-e86dec9934204eecabc9213aa12aefe2?pvs=4
```

konfigürasyon yapacağımız yerler

```
postgresql.conf     (tüm parametreler burda)
pg_hba.conf     (bağlantı izni için ayarlar)
hugepage - tuned
```
Bazı parametrelerin açıklandığı örnek postgresql.conf dosyası repoda mevcuttur.

pg_hba.conf dosyasında aşağıdaki gibi ayarlama yapılır.
```
host    all             all             0.0.0.0/0               scram-sha-256
```
bu ayar ile tüm dış makinelerden bağlantı kabul eder.

![image](https://github.com/dbaemreors/postgresconf/assets/132146256/14f4cbc8-1eeb-4fa6-b99e-5df71f674e2d)

Yukarıdaki gibi user ve ip adresi vererek sadece o ipden ve kullanıcıdan erişim verebiliriz.

bağlantı için ayrıca yapılması gerekenler:

tüm bu ayarlar yapılsa dahi dışarıdan bağlantı sağlanamaz. os bazında port izni gereklidir. ve postgres kullanıcısı ile bağlanacaksak eğer psql ile parolası güncellenmelidir.

root kullanıcısı ile port izni verilir.
```
firewall-cmd --zone=public --add-port=5432/tcp --permanent
firewall-cmd --reload
```
postgres kullanıcısı güncellenir.
```
ALTER USER postgres PASSWORD 'password'
```
son ayar huge page ayarı. ilk önce huge page adlı makaleye göz atınız. bu makalede hugepage ayarı değil linux tarafında yapılacak ayarlamalar yer almaktadır.
```
https://dust-battery-476.notion.site/fad7e5f3fd3c44c0b02c0867a9da5e92?v=9d560ae5faef4978838de8bac4cc7bb8
```
bu ayarda bittikten sonra postgresql başlatılır. servis aktif hale gelir.
