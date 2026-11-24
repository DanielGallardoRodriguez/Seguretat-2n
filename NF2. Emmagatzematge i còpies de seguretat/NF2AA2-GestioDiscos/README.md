# Gestió d'emmagatzematge redundant — Informe final

**Mòdul:** Seguretat — NF2. Emmagatzematge i còpies de seguretat
**Activitat:** NF2AA2 — Gestió d'emmagatzematge redundant
**Empresa fictícia:** EverPia
**Autor:** Daniel Gallardo Rodríguez

---

## Índex

1. [Context](#1-context)
2. [Contingut del repositori](#2-contingut-del-repositori)
3. [Entorn de treball](#3-entorn-de-treball)
4. [Part 1: LVM a Linux](#4-part-1-lvm-a-linux)
5. [Part 2: Espais d'emmagatzematge a Windows](#5-part-2-espais-demmagatzematge-a-windows)
6. [Comparació de les dues solucions](#6-comparació-de-les-dues-solucions)
7. [Incidències trobades](#7-incidències-trobades)
8. [Conclusions i proposta al client](#8-conclusions-i-proposta-al-client)
9. [Ús de la IA](#9-ús-de-la-ia)

---

## 1. Context

EverPia és una consultora tecnològica que ha estat contractada per un bufet d'advocats amb dades molt sensibles. El sistema d'emmagatzematge actual del client és vulnerable a fallades de disc i a la pèrdua d'informació. La direcció demana renovar els servidors de manera que:

- La informació quedi protegida davant la fallada d'un disc.
- L'espai es pugui ampliar sense aturar el servei.

Aquest repositori conté la prova de concepte de dues solucions, una per a servidors Linux i una altra per a servidors Windows. No s'ha treballat amb servidors reals. Cada solució s'ha muntat en una màquina virtual de VirtualBox, i els procediments s'han documentat pas a pas amb captures de pantalla.

## 2. Contingut del repositori

| Fitxer | Descripció |
|---|---|
| [`README.md`](README.md) | Aquest document: resum de l'activitat i conclusions. |
| [`Tascapartlinux.md`](Tascapartlinux.md) | Documentació de la part de Linux: LVM a Zorin OS. |
| [`tascapartwindows.md`](tascapartwindows.md) | Documentació de la part de Windows: Espais d'emmagatzematge a Windows 11. |
| [`captures/`](captures/) | Captures de pantalla usades als dos documents. |

## 3. Entorn de treball

Totes dues parts s'han fet a **VirtualBox**, amb discs virtuals de tipus VDI i reservats dinàmicament.

| | Part 1 (Linux) | Part 2 (Windows) |
|---|---|---|
| Màquina | `EverPia-Zorin-LVM` | `EverPia-Win11-SS` |
| Sistema operatiu | Zorin OS 18.1 Core, 64 bits | Windows 11 Enterprise LTSC (avaluació), compilació 26100 |
| Disc del sistema | 25 GB (`/dev/sda`) | 64 GB |
| Eina | LVM | Espais d'emmagatzematge (Storage Spaces) |
| Discs addicionals | Discs de 10 GB | Cinc discs de 10 GB |

## 4. Part 1: LVM a Linux

Documentació completa: [`Tascapartlinux.md`](Tascapartlinux.md).

Amb LVM es treballa en tres capes: volums físics (PV), grups de volums (VG) i volums lògics (LV). Això és el que s'ha fet:

| Requisit | Resultat |
|---|---|
| Grup de volums i volum lògic amb dos discs de 10 GB, formatat i muntat amb `/etc/fstab` | Grup `vg1` amb `sdb` i `sdc` (uns 20 GB), volum `lvl` de 2 GB amb ext4, muntat a `/mnt/lvm` i comprovat després de reiniciar |
| Alta disponibilitat: mirall `lvm_mirror` | Grup `vg_mirror` amb `sdd` i `sde` i volum `lvm_mirror` de 750 MB amb `-m 1`, sincronitzat al 100 % |
| Instantànies | `sdf` i `sdg` afegits a `vg1`; `lvm_dades` de 5 GB a `sdf` amb imatges; `lv_snapshot` de 2 GB a `sdg` |
| Restauració del snapshot | Esborrat d'un fitxer, `lvconvert --merge` i fitxer recuperat |
| Escalabilitat | `lvm_dades` ampliat de 5 GB a 10 GB amb l'espai lliure del grup, sense perdre dades |

## 5. Part 2: Espais d'emmagatzematge a Windows

Documentació completa: [`tascapartwindows.md`](tascapartwindows.md).

Els Espais d'emmagatzematge agrupen discs físics en un grup (*pool*) i en treuen espais virtuals amb un nivell de resistència. Això és el que s'ha fet:

| Requisit | Resultat |
|---|---|
| Grup d'emmagatzematge amb tres discos de 10 GB | Grup `tasca`, creat amb els discos 1 i 2 i ampliat després amb el tercer |
| Mirall (dos discos) i alta disponibilitat | `tasca (E:)` de 9,5 GB amb *Reflejo doble*. En treure un disc, el grup passa a *Degradado* i les dades continuen accessibles; en reconnectar-lo torna a *Aceptar* |
| Paritat (tres discos) | `paridad (F:)` de 19 GB amb *Paridad* |
| Mirall triple (cinc discos) | `Mirall triple (G:)` de 15 GB amb *Reflejo triple*, amb els discos 4 i 5 afegits al grup (47,4 GB en total) |
| Gestió des de Windows | Estat vist a Configuració, a Administració de discos i amb PowerShell (`Get-StoragePool`, `Get-PhysicalDisk`, `Get-VirtualDisk`) |

La integritat de les dades es va comprovar calculant el hash SHA256 dels fitxers abans, durant i després de la fallada del disc. Els valors van ser idèntics.

## 6. Comparació de les dues solucions

| Aspecte | LVM (Linux) | Espais d'emmagatzematge (Windows) |
|---|---|---|
| Disc físic | Volum físic (PV) | Disc físic del grup |
| Contenidor de discs | Grup de volums (VG) | Grup d'emmagatzematge (*pool*) |
| Volum virtual | Volum lògic (LV) | Espai d'emmagatzematge |
| Mirall | `lvcreate -m 1` | Resistència *Reflejo doble* |
| Paritat | No s'ha treballat en aquesta activitat | Resistència *Paridad*, amb tres discos |
| Mirall triple | `-m 2`, no s'ha provat | *Reflejo triple*, amb cinc discos |
| Instantànies | Sí, amb `lvcreate -s` i `lvconvert --merge` | No s'han treballat en aquesta activitat |
| Ampliació | `lvextend` i `resize2fs`, amb el volum desmuntat | Afegir discos al grup; els espais són d'aprovisionament fi |
| Gestió | Línia d'ordres | Configuració, Administració de discos i PowerShell |
| Format | A part: `mkfs` i `mount` | A part: assistent de volum o Administració de discos |

Eficiència de l'espai aproximada segons el tipus de resistència:

| Tipus | Espai útil | Tolera |
|---|---|---|
| Mirall doble | 50 % | 1 disc |
| Paritat amb 3 discos | 67 % | 1 disc |
| Mirall triple | 33 % | 2 discos |

## 7. Incidències trobades

Les incidències estan explicades amb detall als documents de cada part. Les més rellevants:

**Linux**

- Es va confondre el nom `lvl` amb `lv1` en formatar el volum, i `mkfs` no el trobava.
- Un mirall no es pot crear amb discs que ja són en un altre grup. Per això s'han afegit dos discs nous i un grup `vg_mirror`.
- En crear el snapshot es va escriure `-l 2g` en lloc de `-L 2G`.
- Després de restaurar el snapshot, systemd avisava que `/etc/fstab` s'havia modificat.

**Windows**

- La màquina no arrencava la primera vegada: calia muntar la ISO i revisar l'ordre d'arrencada.
- La mida proposada per defecte per al mirall doble (18,99 GB) no es pot garantir amb dos discs de 10 GB. Es va reduir a 9,5 GB.
- Amb el mirall triple, Configuració no deixa formatar l'espai perquè l'ús màxim del grup (45 GB) supera la capacitat que tenia aleshores (28,4 GB). El volum es va crear des d'Administració de discos.
- Windows 11 xifra automàticament amb BitLocker els volums nous.
- A PowerShell es va escriure `Syze` en lloc de `Size`, i la columna de la mida va sortir buida.

## 8. Conclusions i proposta al client

- **Protecció davant fallades.** Tant LVM amb mirall com els Espais d'emmagatzematge amb mirall doble mantenen les dades si falla un disc. A Windows s'ha comprovat amb un disc desconnectat: el grup passa a degradat, però les dades i els hashes es conserven.
- **Escalabilitat.** En totes dues solucions es poden afegir discos al grup i ampliar l'espai sense reinstal·lar res. A LVM l'ampliació del volum requereix desmuntar-lo.
- **Cost de l'espai.** La redundància gasta capacitat. El mirall doble només aprofita la meitat; la paritat aprofita més, però escriure és més lent. El mirall triple protegeix més, però exigeix cinc discos i només aprofita un terç.
- **Proposta.** Per al bufet d'advocats, la solució depèn del sistema operatiu dels servidors. En Linux, LVM amb mirall i instantànies abans de les còpies de seguretat. En Windows, Espais d'emmagatzematge amb mirall doble, o amb paritat si es prioritza l'espai.
- **Pendent.** Aquesta redundància no substitueix les còpies de seguretat: un mirall copia també els errors i els esborrats. La prova de fallada de disc només s'ha fet amb el mirall doble de Windows. Al mirall de LVM i a la paritat i el mirall triple de Windows no s'ha desconnectat cap disc.

## 9. Ús de la IA

Aquesta activitat permet un ús parcial de la IA (semàfor taronja). La configuració de les màquines virtuals, les proves i les captures són meves. S'ha usat una eina d'IA com a suport per estructurar i redactar la documentació a partir de les meves notes i captures, i per revisar-ne l'ortografia. Els resultats s'han revisat i validat contra el que es va fer a les màquines.

