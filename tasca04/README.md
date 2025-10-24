# 🧩 Projecte T04 — Serveis de Directori: LDAP

## 💼 Descripció del projecte

Treballo per **EverPia**, una consultora tecnològica que ha rebut l’encàrrec d’ajudar a **Innovatech**, una start-up en ple creixement que està patint un **autèntic caos en la gestió dels seus usuaris i accessos**.

Actualment, cada servei intern (servidor de fitxers, wiki de documentació, etc.) utilitza la seva pròpia base de dades d’usuaris i contrasenyes.  
A més, als ordinadors clients s’utilitza **autenticació local**, cosa que genera diversos problemes:

- ⚙️ **Ineficiència operativa:** Cada vegada que s’incorpora o marxa un empleat, cal crear o eliminar el compte manualment a múltiples sistemes.  
- 🔒 **Risc de seguretat:** Els usuaris sovint reutilitzen contrasenyes entre serveis per evitar oblidar-les.  
- 📈 **Manca d’escalabilitat:** A mesura que l’empresa creix i afegeix nous serveis, la gestió es torna insostenible.

Davant d’aquesta situació, el **CEO d’Innovatech** ha contactat amb nosaltres per **implantar una solució d’autenticació centralitzada**.

---

## 🎯 Missió

La meva missió en aquesta tasca és **implementar un servidor OpenLDAP (Lightweight Directory Access Protocol)** com a solució robusta i de codi obert per unificar la gestió dels usuaris d’Innovatech.

Aquesta solució s’alinea amb la filosofia de l’empresa, ja que tots els ordinadors utilitzen **GNU/Linux** i aposten per **tecnologies lliures i escalables**.

---

## 🧠 Objectius

Amb aquesta tasca, busco aconseguir:

- Entendre el funcionament dels **serveis de directori LDAP** i la seva estructura jeràrquica.  
- **Instal·lar i configurar** un servidor **OpenLDAP** en un entorn Linux.  
- Definir el **domini base** i crear la **jerarquia d’unitats organitzatives (OU)** per gestionar usuaris i grups.  
- Afegir i gestionar **usuaris i grups** dins del directori.  
- Configurar un **client Linux** perquè utilitzi el directori LDAP per **autenticar usuaris**.  
- Documentar tot el procés tècnic de forma clara i professional utilitzant **Markdown i GitHub**.

---

## 🧩 Desenvolupament del projecte

Durant el projecte, he treballat seguint les fases següents:

1. **Instal·lació del servei OpenLDAP** en un servidor Linux.  
2. **Configuració inicial del domini base** (Base DN).  
3. **Creació de la jerarquia del directori**, amb unitats organitzatives per usuaris, grups i departaments.  
4. **Inserció d’usuaris i grups** utilitzant fitxers LDIF.  
5. **Verificació del servei** mitjançant ordres com `ldapsearch` i `ldapwhoami`.  
6. **Integració d’un client Linux** per autenticar usuaris des del directori LDAP.  
7. **Proves d’autenticació i accés**, garantint que tot funcioni de manera centralitzada.  

---

## 🧾 Recursos utilitzats

Per dur a terme aquesta tasca, he utilitzat el **material docent** disponible al Moodle de l’assignatura:

- 📘 *UD04.AA1 — Serveis de Directori*  
- 📘 *UD04.AA2 — Instal·lació OpenLDAP*  
- 📘 *UD04.AA3 — Configuració del Directori*  
- 📘 *UD04.AA5 — Agregar Client al Directori*

A més, he seguit el **plec de condicions tècniques** del projecte, on es detallen els requisits i passos que cal complir.

---

## 💡 Conclusions

Amb aquest projecte he après que **centralitzar la gestió d’usuaris** és essencial en qualsevol empresa que creix.  
El servei **OpenLDAP** permet tenir un **control més segur, eficient i escalable** dels accessos, evitant problemes de seguretat i reduint la càrrega de treball del departament tècnic.

També he consolidat habilitats tècniques com la **configuració de serveis en Linux**, l’ús de **fitxers LDIF**, la **verificació d’autenticacions** i la **documentació tècnica professional** amb GitHub.

---

## ✨ Reflexió final

Aquesta experiència m’ha fet entendre millor com treballen les empreses reals amb sistemes de directori.  
He après a mantenir una visió global: no només instal·lar un servei, sinó **com integrar-lo dins d’una infraestructura IT complexa**.

En resum, implementar OpenLDAP ha estat un **repte tècnic i organitzatiu** que m’ha ajudat a desenvolupar les competències pròpies d’un tècnic de sistemes en un entorn professional.

> *“Centralitzar no és només optimitzar — és posar ordre al caos digital.”*

---

[Solució de la tasca](solucio.md)

---

[Tornar a la pàgina principal](../)
