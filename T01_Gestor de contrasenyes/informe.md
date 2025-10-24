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

### 🧠 Recomanació

Tenint en compte el projecte d’**EverPia** com a empresa tecnològica que gestiona informació important, la millor opció és **KeePassXC** com a gestor de contrasenyes.

---

### ⚙️ Motius tècnics de la decisió

- **Seguretat:** L’arxiu es guarda xifrat localment, fora del núvol, eliminant riscos en la sincronització online.  
- **Control:** El personal tècnic pot definir les seves pròpies polítiques de còpia i accés sense dependre d’un servei extern.  
- **Cost zero:** No hi ha dependència de subscripcions o funcionalitats premium.  
- **Adequat per entorns tècnics:** Tot i que requereix una configuració inicial més experta, això no és un problema per a un equip tècnic com el d’EverPia.

---

[Guia de la tasca](guia.md)

---

[Tornar a la pàgina principal](../)
