# 🗂️ T03: Gestió flexible de discos (LVM i Espais d’emmagatzematge)

## 📄 Breu descripció
En aquesta tasca he assumit un nou repte per al client **Garriga i Associats**, un bufet d’advocats que gestiona una gran quantitat d’informació legal sensible. La integritat, la disponibilitat i la facilitat de gestió del seu emmagatzematge són essencials, i per això m’han encomanat el disseny i la documentació de solucions que garanteixin aquests requisits.

La direcció del bufet necessita actualitzar urgentment els seus sistemes de servidors per assegurar:
- Protecció davant fallades de disc  
- Alta redundància  
- Possibilitat d’ampliar l’espai sense interrupcions  

Per això, com a tècnic d’EverPia, he de dissenyar i documentar **dues solucions d’emmagatzematge**, una per **Linux** i una per **Windows**, a partir de proves de concepte fetes amb màquines virtuals.

---

# 🔧 1. Part Linux: LVM amb Zorin OS

En aquesta primera part utilitzo **Zorin OS** (o una distribució Linux equivalent) per demostrar l’ús del **Logical Volume Manager (LVM)**.

### Què he de demostrar?
- **Configuració inicial:** Crear un *volume group* i un *logical volume* utilitzant dos discos de 10 GB, formatejar-lo i muntar-lo automàticament mitjançant `/etc/fstab`.
- **Alta disponibilitat:** Configurar un **mirall LVM** per protegir les dades en cas de fallada d’un disc.
- **Snapshots:** Afegir dos discos més de 10 GB al grup de volums, crear un volum `lvm_dades`, afegir-hi dades i generar un **snapshot** per documentar com es pot restaurar en cas de corrupció.
- **Escalabilitat:** Ampliar el volum `lvm_dades` amb l’espai lliure restant dins del grup de volums.

---

# 🪟 2. Part Windows: Espais d’Emmagatzematge (Storage Spaces)

A la segona part treballo amb **Windows 11** per mostrar les possibilitats dels **Storage Spaces**.

### Què he de demostrar?
- **Configuració inicial:** Crear un *Storage Pool* amb tres discos de 10 GB.
- **Proves de resiliència:**
  - **Mirroring:** demostrant la protecció contra fallades.
  - **Parity:** mostrant la seva eficiència en comparació amb el mirall.
  - **Triple mirall:** afegint els discos addicionals necessaris.
- **Gestió:** Visualitzar l’estat dels discos i del pool des de la consola de Windows per evidenciar la facilitat d’administració.

---

# 🧪 Com treballo i què entrego?

Aquesta tasca es fa en grup. Primer ens dividim en dos equips:  
- Un equip treballa la part de **Linux amb LVM**  
- L’altre la part de **Windows amb Storage Spaces**

Un cop formats els equips, jo preparo **individualment el meu guió** (comandes, procediments i documentació). Després, en parelles, fem les demostracions pràctiques. Finalment, el grup revisa tota la documentació i cada membre la puja al seu repositori.

A la carpeta `tasca03/` he d’incloure:
- La documentació de Linux  
- La documentació de Windows  
- Aquest `README.md` amb la descripció i els enllaços corresponents  

La documentació l’escric en **Markdown**, incloent imatges i explicacions.  
La nota és **grupal**, i posteriorment haurem de presentar al client les conclusions en una presentació conjunta.

---

