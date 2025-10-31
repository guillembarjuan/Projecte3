# T06: Fonaments del servei DNS - Guia d'Auditoria

## Introducció
Aquesta guia explica com realitzar una auditoria DNS utilitzant les eines de línia de comandes més comunes. El DNS (Domain Name System) és com la "agenda de telèfons" d'Internet, convertint noms de domini (com google.com) en adreces IP que els ordinadors poden entendre.

## Fase Pràctica: Diagnosi amb Comandes

### A. Diagnosi Avançada amb `dig`

#### Comanda 1: Consulta Bàsica de Registre A
```bash
dig xtec.cat A
```

![Consulta A de xtec.cat](/tasca06/img/captura1.png)

**Anàlisi:**
- **IP de resposta:** 83.247.151.214
- **Valor TTL:** 3270 segons (aproximadament 54 minuts)
- **Servidor que va respondre:** 127.0.0.53 (el servidor DNS local del sistema)
- **Temps de consulta:** 5 ms (molt ràpid)

El TTL ens indica quant de temps es guardarà aquesta resposta a la memòria cau abans de fer una nova consulta.

#### Comanda 2: Consulta de Servidors de Noms (NS)
```bash
dig tecnocampus.cat NS
```

![Consulta NS de tecnocampus.cat](/tasca06/img/captura2.png)

**Anàlisi - Servidors de noms autoritatius:**
El domini tecnocampus.cat té quatre servidors de noms autoritatius:
- ns-1689.awsdns-19.co.uk
- ns-535.awsdns-02.net
- ns-1071.awsdns-05.org
- ns-130.awsdns-16.com

Aquests servidors pertanyen a AWS (Amazon Web Services) i són els responsables finals de donar informació autoritativa sobre aquest domini.

#### Comanda 3: Consulta Detallada SOA
```bash
dig escolapia.cat SOA
```

![Consulta SOA de escolapia.cat](/tasca06/img/captura3.png)

**Anàlisi - Informació SOA:**
- **Correu de l'administrador:** root.dns1.nominalia.com
- **Número de sèrie:** 1761028965
- **Servidor DNS primari:** dns1.nominalia.com

El registre SOA és com el "document d'identitat" del domini, conté informació vital com qui és l'administrador i quan va ser l'última actualització.

#### Comanda 4: Consulta de Resolució Inversa
```bash
dig -x 147.83.2.135
```

![Resolució inversa](/tasca06/img/captura4.png)

**Anàlisi - Informació obtinguda:**
La IP 147.83.2.135 està associada a múltiples noms de domini de la UPC:
- barcelonatech.upc.edu
- upc.edu
- www.upc.es
- upc.cat
- i altres...

Això significa que una mateixa IP pot servir múltiples webs i serveis diferents.

### B. Comprovació de Resolució amb `nslookup`

#### Comanda 1: Consulta Bàsica no Autoritativa
```bash
nslookup
> set type=A
> tecnocampus.cat
```

![Consulta no autoritativa](/tasca06/img/captura5.png)

**Anàlisi - Per què és "no autoritativa"?**

La resposta és no autoritativa perquè ve del nostre servidor DNS local (127.0.0.53) que ha guardat la resposta a la memòria cau, no directament dels servidors autoritatius de tecnocampus.cat. És com obtenir informació d'un amic que ja ho sabia, en llocs de preguntar directament a la font original.

#### Comanda 2: Consultes Autoritatives
```bash
> server ns-535.awsdns-02.net
> set type=A
> tecnocampus.cat
```

![Configuració servidor](/tasca06/img/captura7.png)

![Consulta autoritativa](/tasca06/img/captura8.png)

**Anàlisi - Diferències amb la comanda 1:**
- **Servidor:** Ara consultem directament a ns-535.awsdns-02.net (un servidor autoritatiu)
- **Resposta:** És autoritativa (ve directament de la font)
- **Informació:** És idèntica però més fiable

Quan consultem directament als servidors autoritatius, obtenim la informació directament de la font, sense intermediaris.

### C. Consulta de Servidors de Noms amb nslookup
```bash
nslookup
> set type=NS
> tecnocampus.cat
```

![Consulta NS amb nslookup](/tasca06/img/captura6.png)

**Anàlisi:**
Aquesta comanda confirma els mateixos quatre servidors de noms que vam veure amb `dig`, demostrant que diferents eines poden donar resultats consistents.

## Conclusions de l'Auditoria

### Resum dels Resultats
- **Resolució DNS:** Funciona correctament amb temps de resposta ràpids
- **Infraestructura:** Els dominis auditats utilitzen serveis cloud (AWS) amb bona redundància
- **Configuració:** Els registres estan ben configurats i les respostes són consistents

---

[Explicació de la tasca](README.md)

[Tornar a la pàgina principal](../)
