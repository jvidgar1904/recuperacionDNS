# DNS: Maestro y esclavo con subdominios

Se usa una configuración de servidores DNS maestro y esclavo para el dominio `ies.test` y sus subdominios utilizando máquinas virtuales creadas con Vagrant. Las máquinas (`DNSA` y `DNSB`) manejan los subdominios en la red `192.168.57.0/24`.

---

## Estructura del Proyecto

```bash
recuperacionDNS
│   .gitignore
│   pruebas.md
│   README.md
│   Vagrantfile
│
└───files
    ├───DNSA
    │   │   named.conf.local
    │   │   
    │   └───zones
    │           db.192.168.57
    │           db.aulas.ies.test
    │           db.departamentos.ies.test
    │           db.ies.test
    │           db.informatica.ies.test
    │
    └───DNSB
            named.conf.local
```

---

## Descripción General

Ambos servidores resuelven nombres de dominio y direcciones IP tanto directa como inversamente para el dominio `ies.test` y sus subdominios `informatica`, `aulas` y `departamentos`. Los subdominios y las IPs asociadas se definen en el archivo de configuración de cada zona.

### Servidor `DNSA` (Maestro)

- **Dominio principal (`ies.test`)**: Configurado para la resolución directa e inversa.

- **Zonas maestras**: Configuradas en `/etc/bind/named.conf.local` y hacen referencia a la configuración específica de cada zona en los **_Archivos de zona_**
    - `informatica.ies.test`
    - `aulas.ies.test`
    - `departamentos.ies.test`
    - `57.168.192.in-addr.arpa`

- **Archivos de zona**: Creados en `/etc/bind/zones/`
    - `db.ies.test`: Dominio principal.
    - `db.informatica.ies.test`, `db.aulas.ies.test` y `db.departamentos.ies.test`: Subdominios respectivos.
    - `db.192.168.57`: Zona inversa

### Servidor `DNSB` (Esclavo)

- Obtiene la configuración de las zonas `infotmatica`, `aulas` y `departamentos` desde `DNSA` y las configura en `etc/bind/named.conf.local`.


## Configuración y Ejecución

1. **Instalación de Bind**: El archivo VagrantFile instala automáticamente `bind9` y sus utilidades a ambos servidores.

2. **Copiar Configuración**: Los archivos de configuración guardados en los directorios `files/DNSA` y `files/DNSB` se copian mediante la provisión de cada máquina a su directorio correspondiente `/etc/bind/` en la máquina indicada.

3. **Pruebas de Funcionamiento**: 
    - Utilizando comandos de `dig`, `nslookup`, y `host` se ha verificado el funcionamiento de las resoluciones directas e inversas tanto en `DNSA` como en `DNSB`.
    - Estas pruebas se encuentran documentadas en el documento `pruebas.md`.

---

Autor: [Javier Vidal García](https://github.com/jvidgar1904/)
Email: <jvidgar1904@ieszaidinvergeles.org>