# Tasca 04: Serveis de directori. LDAP

En aquesta guia aprendrem a instal·lar i configurar un servidor LDAP amb OpenLDAP per gestionar usuaris i grups d'una organització.

## 1. Configuració inicial del servidor

### Verificació de la configuració de xarxa

Abans de començar, és important verificar la configuració de xarxa de la nostra màquina virtual.

![Configuració de xarxa principal](/tasca04/img_t04/captura01.png)

**Anàlisi**: El servidor té l'Adaptador 1 connectat a una xarxa NAT, que permet la connexió a Internet per descarregar paquets. L'adaptador està configurat com a Intel PRO/1000 MT Desktop i té l'adreça MAC 080027049FF6.

![Configuració de xarxa addicional](/tasca04/img_t04/captura02.png)

**Anàlisi**: Aquesta captura mostra una configuració de xarxa addicional amb un adaptador en mode "només l'amfitrió" (Host-Only). Aquest tipus de configuració s'utilitza per crear una xarxa privada entre la màquina virtual i l'amfitrió.

### Configuració del nom del servidor i domini

Primer, configurem el nom del servidor i el domini:

```bash
sudo hostnamectl set-hostname server
hostname
hostname -f
```

![Configuració del nom del servidor](/tasca04/img_t04/captura05.png)

**Anàlisi**: Amb la comanda `hostnamectl` canviem el nom del servidor a "server". Després verifiquem que els canvis s'han aplicat correctament amb `hostname` i `hostname -f`. Veiem que el nom complet del servidor ara és `server.innovatech01.test`.

### Configuració del fitxer /etc/hosts

```bash
sudo nano /etc/hosts
```

![Configuració del fitxer hosts](/tasca04/img_t04/captura04.png)

**Anàlisi**: Configurem el fitxer `/etc/hosts` per assegurar-nos que el servidor es pugui resoldre correctament pel seu nom de domini. S'ha definit que `server.innovatech01.test` apunti a l'adreça local 127.0.1.1.

## 2. Instal·lació del servei LDAP

### Actualització del sistema

Abans d'instal·lar qualsevol servei, sempre actualitzem el sistema:

```bash
sudo apt update && sudo apt upgrade -y
```

![Actualització del sistema](/tasca04/img_t04/captura03.png)

**Anàlisi**: La comanda `apt update` actualitza la llista de paquets disponibles i `apt upgrade -y` actualitza tots els paquets instal·lats. El paràmetre `-y` confirma automàticament la instal·lació. Veiem que tots els paquets ja estaven actualitzats.

### Instal·lació d'OpenLDAP

Ara instal·lem el servidor LDAP i les utilitats:

```bash
sudo apt install slapd ldap-utils -y
```

