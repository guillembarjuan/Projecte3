# 🧠 T01: Gestor de contrasenyes

## 🧩 Fase 1: Anàlisi i Justificació (Document d'Informe)

### 🔐 Introducció i Justificació

**Per què les contrasenyes febles o reutilitzades són un risc crític?**

Les contrassenyes febles o reutilitzades són un risc perquè permeten en els ciberdelinqüents entrar en els nostres sistemes d’una manera molt més senzilla.  
Hi ha dues situacions que la gent fa molt a l’hora de crear contrasenyes:

1. Les **contrasenyes dèbils** (com "123456" o noms propis) poden ser descobertes fàcilment mitjançant **atacs de diccionari**, on es proven automàticament milers de combinacions comunes en pocs segons.  
2. La **reutilització de contrasenyes** fa que si un servei extern és piratejat, els delinqüents puguin utilitzar les mateixes credencials per accedir als nostres sistemes interns, un mètode conegut com **"credential stuffing"**.

---

### 🧾 Comparativa Tècnica: Taula comparativa

| **Aspecte** | **Bitwarden (Online / Cloud)** | **KeePassXC (Offline / Local)** |
|--------------|--------------------------------|----------------------------------|
| **Tipus d’emmagatzematge** | Núvol xifrat amb sincronització automàtica | Fitxer local `.kdbx` xifrat |
| **Xifratge intern** | AES-256 + RSA-2048 + PBKDF2 | AES-256 + ChaCha20 + Argon2 |
| **Arquitectura de seguretat** | Model de coneixement zero (E2E): només l’usuari pot desxifrar les dades | Tot es guarda localment, sota una clau mestra |
| **Compatibilitat multiplataforma** | Windows, macOS, Linux, Android, iOS, navegadors (Edge, Chrome, Firefox, Safari) | Windows, macOS, Linux (ports manuals per Android/iOS) |
| **Interfície d’usuari (UX)** | Molt intuïtiva i moderna; orientada a la simplicitat i productivitat | Interfície més tècnica i datada, sense assistents gràfics |
| **Autocompletat de formularis** | Funció avançada per a logins i dades de targetes | Auto Type configurable manualment (requereix configuració extra) |
| **Autenticació multifactor (2FA)** | Inclosa, compatible amb apps OTP, YubiKey i WebAuthn | Només OTP bàsic via plugins externs |
| **Codi font** | 100% open source | 100% open source |
| **Model de cost** | Gratuït (opcional Premium 10 $/any) | Sempre gratuït |
| **Copy/Backup** | Sincronització automàtica i còpies xifrades | Còpia manual del fitxer `.kdbx` |
| **Control i dependència de tercers** | Depèn del servidor Bitwarden o de l’autohospedatge | Total control local, sense connexió externa |

---

### ⚖️ Avantatges i Inconvenients

#### 💻 Bitwarden (Online)

**Avantatges:**
- Més compatibilitat multiplataforma (inclou mòbil i navegador).  
- Interfície molt fàcil d’utilitzar.  
- Suport per 2FA avançat i E2E.  
- Possibilitat d’autohospedatge per entorns corporatius.  
- Sincronització automàtica entre dispositius.  

**Inconvenients:**
- Dependència d’Internet per accedir-hi.  
- Possible preocupació si el servidor de Bitwarden fos compromès (tot i l’E2E).  
- Algunes funcions premium són de pagament.  

---

#### 🧮 KeePassXC (Offline)

**Avantatges:**
- Control total local; les dades mai surten del dispositiu.  
- Totalment gratuït i open source.  
- Alt nivell de seguretat (AES + ChaCha20).  
- No depèn de serveis externs ni núvols.  

**Inconvenients:**
- Interfície més tècnica, menys amigable.  
- Sense sincronització automàtica (requereix Dropbox, USB o altres mitjans manuals).  
- Menys integració amb navegadors o mòbils.  

---

## 🧠 Recomanació final

Després d’analitzar les alternatives, la recomanació per a **EverPia** és **adoptar Bitwarden com a gestor corporatiu de contrasenyes**.

---

### ⚙️ Justificació tècnica

#### 🔄 Màxima eficiència i accessibilitat  
Bitwarden permet treballar de forma sincronitzada tant des de l’oficina com fora d’ella, sense preocupar-se per còpies manuals.

#### 🔒 Seguretat empresarial  
Utilitza **xifratge end-to-end amb coneixement zero**, afegint protecció addicional mitjançant **autenticació multifactor (2FA)**.

#### 🤝 Entorn col·laboratiu segur  
El **pla Teams/Enterprise** facilita la compartició controlada de credencials entre equips, essencial per a treballs en projectes comuns.

#### 🧩 Transparència i confiança  
És de **codi obert** i està constantment **auditada**, garantint confiança per a entorns tècnics i corporatius.

#### 💡 Usabilitat i adopció ràpida  
La seva interfície senzilla permet que qualsevol empleat, tècnic o administrador la domini ràpidament sense formació complexa.

---

### 🧾 Conclusió

En conjunt, **Bitwarden** ofereix un **bon equilibri entre seguretat, rendiment i facilitat d’adopció**, fet que el converteix en **l’opció més adequada per a la seguretat digital d’EverPia**.

---

[Guia de la tasca](guia.md)

---

[Tornar a la pàgina principal](../)
