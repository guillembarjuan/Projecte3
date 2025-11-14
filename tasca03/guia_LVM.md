# Part Linux: LVM amb Zorin OS (Marc i Martí)
S'ha d'utilitzar la distribució Zorin OS (o una alternativa Linux compatible) per demostrar la utilitat del Logical Volume Manager (LVM).

# Requisits de la Implementació i Demostració:
### 1. Configuració Inicial: Crear un grup de volums (VG) i un volum lògic (LV) utilitzant inicialment un mínim de dos discs durs (simulats) de 10 GB de capacitat. Aquest volum haurà estar formatat i muntat automàticament al sistema mitjançant l’edició de l’arxiu /etc/fstab.
![solucio](/tasca03/img_LVM/cap1.png)
![solucio](/tasca03/img_LVM/cap2.png)
![solucio](/tasca03/img_LVM/cap3.png)
![solucio](/tasca03/img_LVM/cap4.png)
![solucio](/tasca03/img_LVM/cap5.png)
![solucio](/tasca03/img_LVM/cap6.png)
![solucio](/tasca03/img_LVM/cap7.png)

### 2. Alta Disponibilitat: Implementar la configuració d’un mirall (lvm_mirror) que protegeixi la informació davant la fallada d'un disc.
![solucio](/tasca03/img/cap8.png)


### 3. Instantànies (snapshots):  Crear i afegir dos discos de 10 GB al grup de volums. Crear un volum (lvm_dades) amb el primer disc afegit, formatar-lo i muntar-lo. A continuació afegir arxius al volum (poden ser imatges d’Internet). Usar el segon disc afegit per crear un snapshot (lv_snapshot) i documentar com es pot restaurar aquest snapshot, si per exemple, la informació del volum original es danyés.
![solucio](/tasca03/img_LVM/cap9.png)
![solucio](/tasca03/img_LVM/cap10.png)
![solucio](/tasca03/img_LVM/cap12.png)
![solucio](/tasca03/img_LVM/cap13.png)
![solucio](/tasca03/img_LVM/cap14.png)
![solucio](/tasca03/img_LVM/cap15.png)
![solucio](/tasca03/img_LVM/cap16.png)
![solucio](/tasca03/img_LVM/cap17.png)
![solucio](/tasca03/img_LVM/cap18.png)
![solucio](/tasca03/img_LVM/cap19.png)
![solucio](/tasca03/img_LVM/cap20.png)
![solucio](/tasca03/img_LVM/cap21.png)
![solucio](/tasca03/img_LVM/cap22.png)
![solucio](/tasca03/img_LVM/cap23.png)

### 4. Escalabilitat: Demostrar el procés d'ampliació*.* Usar l’espai que quedi lliure dins el grup de volums per ampliar el volum lv_dades.
![solucio](/tasca3/img_LVM/cap24.png)
![solucio](/tasca3/img_LVM/cap25.png)
![solucio](/tasca3/img_LVM/cap26.png)
![solucio](/tasca3/img_LVM/cap27.png)