![Instal·lació d'OpenLDAP](/tasca04/img_t04/captura06.png)

**Anàlisi**: Estem instal·lant dos paquets essencials:
- **slapd**: El servidor LDAP pròpiament dit (Standalone LDAP Daemon)
- **ldap-utils**: Les utilitats de línia de comanda per gestionar LDAP

Durant la instal·lació, ens demanarà configurar algunes opcions:

#### Contrasenya d'administrador

![Configuració de la contrasenya](/tasca04/img_t04/captura07.png)

**Anàlisi**: Estem configurant la contrasenya per a l'usuari administrador de LDAP. És molt important triar una contrasenya segura i recordar-la, ja que serà necessària per administrar el servidor.

#### Procés d'instal·lació

![Procés d'instal·lació](/tasca04/img_t04/captura08.png)

**Anàlisi**: El procés d'instal·lació mostra com es van instal·lant els diferents paquets i les seves dependències. Podem veure que:
- S'instal·len les llibreries necessàries (`libltdl7`, `libodbc2`)
- S'instal·len els paquets principals (`slapd`, `ldap-utils`)
- Es crea l'usuari `openldap`
- Es crea la configuració inicial del directori LDAP

## 3. Configuració del servidor LDAP

### Verificació del servei

Després de la instal·lació, verifiquem que el servei està funcionant:

```bash
sudo systemctl status slapd
```

![Estat del servei slapd](/tasca04/img_t04/captura10.png)

**Anàlisi**: El servei `slapd` està actiu i funcionant correctament. La informació mostra:
- **Estat**: "active (running)" des de fa 1 minut i 29 segons
- **PID**: El procés s'està executant amb ID 6664
- **Configuració**: S'està executant amb l'usuari `openldap` i escoltant en `ldap:///ldapi:///`

### Reconfiguració del servei

Si volem canviar alguna configuració inicial, podem reconfigurar el servei:

```bash
sudo dpkg-reconfigure slapd
```

![Inici de la reconfiguració](/tasca04/img_t04/captura11.png)

**Anàlisi**: La comanda `dpkg-reconfigure` ens permet modificar la configuració d'un paquet ja instal·lat. En aquest cas, reconfigurarem el servidor LDAP.

#### Configuració del domini DNS

![Configuració del domini DNS](/tasca04/img_t04/captura12.png)

**Anàlisi**: Introduïm el nom de domini DNS que s'utilitzarà per construir el DN (Distinguished Name) base del directori LDAP. En el nostre cas és `innovatech01.test`. Això crearà un DN base de `dc=innovatech01,dc=test`.

#### Configuració del nom de l'organització

![Nom de l'organització](/tasca04/img_t04/captura13.png)

**Anàlisi**: Establim el nom de l'organització que s'utilitzarà al DN base. En aquest cas és "innovatech". Aquest nom apareixerà a la configuració del directori LDAP.

#### Configuració de la base de dades

![Configuració de la base de dades](/tasca04/img_t04/captura14.png)

**Anàlisi**: Ens pregunta si volem esborrar la base de dades quan es purgui el paquet `slapd`. És recomanable triar "No" per conservar les dades en cas que haguem de reinstal·lar el paquet.

#### Gestió de la base de dades antiga

![Gestió de la base de dades antiga](/tasca04/img_t04/captura15.png)

**Anàlisi**: Ens informa que hi ha fitxers existents a `/var/lib/ldap` que podrien interferir amb la nova configuració. Si triem "Sí", es farà una còpia de seguretat de la base de dades antiga abans de crear una nova.

#### Finalització de la reconfiguració

![Finalització de la reconfiguració](/tasca04/img_t04/captura16.png)

**Anàlisi**: El procés de reconfiguració s'ha completat. Es fa una còpia de seguretat de la configuració anterior i es crea una nova configuració i directori LDAP.

## 4. Verificació de la configuració

### Comprovació del contingut LDAP

```bash
sudo slapcat
```

![Verificació amb slapcat](/tasca04/img_t04/captura17.png)

**Anàlisi**: La comanda `slapcat` mostra tot el contingut de la base de dades LDAP en format LDIF. Veiem l'entrada base amb:
- **DN**: `dc=innovatech01,dc=test`
- **Classes d'objecte**: `top`, `dcObject`, `organization`
- **Atributs**: `o` (organització) amb valor "innovatech" i `dc` (domain component) amb valor "innovatech01"

## 5. Creació d'Unitats Organitzatives (OUs)

### Creació del fitxer LDIF

Ara crearem les Unitats Organitzatives (OUs) per organitzar els usuaris i grups:

```bash
sudo nano OU_users.ldif
```

![Creació del fitxer LDIF](/tasca04/img_t04/captura18.png)

**Anàlisi**: Creem un fitxer amb extensió `.ldif` (LDAP Data Interchange Format) que contindrà les definicions de les Unitats Organitzatives.

### Contingut del fitxer LDIF

![Contingut del fitxer LDIF](/tasca04/img_t04/captura19.png)

**Anàlisi**: El fitxer conté dues entrades LDIF:

1. **OU pels usuaris**:
   - `dn: ou=users,dc=innovatech01,dc=test` - El nom distintiu de la unitat organitzativa
   - `ou: users` - El nom de la unitat organitzativa
   - `objectClass: top` i `objectClass: organizationalUnit` - Les classes que defineixen el tipus d'objecte

2. **OU pels grups**:
   - `dn: ou=groups,dc=innovatech01,dc=test` - El nom distintiu de la unitat organitzativa
   - `ou: groups` - El nom de la unitat organitzativa
   - Les mateixes classes d'objecte que la OU anterior

### Afegir les OUs al directori LDAP

```bash
ldapadd -D "cn=admin,dc=innovatech01,dc=test" -W -f OU_users.ldif
```

![Afegint les OUs al directori](/tasca04/img_t04/captura20.png)

**Anàlisi**: Utilitzem la comanda `ldapadd` per afegir les noves OUs al directori LDAP:

- `-D "cn=admin,dc=innovatech01,dc=test"`: Especifica el DN de l'administrador per autenticar-se
- `-W`: Demana la contrasenya de forma interactiva (més segur que posar-la a la línia de comandes)
- `-f OU_users.ldif`: Indica el fitxer LDIF que conté les entrades a afegir

La sortida confirma que les dues entrades s'han afegit correctament al directori:
- `adding new entry "ou=users,dc=innovatech01,dc=test"`
- `adding new entry "ou=groups,dc=innovatech01,dc=test"`

---

## 6. Verificació de l'estructura LDAP

Abans de continuar, verifiquem que les OUs que vam crear estan correctament configurades al directori LDAP:

```bash
sudo slapcat
```

![Verificació completa del directori LDAP](/tasca04/img_t04/captura21.png)

**Anàlisi**: La comanda `slapcat` mostra ara totes les entrades del nostre directori LDAP. Podem veure:
- **Entrada base**: `dc=innovatech01,dc=test` amb la informació de l'organització
- **OU users**: `ou=users,dc=innovatech01,dc=test` creada correctament
- **OU groups**: `ou=groups,dc=innovatech01,dc=test` també creada correctament

Cada entrada mostra informació detallada com els timestamps de creació i modificació, l'UUID de l'entrada i qui la va crear.

## 7. Instal·lació de LDAP Account Manager (LAM)

Ara instal·larem una interfície gràfica per gestionar el nostre servidor LDAP de manera més senzilla:

```bash
sudo apt install ldap-account-manager -y
```

![Instal·lació de LAM](/tasca04/img_t04/captura22.png)

**Anàlisi**: Estem instal·lant el paquet `ldap-account-manager` que inclou:
- **Apache2**: El servidor web que hostatjarà LAM
- **PHP i extensions**: Necessaris per executar l'aplicació web
- **Llibreries LDAP**: Per connectar-se al servidor LDAP
- **Altres dependències**: Com fonts, llibreries JavaScript, etc.

La instal·lació descarrega 32.3 MB i utilitzarà 126 MB d'espai addicional.

### Finalització de la instal·lació

![Finalització de la instal·lació LAM](/tasca04/img_t04/captura23.png)

**Anàlisi**: La instal·lació s'ha completat sense errors. No cal reiniciar cap servei ni contenidor, i no hi ha sessions d'usuari executant binaris desactualitzats.

### Verificació del servei Apache

```bash
sudo systemctl status apache2
```

![Estat del servei Apache](/tasca04/img_t04/captura24.png)

**Anàlisi**: El servei Apache2 està actiu i funcionant correctament:
- **Estat**: "active (running)" des de fa 1 minut i 58 segons
- **PID**: S'està executant amb múltiples processos fills
- **Memòria**: Utilitzant 13.6 MB de memòria

## 8. Accés a LDAP Account Manager

### Obtenció de la IP del servidor

![IP del servidor](/tasca04/img_t04/captura25.png)

**Anàlisi**: La IP del nostre servidor és `192.168.56.101`. Aquesta és la IP que utilitzarem per accedir a la interfície web de LAM.

### Pantalla d'inici de LAM

![Pantalla d'inici de LAM](/tasca04/img_t04/captura26.png)

**Anàlisi**: Ara accedim a LAM través del navegador web a `http://192.168.56.101/lam`. Veiem la pantalla d'inici de sessió amb:
- **Camps**: Usuari, contrasenya i idioma
- **Servidor LDAP**: `ldap://localhost:389` (el servidor local)
- **Perfil del servidor**: Per defecte "lam"

### Configuració del perfil del servidor

![Edició de la configuració general](/tasca04/img_t04/captura27.png)

**Anàlisi**: Abans d'iniciar sessió, hem d'editar el perfil del servidor per configurar-lo amb el nostre domini LDAP.

### Autenticació per canviar preferències

![Contrasenya del perfil](/tasca04/img_t04/captura28.png)

**Anàlisi**: Ens demana la contrasenya del perfil "lam". La contrasenya per defecte és "lam".

### Configuració del servidor LDAP

![Preferències del servidor](/tasca04/img_t04/captura29.png)

**Anàlisi**: Configurem les preferències del servidor:
- **Adreça del servidor**: `ldap://localhost:389`
- **Llista d'usuaris vàlids**: `cn=admin,dc=innovatech01,dc=test` (el nostre administrador LDAP)

### Configuració d'idioma i zona horària

![Configuració d'idioma](/tasca04/img_t04/captura30.png)

**Anàlisi**: Establim l'idioma per defecte a "Español (España)" i la zona horària a "Europe/Madrid".

### Configuració d'eines

![Configuració d'eines](/tasca04/img_t04/captura31.png)

**Anàlisi**: Aquí podem configurar quines eines volem mostrar a la interfície. Deixem les opcions per defecte.

### Configuració de seguretat i lamdaemon

![Configuració de seguretat](/tasca04/img_t04/captura32.png)

**Anàlisi**: Configurem les opcions de seguretat com les polítiques de contrasenyes i l'autenticació de dos factors.

### Configuració dels tipus de comptes

![Tipus de comptes actius](/tasca04/img_t04/captura33.png)

**Anàlisi**: És una part important de la configuració. Aquí definim:
- **Usuaris**: Amb sufix `ou=users,dc=innovatech01,dc=test`
- **Grups**: Amb sufix `ou=groups,dc=innovatech01,dc=test`
Això assegura que LAM sàpiga on crear els nous usuaris i grups.

### Guardant la configuració

![Guardar configuració](/tasca04/img_t04/captura34.png)

![Configuració guardada correctament](/tasca04/img_t04/captura35.png)

**Anàlisi**: Guardem la configuració i rebem la confirmació que s'ha guardat correctament.

## 9. Inici de sessió a LAM

### Pantalla d'inici de sessió

![Inici de sessió a LAM](/tasca04/img_t04/captura36.png)

**Anàlisi**: Ara tornem a la pantalla d'inici de sessió. Introduïm:
- **Usuari**: `admin`
- **Contrasenya**: La contrasenya d'administrador de LDAP que vam configurar anteriorment
- **Idioma**: Español (España)

### Creació dels sufijos LDAP

![Creació de sufijos LDAP](/tasca04/img_t04/captura37.png)

**Anàlisi**: LAM detecta que els sufijos que hem configurat (`ou=users` i `ou=groups`) no existeixen al directori LDAP i ens ofereix crear-los automàticament. Acceptem per crear-les.

### Confirmació dels canvis

![Canvis exitosos](/tasca04/img_t04/captura38.png)

**Anàlisi**: Tots els canvis s'han aplicat correctament. Les OUs s'han creat al directori LDAP.

### Pantalla principal de LAM

![Llista d'usuaris buida](/tasca04/img_t04/captura39.png)

**Anàlisi**: Ara estem a la pantalla principal de LAM. Veiem que:
- El compte d'usuaris està buit (0 usuaris)
- Tenim opcions per crear nous usuaris
- La interfície està en espanyol

### Navegació per les seccions

![Navegació per usuaris i grups](/tasca04/img_t04/captura40.png)

**Anàlisi**: A la part esquerra veiem el menú de navegació amb les seccions principals:
- **Usuaris**: Per gestionar comptes d'usuari
- **Grups**: Per gestionar grups

---

## 10. Creació de grups al directori LDAP

Ara crearem els grups que necessitem per organitzar els usuaris.

### Creació del grup "tech"

![Creació del grup tech](/tasca04/img_t04/captura41.png)

**Anàlisi**: Estem creant el primer grup "tech" amb les següents característiques:
- **Sufix**: `ou=groups,dc=innovatech01,dc=test`
- **Nom del grup**: `tech`
- **Número GID**: Es generarà automàticament

### Confirmació de la creació del grup

![Grup creat exitosament](/tasca04/img_t04/captura42.png)

**Anàlisi**: El grup "tech" s'ha creat correctament al directori LDAP. LAM ens mostra un missatge de confirmació i ens ofereix opcions per tornar a la llista de grups o crear-ne un de nou.

### Creació del grup "manager"

![Creació del grup manager](/tasca04/img_t04/captura43.png)

**Anàlisi**: Ara creem el segon grup "manager" amb la mateixa estructura. Aquests grups ens serviran per organitzar els diferents tipus d'usuaris.

### Llista de grups creats

![Llista de grups](/tasca04/img_t04/captura44.png)

**Anàlisi**: Ara veiem la llista completa dels grups creats:
- **tech**: Amb GID 10000
- **manager**: Amb GID 10001

Cada grup té el seu propi GID (Group ID) que l'identifica de manera única al sistema.

### Verificació des de la línia de comandes

```bash
sudo ldapsearch -x -LLL -b "ou=groups,dc=innovatech01,dc=test" "objectClass=posixGroup" cn gidNumber
```

![Verificació dels grups via LDAP](/tasca04/img_t04/captura45.png)

**Anàlisi**: Fem una consulta LDAP des de terminal per verificar que els grups s'han creat correctament al directori. La comanda mostra:
- **tech**: cn=tech, gidNumber=10000
- **manager**: cn=manager, gidNumber=10001

Això confirma que els grups estan ben configurats al servidor LDAP.

## 11. Creació d'usuaris al directori LDAP

Ara crearem els usuaris i els assignarem als grups corresponents.

### Accés a la secció d'usuaris

![Secció d'usuaris](/tasca04/img_t04/captura46.png)

![Llista d'usuaris buida](/tasca04/img_t04/captura47.png)

**Anàlisi**: Accedim a la secció d'usuaris on veiem que encara no hi ha cap usuari creat (compte d'usuaris: 0).

### Creació de l'usuari "tech01"

#### Informació personal

![Informació personal de tech01](/tasca04/img_t04/captura48.png)

**Anàlisi**: Omplim la informació personal de l'usuari tech01:
- **Nom**: Tech
- **Cognom**: User (obligatori)
- **Descripció**: Informació addicional sobre l'usuari

#### Configuració Unix

![Configuració Unix de tech01](/tasca04/img_t04/captura49.png)

**Anàlisi**: Configurem les opcions Unix de l'usuari:
- **Nom d'usuari**: `tech01`
- **Nom comú**: `tech01` 
- **Número UID**: `10000` (coincideix amb el GID del grup tech)
- **Grup primari**: `tech` (l'assignem al grup tech)
- **Directori home**: `/home/tech01`
- **Shell**: `/bin/bash`

#### Establiment de contrasenya

![Establiment de contrasenya](/tasca04/img_t04/captura50.png)

**Anàlisi**: Establim la contrasenya per a l'usuari tech01. És important triar una contrasenya segura.

### Llista amb el primer usuari creat

![Llista amb tech01](/tasca04/img_t04/captura51.png)

**Anàlisi**: Ara veiem que ja tenim 1 usuari creat (tech01) a la llista.

### Creació de l'usuari "manager01"

#### Informació personal

![Informació personal de manager01](/tasca04/img_t04/captura52.png)

**Anàlisi**: Omplim la informació personal de l'usuari manager01:
- **Nom**: Manager
- **Cognom**: User

#### Configuració Unix

![Configuració Unix de manager01](/tasca04/img_t04/captura53.png)

**Anàlisi**: Configurem les opcions Unix de manager01:
- **Nom d'usuari**: `manager01`
- **Nom comú**: `manager01`
- **Número UID**: `10001` (coincideix amb el GID del grup manager)
- **Grup primari**: `manager` (l'assignem al grup manager)
- **Directori home**: `/home/manager01`
- **Shell**: `/bin/bash`

#### Establiment de contrasenya

![Contrasenya per manager01](/tasca04/img_t04/captura54.png)

**Anàlisi**: Establim la contrasenya per a l'usuari manager01.

### Llista completa d'usuaris

![Llista completa d'usuaris](/tasca04/img_t04/captura55.png)

**Anàlisi**: Ara tenim els dos usuaris creats correctament:
- **tech01**: UID 10000, GID 10000, grup tech
- **manager01**: UID 10001, GID 10001, grup manager

Cada usuari està correctament assignat al seu grup corresponent.

## 12. Configuració del client Ubuntu

Ara configurarem una màquina client per connectar-se al servidor LDAP.

### Configuració de xarxa del client

![Configuració de xarxa del client](/tasca04/img_t04/captura56.png)

**Anàlisi**: El client està configurat amb:
- **Adaptador 1**: Xarxa NAT (per connectivitat)
- **Adreça MAC**: 08002794AA6C
Aquesta configuració permet que el client es comuniqui amb el servidor a través de la xarxa.

### Instal·lació del sistema client

![Instal·lació del client Ubuntu](/tasca04/img_t04/captura57.png)

**Anàlisi**: Durant la instal·lació del client Ubuntu configurem:
- **Nom de l'equip**: `client-VirtualBox`
- **Nom d'usuari**: `client`
- **Contrasenya**: `p@sswOrd`

### Verificació de la xarxa del client

```bash
ip a
```

![Configuració de xarxa del client](/tasca04/img_t04/captura58.png)

**Anàlisi**: El client té la següent configuració de xarxa:
- **Interface enp0s3**: IP `10.0.2.5/24`
- **Adreça MAC**: 08:00:27:94:aa:6c
Aquesta IP està a la mateixa xarxa que el servidor (10.0.2.0/24).

### Verificació de la xarxa del servidor

```bash
ip a
```

![Configuració de xarxa del servidor](/tasca04/img_t04/captura59.png)

**Anàlisi**: El servidor té dues interfícies de xarxa:
- **enp0s3**: IP `10.0.2.4/24` (xarxa NAT - mateixa que el client)
- **enp0s8**: IP `192.168.56.101/24` (xarxa host-only)

Això confirma que el client i el servidor poden comunicar-se a través de la xarxa 10.0.2.0/24.

---

## 13. Configuració del client per a autenticació LDAP

### Configuració del fitxer hosts del client

```bash
sudo nano /etc/hosts
```

![Edició del fitxer hosts del client](/tasca04/img_t04/captura60.png)

**Anàlisi**: Estem editant el fitxer `/etc/hosts` del client per afegir la resolució del servidor LDAP.

![Configuració del fitxer hosts](/tasca04/img_t04/captura61.png)

**Anàlisi**: Hem afegit la línia `10.0.2.4 server.innovatech01.test` per assegurar que el client pugui resoldre el nom del servidor LDAP correctament.

### Configuració del nom del client

```bash
sudo hostnamectl set-hostname client
hostname
hostname -f
```

![Configuració del nom del client](/tasca04/img_t04/captura62.png)

**Anàlisi**: Configurem el nom del client com a "client" i verifiquem que el nom complet és `client.innovatech01.test`.

### Verificació de connectivitat amb el servidor

```bash
ping server.innovatech01.test -c 4
```

![Test de ping al servidor](/tasca04/img_t04/captura63.png)

**Anàlisi**: Fem un test de connectivitat al servidor LDAP. El ping és exitós amb:
- **4 paquets transmesos i rebuts**: 0% de pèrdua
- **Temps de resposta**: Mitjana de 0.839 ms
Això confirma que el client pot arribar al servidor.

### Verificació amb dig

```bash
dig server.innovatech01.test
```

![Verificació DNS amb dig](/tasca04/img_t04/captura64.png)

**Anàlisi**: La comanda `dig` mostra que el nom `server.innovatech01.test` es resol correctament a la IP `192.168.56.101`.

### Actualització del sistema client

```bash
sudo apt update
```

![Actualització del client](/tasca04/img_t04/captura65.png)

**Anàlisi**: Actualitzem la llista de paquets del client abans d'instal·lar les dependències LDAP.

### Instal·lació dels paquets LDAP al client

```bash
sudo apt install libnss-ldap libpam-ldap ldap-utils nscd -y
```

![Instal·lació dels paquets LDAP](/tasca04/img_t04/captura66.png)

**Anàlisi**: Instal·lem els paquets necessaris per a la integració LDAP:
- **libnss-ldap**: Permet al Name Service Switch utilitzar LDAP
- **libpam-ldap**: Permet a PAM utilitzar LDAP per autenticació
- **ldap-utils**: Eines per gestionar LDAP des del client
- **nscd**: Name Service Cache Daemon (millora el rendiment)

### Configuració del client LDAP

#### URI del servidor LDAP

![Configuració de la URI LDAP](/tasca04/img_t04/captura67.png)

**Anàlisi**: Configurem la URI del servidor LDAP. Introduïm `ldap://server.innovatech01.test` per indicar on està el nostre servidor LDAP.

#### Base DN de cerca

![Configuració del Base DN](/tasca04/img_t04/captura68.png)

**Anàlisi**: Establim el Base DN (Distinguished Name) de cerca com a `dc=innovatech01,dc=test`. Aquest és el punt d'inici per les cerques al directori LDAP.

#### Versió del protocol LDAP

![Configuració de la versió LDAP](/tasca04/img_t04/captura69.png)

**Anàlisi**: Seleccionem la versió 3 del protocol LDAP, que és l'estàndard actual i ofereix més funcionalitats que les versions anteriors.

#### Configuració de la base de dades local

![Configuració de l'administrador local](/tasca04/img_t04/captura70.png)

**Anàlisi**: Triem "No" per no fer l'administrador de la base de dades local. Això manté la separació entre usuaris locals i usuaris LDAP.

#### Autenticació a la base de dades LDAP

![Configuració d'autenticació LDAP](/tasca04/img_t04/captura71.png)

**Anàlisi**: Triem "No" perquè la base de dades LDAP no requereix autenticació per fer cerques. Els usuaris poden autenticar-se sense credencials administratives.

#### Compte LDAP per a root

![Compte LDAP per a root](/tasca04/img_t04/captura72.png)

**Anàlisi**: Introduïm `cn=admin,dc=innovatech01,dc=test` com a compte administratiu per canvis de contrasenya.

#### Contrasenya del compte root LDAP

![Contrasenya del compte admin](/tasca04/img_t04/captura73.png)

**Anàlisi**: Introduïm la contrasenya de l'administrador LDAP. Aquesta contrasenya s'emmagatzemarà de forma segura a `/etc/ldap.secret`.

### Procés d'instal·lació completat

![Instal·lació completada](/tasca04/img_t04/captura74.png)

**Anàlisi**: La instal·lació s'ha completat correctament. S'han configurat tots els paquets i s'ha creat el servei nscd.

### Verificació de la connexió LDAP

```bash
ldapsearch -x -D 'cn=admin,dc=innovatech01,dc=test' -W -H ldap://server.innovatech01.test -b 'dc=innovatech01,dc=test' objectClass=posixAccount uid
```

![Verificació de la connexió LDAP](/tasca04/img_t04/captura75.png)

**Anàlisi**: Fem una consulta LDAP per verificar que el client es pot connectar al servidor i llegir les entrades. La consulta retorna èxit, confirmant que la connexió funciona correctament.

## 14. Configuració final del client

### Configuració de nsswitch.conf

```bash
sudo nano /etc/nsswitch.conf
```

![Edició de nsswitch.conf](/tasca04/img_t04/captura76.png)

![Configuració de nsswitch.conf](/tasca04/img_t04/captura77.png)

**Anàlisi**: Configurem el fitxer `/etc/nsswitch.conf` per incloure LDAP en la resolució d'usuaris i grups:
- **passwd**: `files systemd ldap` - Cerca usuaris primer als fitxers locals, després a systemd, i finalment a LDAP
- **group**: `files systemd ldap` - El mateix per als grups
- **shadow**: `files ldap` - Per a les contrasenyes

### Configuració de PAM

```bash
sudo nano /etc/pam.d/common-password
```

![Edició de common-password](/tasca04/img_t04/captura78.png)

![Configuració de common-password](/tasca04/img_t04/captura79.png)

**Anàlisi**: Verifiquem la configuració de PAM per a les contrasenyes. Veiem que ja inclou `pam_ldap.so` per gestionar les contrasenyes LDAP automàticament.

---

## 15. Configuració final de PAM i verificació

### Configuració de common-password (revisió)

![Configuració de common-password](/tasca04/img_t04/captura80.png)

**Anàlisi**: Revisem el fitxer `/etc/pam.d/common-password`. Veiem que la configuració està correcta i inclou els mòduls necessaris per a la gestió de contrasenyes amb PAM.

### Configuració de common-session

```bash
sudo nano /etc/pam.d/common-session
```

![Edició de common-session](/tasca04/img_t04/captura81.png)

![Configuració de common-session](/tasca04/img_t04/captura82.png)

**Anàlisi**: Configurem el fitxer `/etc/pam.d/common-session` i afegim la línia:
```bash
session optional    pam_mkhomedir.so skel=/etc/skel umask=077
```
Aquesta línia és crucial perquè crea automàticament el directori home dels usuaris LDAP quan inicien sessió per primera vegada.

### Reinici del servei nscd

```bash
sudo systemctl restart nscd.service
sudo systemctl status nscd.service
```

**Anàlisi**: Reiniciem el servei nscd (Name Service Cache Daemon) per aplicar els canvis de configuració. El servei està actiu i funcionant correctament.

### Verificació del contingut LDAP

```bash
ldapsearch -x -H ldap://server.innovatech01.test -b "dc=innovatech01,dc=test" -LLL
```

![Verificació del contingut LDAP](/tasca04/img_t04/captura84.png)

**Anàlisi**: Fem una consulta LDAP completa que mostra totes les entrades del nostre directori:
- **Entrada base**: `dc=innovatech01,dc=test`
- **OUs**: `users` i `groups` 
- **Grups**: `tech` (GID 10000) i `manager` (GID 10001)

### Verificació dels usuaris LDAP

![Usuaris LDAP](/tasca04/img_t04/captura85.png)

**Anàlisi**: La consulta LDAP també mostra els usuaris creats:
- **tech01**: UID 10000, GID 10000, shell `/bin/bash`, home `/home/tech01`
- **manager01**: UID 10001, GID 10001, shell `/bin/bash`, home `/home/manager01`

### Verificació amb getent

```bash
sudo getent passwd | tail
```

![Verificació amb getent](/tasca04/img_t04/captura86.png)

**Anàlisi**: La comanda `getent passwd` ara mostra els usuaris LDAP al final de la llista:
- **tech01**: `*:10000:10000:tech01:/home/tech01:/bin/bash`
- **manager01**: `*:10001:10001:manager01:/home/manager01:/bin/bash`

El símbol `*` indica que les contrasenyes es gestionen mitjançant LDAP.

### Configuració de GDM (opcional)

![Configuració de GDM](/tasca04/img_t04/captura87.png)

**Anàlisi**: El fitxer de configuració de GDM inclou `pam_ldap.so` per permetre l'autenticació LDAP en l'inici de sessió gràfic.

## 16. Prova d'autenticació LDAP

### Pantalla d'inici de sessió

![Pantalla d'inici de sessió](/tasca04/img_t04/captura88.png)

**Anàlisi**: A la pantalla d'inici de sessió de Zorin OS (basat en Ubuntu), veiem l'usuari local "client" i l'opció "No está en la lista" per introduir un usuari manualment.

### Selecció de l'usuari tech01

![Selecció de tech01](/tasca04/img_t04/captura89.png)

**Anàlisi**: Seleccionem l'usuari "tech01" que és un usuari LDAP.

### Inici de sessió amb tech01

![Inici de sessió tech01](/tasca04/img_t04/captura90.png)

**Anàlisi**: Introduïm la contrasenya de l'usuari tech01. El sistema autenticarà aquest usuari contra el servidor LDAP.

### Sessió iniciada correctament

![Sessió iniciada](/tasca04/img_t04/captura91.png)

**Anàlisi**: L'usuari tech01 ha iniciat sessió correctament. El sistema ha creat automàticament el seu directori home gràcies a `pam_mkhomedir.so`.

### Prova amb manager01

![Selecció de manager01](/tasca04/img_t04/captura92.png)

**Anàlisi**: Ara provem amb l'usuari "manager01" per verificar que tots els usuaris LDAP funcionen.

### Inici de sessió amb manager01

![Inici de sessió manager01](/tasca04/img_t04/captura93.png)

**Anàlisi**: Introduïm la contrasenya de manager01. Aquest usuari també s'autenticarà correctament contra el servidor LDAP.

---

## **RESUM FINAL DE LA TASCA 04**

### **Èxits aconseguits:**

1. **✅ Servidor LDAP configurat** amb OpenLDAP
2. **✅ Estructura organitzativa** creada amb OUs per usuaris i grups
3. **✅ Gestió gràfica** implementada amb LDAP Account Manager
4. **✅ Usuaris i grups** creats i organitzats correctament
5. **✅ Client configurat** per autenticació LDAP
6. **✅ Connectivitat** verificada entre client i servidor
7. **✅ Autenticació funcionant** amb usuaris LDAP

### **Elements clau del sistema:**

- **Domini LDAP**: `dc=innovatech01,dc=test`
- **Servidor**: `server.innovatech01.test` (10.0.2.4)
- **Client**: `client.innovatech01.test` (10.0.2.5)
- **Grups**: `tech` (GID 10000) i `manager` (GID 10001)
- **Usuaris**: `tech01` i `manager01` amb els seus grups corresponents

### **Verificació final:**

- Els usuaris LDAP apareixen a `getent passwd`
- Les consultes LDAP funcionen des del client
- Els usuaris poden iniciar sessió amb les seves credencials LDAP
- Els directoris home es creen automàticament
- La gestió centralitzada funciona a través de LAM

### **Conclusió:**

Hem implementat amb èxit un sistema de directori LDAP complet que permet la gestió centralitzada d'usuaris i grups. Els usuaris creats al servidor LDAP poden autenticar-se des del client Ubuntu, demostrant que la integració LDAP funciona correctament. Aquest sistema és escalable i pot gestionar múltiples clients i centenars d'usuaris de manera eficient.
