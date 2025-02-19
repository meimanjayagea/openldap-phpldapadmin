
# Hi, Saya Meiman Jaya Gea 👋

# OpenLDAP + phpldapadmin
Persiapan dan Mengonfigurasi OpenLDAP menggunakan Docker/Docker desktop

Penggunaan : OpenLDAP + phpldapadmin (Docker / Docker Compose)

#### OpenLDAP
referensi : https://hub.docker.com/r/osixia/openldap

###### Base Image openldap :

```bash
osixia/openldap:latest
```

###### Port:

```bash
- "389:389"
- "636:636"
```

###### Volumes:

```bash
- ./data/certificates:/container/service/slapd/assets/certs
- ./data/slapd/database:/var/lib/ldap
- ./data/slapd/config:/etc/ldap/slapd.d
```

###### Environment:

```bash
- LDAP_ORGANISATION=<ORGANISATION>
- LDAP_DOMAIN=<DOMAIN>
- LDAP_ADMIN_USERNAME=<USER>
- LDAP_ADMIN_PASSWORD=<PASSWORD>
- LDAP_CONFIG_PASSWORD=<PASSWORD>
- "LDAP_BASE_DN=dc=<DOMAIN>,dc=<SUFFIX>"
- LDAP_TLS_CRT_FILENAME=server.crt
- LDAP_TLS_KEY_FILENAME=server.key
- LDAP_TLS_CA_CRT_FILENAME=<DOMAIN>.ca.crt
- LDAP_READONLY_USER=true
- LDAP_READONLY_USER_USERNAME=<USER>
- LDAP_READONLY_USER_PASSWORD=<PASSWORD>
```


#### phpLDAPadmin
referensi : https://hub.docker.com/r/osixia/phpldapadmin

###### Base Image phpLDAPadmin :

```bash
osixia/phpldapadmin:latest
```

###### Port:

```bash
- "80:80"
```

###### Environment:

```bash
- PHPLDAPADMIN_LDAP_HOSTS=openldap
- PHPLDAPADMIN_HTTPS=false
```


## Menjalankan Projek di lokal

Clone projek 

```bash
  https://github.com/meimanjayagea/openldap-phpldapadmin.git
```

pergi ke direktori projek

```bash
  cd openldap
```

jalankan docker compose

```bash
  $> docker-compose up -d atau $> docker-compose up
```
jika menggunakan Compose V2, docker versi 20.10 keatas

```bash
  $> docker compose up -d atau $> docker compose up
```

buka pada browser http://localhost:80/ atau sesuaikan dengan konfigurasi anda contoh seperti pada project ini yaitu : http://localhost:8088/

### openldap access / login

**hostname :** http://localhost:8088/

**username :** cn=admin,dc=meiman,dc=com


![Logo]()

catatan :
username dan password di dapat dari konfigurasi pada docker-compose.yml

### Contoh konfigurasi file :
``` 
version: '3.7'
services:
  openldap:
    image: osixia/openldap:latest
    container_name: openldap
    hostname: openldap
    ports: 
      - "389:389"
      - "636:636"
    volumes:
      - ./data/certificates:/container/service/slapd/assets/certs
      - ./data/slapd/database:/var/lib/ldap
      - ./data/slapd/config:/etc/ldap/slapd.d
    environment: 
      - LDAP_ORGANISATION=meiman
      - LDAP_DOMAIN=meiman.com
      - LDAP_ADMIN_USERNAME=admin
      - LDAP_ADMIN_PASSWORD=admin_pass
      - LDAP_CONFIG_PASSWORD=config_pass
      - "LDAP_BASE_DN=dc=meiman,dc=com"
      - LDAP_TLS_CRT_FILENAME=server.crt
      - LDAP_TLS_KEY_FILENAME=server.key
      - LDAP_TLS_CA_CRT_FILENAME=meiman.com.ca.crt
      - LDAP_READONLY_USER=true
      - LDAP_READONLY_USER_USERNAME=user-ro
      - LDAP_READONLY_USER_PASSWORD=ro_pass
    networks:
      - openldap
  
  phpldapadmin:
    image: osixia/phpldapadmin:latest
    container_name: phpldapadmin
    hostname: phpldapadmin
    ports: 
      - "8088:80"
    environment: 
      - PHPLDAPADMIN_LDAP_HOSTS=openldap
      - PHPLDAPADMIN_HTTPS=false
    depends_on:
      - openldap
    networks:
      - openldap

networks:
  openldap:
    driver: bridge
```
