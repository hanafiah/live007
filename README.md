# Tutorial
### Cara set SSL ( https ) percuma gunakan letsencrypt, certbot & nginx

Tutorial ini akan menggunakan  struktur dari docker-webstack https://github.com/hanafiah/docker-webstack sebagai asas stack.

fail init-letsencrypt.sh di ambil dari https://github.com/wmnnd/nginx-certbot

## Langkah 1
clone repo ini
```
git clone https://github.com/hanafiah/live007.git
```
## Langkah 2
configure fail nginx.conf di folder projects anda. contoh disini `live007/nginx.conf`

tambah
```
        ssl_certificate /etc/letsencrypt/live/live007.katsini.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/live007.katsini.com/privkey.pem;

        include /etc/letsencrypt/options-ssl-nginx.conf;
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

        location ^~ /.well-known/acme-challenge/ {
            # Set correct content type. According to this:
            # https://community.letsencrypt.org/t/using-the-webroot-domain-verification-method/1445/29
            # Current specification requires "text/plain" or no content header at all.
            # It seems that "text/plain" is a safe option.
            default_type "text/plain";

            root /var/www/certbot;
        }
```

`live007.katsini.com` adalah nama domain .

## Langkah 3
Configure fail init-letsencrypt.sh

- masukkan nama domain 
``` 
domains=(example.com www.example.com)
```

- masukkan email 
``` 
email="email@example.com"
```

- set staging status. letak nilai = `1` jika hendak test configuration. 
``` 
staging=0 # Set to 1 if you're testing your setup to avoid hitting request limits
```

## Langkah 4
chmod execute & run fail init-letsencrypt.sh
```
chmod +x ./init-letsencrypt.sh
```
```
./init-letsencrypt.sh
```

## Langkah 5
jika pada langkah3 set staging=1, dan semua ok.. ( takda error ). edit init-letsencrypt.sh dan set staging=0 dan run balik Langkah 4
