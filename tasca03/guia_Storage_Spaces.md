# Guia Pràctica - Storage Spaces a Windows

Com a tècnics d'EverPia, ens han encarregat implementar una solució d'emmagatzematge resilient per al bufet d'advocats "Garriga i Associats". En aquesta guia us mostrarem com configurar **Storage Spaces** a Windows per assolir alta disponibilitat i redundància.

## Què farem en aquesta guia?

- **Preparació dels discs** virtuals
- **Creació d'un Grup d'Emmagatzematge**
- **Configuració d'Espais amb diferents tipus de resiliència**
- **Proves de tolerància a errors**

## 1. Preparació dels discs virtuals

Abans de començar, hem afegit **tres discs virtuals de 10 GB** cadascun a la nostra màquina virtual Windows 11.

![Configuració dels discs a VirtualBox](/tasca03/img_t03/captura01.png)

Un cop arrenquem la VM, obrim l'**Administrador de discos** i veiem els tres discs nous sense inicialitzar.

![Discs sense inicialitzar](/tasca03/img_t03/captura02.png)

## 2. Inicialització dels discs

Seleccionem els tres discs i els inicialitzem amb l'estil de partició **GPT**:

![Inicialització dels discs](/tasca03/img_t03/captura04.png)

Després de la inicialització, ja tenim els tres discs preparats per ser utilitzats:

![Discs inicialitzats i preparats](/tasca03/img_t03/captura06.png)

## 3. Creació del Grup d'Emmagatzematge

Ara anem al **Panell de control → Espais d'emmagatzematge** i creem un nou grup:

