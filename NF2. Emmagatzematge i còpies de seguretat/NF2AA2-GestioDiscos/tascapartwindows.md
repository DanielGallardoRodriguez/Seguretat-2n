# Gestió d'emmagatzematge redundant — Part 2: Espais d'emmagatzematge a Windows 11

**Activitat:** Gestió d'emmagatzematge redundant (SMX)
**Empresa fictícia:** EverPia
**Sistema operatiu:** Windows 11 Enterprise LTSC (avaluació), compilació 26100, 64 bits
**Eina principal:** Espais d'emmagatzematge (Storage Spaces)
**Entorn:** VirtualBox

---

## Índex

1. [Objectiu](#1-objectiu)
2. [Preparació de la màquina virtual](#2-preparació-de-la-màquina-virtual)
3. [Instal·lació de Windows 11](#3-instal·lació-de-windows-11)
4. [Discs addicionals](#4-discs-addicionals)
5. [Inicialització dels discs](#5-inicialització-dels-discs)
6. [Grup d'emmagatzematge](#6-grup-demmagatzematge)
7. [Espai amb mirall doble](#7-espai-amb-mirall-doble)
8. [Prova de tolerància a fallades](#8-prova-de-tolerància-a-fallades)
9. [Espai amb paritat](#9-espai-amb-paritat)
10. [Espai amb mirall triple](#10-espai-amb-mirall-triple)
11. [Gestió i estat des de Windows](#11-gestió-i-estat-des-de-windows)
12. [Comparació de configuracions](#12-comparació-de-configuracions)
13. [Limitacions i observacions](#13-limitacions-i-observacions)
14. [Resum de la part de Windows](#14-resum-de-la-part-de-windows)

---

## 1. Objectiu

Dissenyar i documentar una solució d'emmagatzematge redundant per a un servidor Windows, usant els Espais d'emmagatzematge (Storage Spaces). La solució ha de permetre:

- Agrupar diversos discs físics en un grup d'emmagatzematge (*storage pool*).
- Crear espais amb mirall doble, paritat i mirall triple.
- Comprovar que la informació es manté quan falla un disc.
- Veure l'estat dels discs i del grup des de les eines de gestió de Windows.

## 2. Preparació de la màquina virtual

La màquina es diu `EverPia-Win11-SS`. Es crea a VirtualBox amb la ISO de Windows 11 Enterprise LTSC (avaluació) i s'emmagatzema a la carpeta `D:\`. No s'activa la instal·lació desatesa (*unattended*), perquè la instal·lació es fa a mà.

![Assistent de nova màquina virtual](captures/01-vm-nom-iso.png)

Hi ha dues imatges ISO descarregades. S'usa `Windows11_LTSC.iso`.

![Imatges ISO disponibles](captures/03-isos-disponibles.png)

El disc principal és un VDI de 64 GB, dinàmicament reservat (no s'activa *Pre-allocate Full Size*).

![Disc principal de 64 GB](captures/02-vm-disc-principal.png)

Un cop creada, aquestes són les característiques inicials de la màquina: 4096 MB de RAM, 2 processadors, TPM 2.0, EFI i Secure Boot activats.

![Detalls inicials de la màquina](captures/04-vm-detalls-inicials.png)

## 3. Instal·lació de Windows 11

Windows 11 exigeix EFI, Secure Boot i TPM 2.0. Es revisa a **Configuració → Sistema** que els tres estiguin activats i es canvia el chipset a **ICH9**.

![Configuració del sistema: ordre d'arrencada i TPM](captures/05-vm-sistema-ordre-arrencada.png)

![Chipset ICH9 i Secure Boot](captures/06-vm-sistema-ich9-uefi.png)

> **Incidència:** en la primera arrencada la màquina no troba cap sistema operatiu i VirtualBox mostra l'avís *The virtual machine failed to boot*. Això passa perquè la ISO no estava muntada o l'ordre d'arrencada no començava pel lector òptic. Es resol amb el botó **Mount and Retry Boot** i comprovant que l'ordre d'arrencada és *Òptic, Disquet, Disc dur*.

![Error d'arrencada de la màquina virtual](captures/07-error-arrencada.png)

![Ordre d'arrencada amb l'òptic en primer lloc](captures/10-vm-sistema-ordre-optic.png)

Amb l'ordre corregit comença la instal·lació. Windows es reinicia diverses vegades.

![Instal·lant Windows 11](captures/08-instalant-windows.png)

Després se segueix l'assistent inicial: país o regió i compte d'usuari.

![Selecció de país o regió](captures/09-regio.png)

![Pantalla de compte](captures/12-compte-microsoft.png)

També es deixa el controlador gràfic en **VMSVGA**, que és el que es veu a la configuració final de la màquina.

![Controlador gràfic VMSVGA](captures/11-vm-pantalla-vmsvga.png)

## 4. Discs addicionals

Amb la màquina apagada s'afegeixen tres discs virtuals de 10 GB al controlador SATA. Es creen des de **VirtualBox → Configuració → Emmagatzematge → Controlador SATA → Afegir disc dur**, amb tipus **VDI** i emmagatzematge **dinàmicament reservat**. Es diuen `EverPia-Win11-Disco1`, `EverPia-Win11-Disco2` i `EverPia-Win11-Disco3`.

![Controlador SATA a la configuració d'emmagatzematge](captures/13-vm-controlador-sata.png)

Les característiques finals de la màquina, amb el chipset ICH9, l'ordre d'arrencada corregit i el controlador gràfic VMSVGA:

![Detalls finals de la màquina](captures/14-vm-detalls-finals.png)

Es posa en marxa la màquina i s'arriba a l'escriptori de Windows 11. A l'extrem inferior dret es veu la marca d'aigua de la versió d'avaluació, amb la compilació 26100.

![Escriptori de Windows 11](captures/15-escriptori-windows.png)

## 5. Inicialització dels discs

Els discs nous no tenen cap taula de particions. En obrir **Administració de discos** (clic dret sobre Inici → *Administració de discos*) apareix l'assistent **Inicialitzar disc**. S'escull l'estil de partició **MBR**, igual que a la guia de l'activitat.

![Assistent d'inicialització de discs amb MBR](captures/16-inicialitzar-discs.png)

Després, els tres discs de 10 GB surten com a *Bàsic*, *En línia* i *No assignat*. No es creen volums: el grup d'emmagatzematge necessita els discs lliures.

![Administració de discos amb els tres discs nous](captures/17-administracio-discs.png)

> **Alternativa amb PowerShell:** es poden inicialitzar tots els discs en brut d'un sol cop, des d'un PowerShell executat com a administrador.
>
> ```powershell
> $disk = Get-Disk | Where-Object PartitionStyle -eq 'RAW'
> $disk | Initialize-Disk -PartitionStyle MBR
> ```

## 6. Grup d'emmagatzematge

El grup es crea a **Configuració → Sistema → Emmagatzematge → Configuració avançada d'emmagatzematge → Espais d'emmagatzematge → Agregar un nuevo grupo de almacenamiento**.

Es dona nom al grup (`tasca`, que Windows mostra com a *Grupo de almacenamiento tasca*) i es marquen els **discos 1 i 2**. El disc 3 es deixa fora del grup. Un grup admet més discs en qualsevol moment, de manera que s'afegirà més endavant. Amb dos discs, la capacitat projectada és d'uns 19 GB.

![Creació del grup d'emmagatzematge](captures/18-nou-grup.png)

## 7. Espai amb mirall doble

En acabar de crear el grup, Windows ofereix crear un espai d'emmagatzematge dins del grup. Si es prem **Omitir**, es pot crear més endavant.

![Assistent de nou espai, amb la mida proposada de 18,99 GB](captures/19-nou-espai.png)

Es configura així:

- **Nom:** `Espacio de almacenamiento tasca`.
- **Mida:** 9,5 GB. Windows proposa primer 18,99 GB, que és la capacitat del grup. Però amb un mirall doble cada dada es desa dues vegades, i amb dos discs de 10 GB l'espai útil és d'uns 10 GB. Per això es canvia a 9,5 GB. Un espai pot ser més gran que la capacitat real del grup, perquè Windows aprovisiona l'espai de manera fina (*thin provisioning*), però aleshores no es poden garantir les còpies.
- **Resistència:** **Reflejo doble**. Fa dues còpies de les dades i necessita com a mínim dos discs físics. Es comporta com un RAID 1.

![Espai de 9,5 GB amb mirall doble](captures/20-espai-mirall-doble.png)
Després cal donar format al volum, com qualsevol disc nou. S'indica l'etiqueta, la lletra d'unitat i el sistema de fitxers **NTFS**.

![Assistent de nou volum](captures/21-nou-volum.png)

![Volum amb l'etiqueta tasca](captures/22-nou-volum-etiqueta.png)

El volum apareix a l'explorador de fitxers com a `tasca (E:)`.

![Volum tasca (E:) a Aquest equip](captures/23-aquest-equip-tasca.png)

Per poder comprovar després la redundància, s'hi desen dades. Primer dos fitxers de text buits (`ej.txt` i `in.txt`) i després dues imatges `.jpg`.

![Fitxers de text al volum](captures/24-fitxers-txt.png)

![Imatges afegides al volum](captures/25-fitxers-jpg.png)

Per demostrar que les dades no es corrompen, se'n calcula el hash SHA256 amb PowerShell. Aquest valor serveix de referència per comparar-lo després de la fallada.

```powershell
Get-ChildItem E:\ -File | Get-FileHash -Algorithm SHA256
```

- `Get-ChildItem E:\ -File` llista els fitxers del volum.
- `Get-FileHash -Algorithm SHA256` calcula el hash de cada fitxer. Si el contingut canvia, canvia el hash.
- Els dos `.txt` tenen el hash `E3B0C442…B855`, que és el d'un fitxer buit.

![Hash dels fitxers abans de la fallada](captures/26-hash-inicial.png)

## 8. Prova de tolerància a fallades

El mirall ha de seguir funcionant si es perd un disc. Per comprovar-ho, s'apaga la màquina i, des de **VirtualBox → Configuració → Emmagatzematge**, es veuen els tres discs addicionals connectats als ports SATA 2, 3 i 4.

![Discs addicionals connectats al controlador SATA](captures/27-vm-discs-connectats.png)

Es desconnecta `EverPia-Win11-Disco2` amb clic dret → **Remove attachment**. D'aquesta manera es simula un disc que s'ha espatllat.

![Desconnexió d'un dels discs del mirall](captures/28-vm-treure-disc.png)

![Màquina sense el disc desconnectat](captures/29-vm-disc-desconnectat.png)

En tornar a arrencar, el volum `tasca (E:)` continua accessible i conté els quatre fitxers.

![Dades accessibles amb un disc menys](captures/30-dades-accessibles.png)

![Volum E: a Aquest equip amb un disc menys](captures/31-aquest-equip-degradat.png)

Però el grup d'emmagatzematge ja avisa del problema: l'estat és **Advertencia, Degradado**.

![Grup en estat degradat](captures/32-grup-degradat.png)

Es recalcula el hash dels fitxers amb la mateixa comanda. Els valors són idèntics als d'abans, de manera que la informació no s'ha perdut ni alterat.

![Hash dels fitxers amb el grup degradat](captures/33-hash-degradat.png)

Per reparar-ho es torna a connectar el disc.

![Reconnexió del disc a VirtualBox](captures/34-vm-reconnectar-disc.png)

Windows detecta el disc i el grup torna a l'estat **Aceptar**. Els hashes continuen sent els mateixos.

![Grup en estat correcte](captures/35-grup-correcte.png)

![Hash dels fitxers després de la reparació](captures/36-hash-restaurat.png)

Les propietats de l'espai mostren el tipus de resistència (**Reflejo doble**), l'aprovisionament **Fino**, una mida de 9,50 GB i 3,00 GB usats de la capacitat del grup. L'estat és *Conectado, Aceptar* i els dos discos físics apareixen amb l'estat *Aceptar*.

![Propietats de l'espai de mirall doble](captures/37-propietats-espai-mirall.png)

Un cop acabada la prova, s'elimina l'espai de mirall doble per deixar lliures els discos i poder provar la paritat. Windows demana confirmació, perquè en eliminar l'espai es perden les dades del volum i no es poden recuperar. Per això les captures de l'espai s'han fet abans.

![Confirmació per eliminar l'espai de mirall doble](captures/38-confirmacio-eliminar-espai.png)

L'eliminació no esborra el grup ni els discos físics. El grup queda sense espais i amb els dos discos físics. A la seva pàgina hi ha les opcions de gestió: **Cambiar nombre**, **Optimizar** (reparteix les dades entre tots els discs del grup), **Agregar un espacio** i **Agregar un disco**.

![Grup sense espais, amb dos discos físics](captures/39-grup-sense-espais.png)

## 9. Espai amb paritat

La paritat necessita **tres discos**. Per això el disc que faltava s'afegeix ara al grup (**Agregar un disco**) i el grup queda amb els tres discos, tots en estat *Aceptar*.

![Discos físics del grup](captures/40-discos-fisics-grup.png)

Es crea un nou espai, `Espacio de almacenamiento paridad`, amb resistència **Paridad**. Se'n demanen 18 GB i Windows el mostra com a 19,0 GB, amb aprovisionament fi. A diferència del mirall, no duplica les dades: guarda la informació de paritat distribuïda entre els discos. Així tolera la fallada d'un disc amb més aprofitament de l'espai.

![Espai amb paritat de 19 GB](captures/41-espai-paritat.png)

Es formata com a `paridad (F:)` amb NTFS i apareix a l'explorador amb uns 18,9 GB lliures.

![Volum paridad (F:) a Aquest equip](captures/42-aquest-equip-paritat.png)

S'hi copien les dues imatges i es creen una carpeta i un fitxer de prova, per comprovar que s'hi pot escriure.

![Contingut del volum de paritat](captures/43-fitxers-paritat.png)

> **Observació:** a **Administració de discos**, el volum `F:` apareix com a *NTFS (Cifrado con BitLocker)*. Windows 11 xifra automàticament els volums nous amb BitLocker. No s'ha activat a mà.

## 10. Espai amb mirall triple

El mirall triple manté **tres còpies** de les dades i, segons la guia, necessita **cinc discos**. Cal afegir-ne dos més de 10 GB a la màquina.

![Detalls de la màquina abans d'afegir els discs 4 i 5](captures/44-vm-afegir-discs-4-5.png)

Igual que amb els primers, s'inicialitzen des d'**Administració de discos** amb MBR.

![Inicialització dels discs 4 i 5](captures/45-inicialitzar-discs-4-5.png)

Després s'afegeixen al grup des de **Agregar un disco**, amb la casella de propagar les dades marcada. L'assistent mostra la capacitat existent (28,4 GB) i la nova capacitat projectada. En acabar, el grup té **47,4 GB** i cinc discs de dades.

![Afegir discos al grup](captures/46-afegir-discs-grup.png)

Es crea un nou espai de 15 GB amb resistència **Reflejo triple**. L'assistent avisa que un mirall de tres vies escriu tres còpies de les dades, tolera dos errors de disc i **requereix almenys cinc discos físics al grup**.

![Nou espai amb mirall triple](captures/47-nou-espai-mirall-triple.png)

> **Incidència:** en aquest assistent es veu que l'**ús màxim del grup** és de 45,0 GB (15 GB × 3 còpies), mentre que la capacitat del grup és de 28,4 GB. Windows crea l'espai, que queda *Conectado, Aceptar*, però no deixa donar-li format des de Configuració: el botó de format surt en gris i l'espai queda sense volum.

L'intent de crear el volum des de Configuració, amb l'etiqueta `mirall triple`, tampoc acaba de funcionar:

![Intent de crear el volum des de Configuració](captures/48-nou-volum-mirall-triple.png)

Un cop creat, el grup té 47,4 GB i en té 9,75 GB usats.

![Grup de 47,4 GB](captures/49-grup-47gb.png)

La llista d'espais mostra els dos tipus: `mirall triple` (*Reflejo triple*, 15,0 GB, sense volums) i `paridad` (*Paridad*, 19,0 GB).

![Espais del grup](captures/50-espais-del-grup.png)

Les propietats de l'espai mostren el tipus de resistència *Reflejo triple*, l'identificador de disc 7 i el botó **Eliminar**.

![Propietats de l'espai de mirall triple](captures/51-propietats-mirall-triple.png)

La solució és donar-li format des de **Administració de discos**. Allà, l'espai apareix com el Disc 7, de 14,98 GB, sense assignar.

![Disc 7 sense assignar](captures/52-admin-discs-sense-assignar.png)

![Disc de paritat i disc 7 a Administració de discos](captures/53-admin-discs-paritat.png)

Es crea un **nou volum simple** amb la lletra `G:` i NTFS. Surt com a `Nuevo vol (G:)`.

![Volum G: acabat de formatar](captures/54-admin-discs-nou-volum.png)

Després se li canvia l'etiqueta a `Mirall triple`.

![Volum Mirall triple (G:)](captures/55-admin-discs-mirall-triple.png)

No s'ha fet cap prova de fallada de disc amb el mirall triple.

## 11. Gestió i estat des de Windows

L'estat dels discs i del grup es pot veure des de tres llocs:

- **Configuració → Espais d'emmagatzematge:** estat del grup, dels espais i dels discs físics (*Aceptar*, *Advertencia*, *Degradado*).
- **Administració de discos:** discs virtuals, volums i lletres d'unitat.
- **PowerShell:** comandes que es poden automatitzar.

Les comandes usades són aquestes:

```powershell
Get-StoragePool -FriendlyName "grupo de almacenamiento tasca"
Get-PhysicalDisk | Select FriendlyName, Size, HealthStatus, OperationalStatus
Get-VirtualDisk | Select FriendlyName, ResiliencySettingName, Size, HealthStatus
```

- `Get-StoragePool` mostra el grup: està en estat `OK` i `Healthy`, amb 47,49 GB de mida i 13,5 GB assignats.
- `Get-PhysicalDisk` llista els discs físics: el de sistema (64 GB) i els cinc de 10 GB, tots `Healthy`. El disc del sistema no pertany al grup. Per veure només els discs que es poden agrupar es pot afegir `-CanPool $true`.
- `Get-VirtualDisk` llista els espais i el tipus de resistència de cadascun: `paridad` surt com a `Parity` i `mirall triple` com a `Mirror`, perquè el mirall triple és un tipus de mirall amb tres còpies.

![Estat del grup, dels discs i dels espais amb PowerShell](captures/56-powershell-estat.png)

> **Incidència:** a la tercera comanda es va escriure `Syze` en lloc de `Size`. PowerShell no dona error, però la columna de la mida surt buida perquè aquesta propietat no existeix. Amb `Size` es mostra la mida de cada espai.

## 12. Comparació de configuracions

| Tipus | Discs mínims | Còpies | Espai útil aproximat | Tolera |
|---|---|---|---|---|
| Simple | 1 | 1 | 100 % | Cap fallada |
| Mirall doble | 2 | 2 | 50 % | 1 disc |
| Paritat | 3 | — | 67 % amb 3 discs | 1 disc |
| Mirall triple | 5 | 3 | 33 % | 2 discs |

- El **mirall doble** és el més senzill, però només aprofita la meitat de l'espai.
- La **paritat** aprofita més espai que el mirall. Amb tres discs, dos terços són per a dades i un terç per a paritat. A canvi, escriure és més lent, perquè cal calcular la paritat.
- El **mirall triple** és el que més protegeix, però només es pot usar un terç de la capacitat.

## 13. Limitacions i observacions

- **Mida màxima del mirall triple.** Un espai de 15 GB amb mirall triple pot arribar a ocupar 45 GB del grup (15 GB × 3 còpies). Quan es va crear, el grup tenia 28,4 GB, de manera que Configuració va deixar el botó de format en gris. Cal dimensionar l'espai segons les còpies: amb tres còpies, la mida de l'espai hauria de ser com a màxim un terç de la capacitat del grup, o bé cal haver afegit abans tots els discs.
- **Cinc discos pel mirall triple.** El mirall triple exigeix almenys cinc discos físics al grup. Per això es van afegir els discos 4 i 5 abans de crear-lo.
- **Format des d'Administració de discos.** Quan Configuració no deixa donar format a l'espai, es pot fer des d'**Administració de discos** creant un volum simple sobre el disc virtual.
- **BitLocker automàtic.** Els volums nous (`F:` i `G:`) apareixen com a *NTFS (Cifrado con BitLocker)*. Windows 11 els xifra sense que ho demanem. En un servidor real, convé guardar la clau de recuperació.
- **Eliminar un espai esborra les dades.** Abans d'eliminar l'espai de mirall doble es van fer les captures i la prova d'integritat, perquè no es poden recuperar. L'eliminació no afecta el grup ni els discos.
- **Aprovisionament fi.** Els espais són *Fino*: l'espai que ocupen al grup depèn de les dades que contenen, no de la mida declarada.
- **Prova de fallada.** Només s'ha fet amb el mirall doble. La paritat i el mirall triple s'han creat i formatat, però no s'ha desconnectat cap disc.

## 14. Resum de la part de Windows

| Apartat de l'enunciat | Resultat |
|---|---|
| Grup d'emmagatzematge amb tres discos de 10 GB | Grup `tasca` creat primer amb els discos 1 i 2 i ampliat després amb el tercer |
| Mirall (dos discos) | `tasca (E:)`, 9,5 GB, *Reflejo doble*; en desconnectar un disc el grup passa a *Degradado*, les dades es conserven i en reconnectar-lo torna a *Aceptar* |
| Paritat (tres discos) | `paridad (F:)`, 19 GB, *Paridad*, aprovisionament fi |
| Mirall triple (cinc discos) | `Mirall triple (G:)`, 15 GB, *Reflejo triple*, amb els discos 4 i 5 afegits al grup |
| Gestió des de Windows | Configuració, Administració de discos i PowerShell |

### Notes

- Els hashes SHA256 dels fitxers són idèntics abans, durant i després de la fallada del disc. Això demostra que les dades es mantenen íntegres.
- El grup acaba amb 47,4 GB de capacitat i cinc discs de 10 GB, més el disc de sistema de 64 GB.
- Les captures són pròpies i es van fer el 17 i el 23 de novembre de 2026.