![Accés als Espais d'emmagatzematge](/tasca03/img_t03/captura07.png)

Seleccionem els **tres discs de 10 GB** per crear el nostre grup d'emmagatzematge:

![Selecció dels discs per al grup](/tasca03/img_t03/captura08.png)

## 4. Creació de l'Espai amb Mirall Doble

Ara creem el nostre primer espai d'emmagatzematge. Escollim:
- **Nom:** Mirall doble
- **Lletra d'unitat:** M:
- **Tipus de resiliència:** Reflejo doble (Mirall doble)

![Configuració de l'espai amb mirall doble](/tasca03/img_t03/captura09.png)

**Nota important:** El mirall doble crea **dues còpies** de les dades, protegint-nos contra errors en una de les unitats.

Un cop creat, ja tenim el nostre espai funcionant:

![Espai creat correctament](/tasca03/img_t03/captura10.png)

## 5. Comprovació del funcionament

Ara podem veure la nostra nova unitat **M:** a l'Explorador d'arxius:

![Unitad M: visible a l'Explorador](/tasca03/img_t03/captura11.png)

I per provar que funciona, hi creem un arxiu de prova:

![Arxiu de prova creat](/tasca03/img_t03/captura12.png)

## 6. Preparació per la prova de tolerància

Ara ve la part interessant: **provarem què passa quan falla un disc**. Per fer-ho, apaguem la màquina virtual:

![Apagat de la VM](/tasca03/img_t03/captura13.png)

I eliminem un dels discs del grup (en aquest cas, el disc3):

![Eliminació d'un disc](/tasca03/img_t03/captura14.png)

Perfecte! Continuo amb les captures 15 i 16 que m'has explicat.

## 7. Prova de Tolerància a Errors - Disc Desconnectat

Ara veiem el moment de la veritat: **què passa quan un disc falla?**

Després d'apagar la VM i eliminar físicament un dels discs (en aquest cas el disc 3), tornem a engegar la màquina.

Quan obrim de nou **Espais d'Emmagatzematge**, veiem que ens surt una **alerta important**:

![Alerta per disc retirat](/tasca03/img_t03/captura15.png)

El sistema ens avisa que ha **perdut la comunicació** amb una de les unitats físiques. Això és normal - li hem "simulat" un error de disc al sistema.

**El més important:** Tot i la pèrdua d'un disc, si anem a l'**Explorador d'arxius**:

![Accés a les dades sense problemes](/tasca03/img_t03/captura16.png)

**SORPRESA!** 💥 La nostra unitat **M:** segueix accessible i podem veure i obrir el nostre arxiu "arxiu prova.txt" sense cap problema.

**Això demostra que el MIRALL DOBLE funciona correctament:** Encara que un disc falli, la segona còpia de les dades ens permet continuar treballant sense pèrdua d'informació.

Perfecte! Ara continuo amb la resta de la guia incloent la captura 17 que m'has explicat i totes les que m'has passat.

## Prova de Tolerància a Errors - Disc Desconnectat (Continuació)

Com et vaig explicar abans, després d'eliminar un disc, **l'arxiu "arxiu prova.txt" segueix accessible** dins de la unitat M:. Això demostra que el mirall doble funciona correctament:

![Arxiu accessible després del error](/tasca03/img_t03/captura17.png)

**El mirall doble ha funcionat perfectament!** Tot i perdre un disc físic, les nostres dades segueixen intactes i accessibles.

## 8. Restauració del Mirall Doble

Ara tornem a connectar el disc que vam eliminar. Primer apaguem la VM:

![Preparació per afegir el disc](/tasca03/img_t03/captura18.png)

I afegim de nou el disc 3 a la configuració de VirtualBox:

![Disc afegit de nou](/tasca03/img_t03/captura19.png)

Quan tornem a engegar Windows, el sistema **automàticament detecta el disc retornat** i comença a resincronitzar les dades:

![Sistema detectant el disc retornat](/tasca03/img_t03/captura20.png)

Podem comprovar que el nostre arxiu segueix intacte:

![Arxiu encara accessible](/tasca03/img_t03/captura21.png)

I l'Administrador de discos mostra que tenim tots els discs disponibles:

![Tots els discs disponibles](/tasca03/img_t03/captura22.png)

## 9. Eliminació de l'Espai Anterior

Abans de continuar amb les noves configuracions, eliminem l'espai de mirall doble que vam crear:

![Eliminació de l'espai anterior](/tasca03/img_t03/captura23.png)

I tornem a crear un nou grup d'emmagatzematge:

![Creació de nou grup](/tasca03/img_t03/captura24.png)

## 10. Configuració d'Espai amb Paritat

Ara anem a provar una altra configuració: **l'espai amb paritat**. Per fer-ho, necessitem preparar més discs. Afegim **2 discs addicionals** de 10 GB:

![Configuració amb 5 discs](/tasca03/img_t03/captura25.png)

Inicialitzem els nous discs:

![Inicialització dels nous discs](/tasca03/img_t03/captura26.png)

I creem un nou grup amb **tots els 5 discs**:

![Grup amb 5 discs](/tasca03/img_t03/captura27.png)

Ara creem un **espai amb mirall triple** (que ofereix encara més redundància):

![Configuració de mirall triple](/tasca03/img_t03/captura28.png)

Un cop creat, veiem el nou espai amb mirall triple:

![Espai de mirall triple creat](/tasca03/img_t03/captura29.png)

I podem veure que tenim **5 discs físics** participant en el grup:

![Llista dels 5 discs](/tasca03/img_t03/captura30.png)

## 11. Comprovació del Mirall Triple

Creem un arxiu de prova dins de la nova unitat T: (Mirall triple):

![Arxiu de prova al mirall triple](/tasca03/img_t03/captura31.png)

## 12. Prova de Tolerància

Ara fem una prova més extrema: eliminem **2 discs simultàniament** per veure si el mirall triple aguanta:

![Eliminació de 2 discs](/tasca03/img_t03/captura32.png)

El sistema ens mostra una **advertència** que hem perdut redundància, però les dades segueixen accessibles:

![Advertència per pèrdua de redundància](/tasca03/img_t03/captura33.png)

Podem veure que dos dels discs mostren advertències:

![Discs amb problemes](/tasca03/img_t03/captura34.png)

Però el més important: **el nostre arxiu segueix accessible**!

![Arxiu encara accessible després de 2 errors](/tasca03/img_t03/captura35.png)

Perfecte! Ara afegiré aquesta última part a la guia que demostra què passa quan només tenim un disc i intentem fer una configuració sense redundància.

## 13. Configuració Sense Redundància - Espai Simple

Ara farem una prova extra: eliminem **tots els discs excepte un** per crear un espai sense redundància:

![Configuració amb només un disc](/tasca03/img_t03/captura36.png)

Veiem que ara només tenim el **Disc 1** disponible:

![Només un disc disponible](/tasca03/img_t03/captura37.png)

Creem un **espai simple** (sense redundància) amb aquest únic disc:

![Creació d'espai simple](/tasca03/img_t03/captura38.png)

Un cop creat, podem veure la nova unitat **E:** a l'Explorador d'arxius:

![Unitad E: visible](/tasca03/img_t03/captura39.png)

I hi creem un arxiu de prova:

![Arxiu de prova a l'espai simple](/tasca03/img_t03/captura40.png)

## 14. Prova de Fallada amb Espai Simple

Ara fem la prova crítica: apaguem la VM i eliminem l'únic disc que tenim:

![Preparació per eliminar l'únic disc](/tasca03/img_t03/captura41.png)

Quan tornem a engegar la VM, anem a Espais d'Emmagatzematge i veiem que **no hi ha cap grup creat**:

![No hi ha grup d'emmagatzematge](/tasca03/img_t03/captura42.png)

I el més important: **la unitat E: HA DESAPAREGUT** de l'Explorador d'arxius!

![Unitad E: desapareguda](/tasca03/img_t03/captura43.png)

## 15. Conclusions Finals - Comparativa de Configuracions

Després de totes les proves, podem fer una **comparativa clara**:

### 🔴 **Espai Simple (1 disc)**
- **❌ Sense redundància**
- **❌ Pèrdua total de dades amb un sol error**
- **✅ Màxim espai útil (100%)**
- **⛔ NO RECOMANABLE per a dades importants**

### 🟡 **Mirall Doble (2+ discs)**
- **✅ Redundància bàsica**
- **✅ Sobreviu a 1 error de disc**
- **⏺️ 50% d'eficiència d'espai**
- **✅ BONS per a la majoria de casos**

### 🟢 **Mirall Triple (5+ discs)**
- **✅ Alta redundància**
- **✅ Sobreviu a 2 errors simultanis**
- **⏺️ 33% d'eficiència d'espai**
- **✅ EXCEL·LENT per a dades crítiques**

### 🔵 **Paritat (3+ discs)**
- **✅ Bon equilibri espai/seguretat**
- **✅ Sobreviu a 1 error**
- **✅ Millor eficiència d'espai**
- **✅ RECOMANABLE per a emmagatzematge gran**

---

## 📋 **Recomanació per a "Garriga i Associats"**

Per al bufet d'advocats, que gestiona informació legal sensible, recomanem:

**✅ CONFIGURACIÓ ÒPTIMA: Mirall Doble o Triple**
- **Raó:** Màxima protecció de dades
- **Avantatge:** Les dastes es mantenen accessibles encara amb múltiples fallades
- **Adequat:** Per a la importància crítica de la seva informació legal

**La inversió en discs addicionals val la pena per la seguretat que ofereix!** 🛡️

---

[Explicació de la tasca](README.md)

---

[Guia de LVM](guia_LVM.md)

---

[Tornar a la pàgina principal del projecte](../)

