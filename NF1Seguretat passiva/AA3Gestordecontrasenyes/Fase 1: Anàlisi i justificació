# 0226NF1.AA3 — Gestor de contrasenyes

## Fase 1. Anàlisi i justificació

**Empresa:** EverPia  
**Mòdul:** Seguretat Informàtica  
**Eina seleccionada:** Bitwarden

---

## 1. Introducció

L'empresa EverPia ha patit un atac informàtic que ha provocat una filtració d'informació confidencial. Després d'investigar l'incident, s'ha descobert que un dels comptes tècnics tenia una contrasenya feble o reutilitzada.

Aquesta situació demostra la importància de protegir correctament els comptes d'usuari, sobretot en una empresa que treballa amb informació privada de clients i projectes.

Per evitar que es repeteixi un incident similar, es proposa utilitzar un gestor de contrasenyes que permeti crear i guardar credencials segures.

En aquest informe es comparen dues opcions, Bitwarden i KeePassXC, per determinar quina és més adequada per al personal tècnic d'EverPia.

## 2. Riscos de les contrasenyes febles o reutilitzades

Utilitzar contrasenyes senzilles o repetir-les en diferents serveis pot posar en perill la seguretat de l'empresa.

Alguns dels atacs més habituals són:

- **Atac de diccionari:** consisteix a provar paraules i contrasenyes habituals fins a trobar la correcta.
- **Força bruta:** consisteix a provar moltes combinacions de caràcters per intentar descobrir una contrasenya.
- **Credential stuffing:** consisteix a provar credencials filtrades d'altres pàgines per accedir a nous comptes.
- **Phishing:** consisteix a enganyar els usuaris mitjançant correus o pàgines falses per aconseguir les seves dades d'accés.

Per exemple, si un treballador utilitza la mateixa contrasenya per al correu electrònic i per a una aplicació interna, una filtració podria permetre als atacants accedir als dos serveis.

Això podria provocar el robatori d'informació, accessos no autoritzats i problemes econòmics per a l'empresa.

### 2.1. Per què utilitzar un gestor de contrasenyes?

Un gestor de contrasenyes és una aplicació que permet guardar les credencials de manera xifrada i gestionar-les des d'un únic lloc.

Els seus principals avantatges són:

- Generar contrasenyes llargues i aleatòries.
- Utilitzar una contrasenya diferent per a cada servei.
- Guardar les credencials de forma segura.
- Evitar haver de memoritzar moltes contrasenyes.
- Facilitar l'inici de sessió en diferents plataformes.

D'aquesta manera es redueix el risc de reutilitzar contrasenyes i es facilita una gestió més segura dels comptes.

Tot i això, també és important protegir la contrasenya mestra i activar l'autenticació en dos passos.

## 3. Comparativa tècnica: Bitwarden i KeePassXC

Per decidir quina eina és més adequada, es comparen dues alternatives amb models de funcionament diferents.

**Bitwarden** és un gestor que permet sincronitzar les contrasenyes al núvol, mentre que **KeePassXC** és un gestor d'escriptori que utilitza una base de dades local.

| Característica | Bitwarden | KeePassXC |
|---|---|---|
| Tipus | Online / Núvol | Offline / Escriptori |
| Emmagatzematge | Cofre xifrat sincronitzat al núvol | Fitxer local KDBX |
| Xifratge | Xifratge d'extrem a extrem | Base de dades xifrada |
| Sincronització | Automàtica | Manual o mitjançant eines externes |
| Connexió a Internet | Necessària per sincronitzar; disposa de certes funcions sense connexió | No és necessària per a l'ús local |
| Compatibilitat | Windows, Linux, macOS, Android, iOS i navegadors | Windows, Linux i macOS |
| Extensió del navegador | Sí | Sí, amb KeePassXC-Browser |
| Generador de contrasenyes | Sí | Sí |
| Còpies de seguretat | Exportació del cofre | Còpia del fitxer KDBX |
| Codi obert | Sí | Sí |
| Preu | Versió gratuïta i plans de pagament | Gratuït |
| Facilitat d'ús | Alta | Mitjana |

### 3.1. Bitwarden

Bitwarden permet guardar les contrasenyes en un cofre xifrat i sincronitzar-les automàticament entre diferents dispositius.

Utilitza xifratge d'extrem a extrem, de manera que les credencials es xifren abans d'enviar-se als servidors.

També disposa d'una extensió per al navegador que facilita l'emplenament automàtic dels formularis d'inici de sessió.

Té una versió gratuïta amb les funcions bàsiques i plans de pagament amb característiques addicionals, especialment útils per a empreses.

### 3.2. KeePassXC

KeePassXC és un gestor de contrasenyes gratuït i de codi obert que funciona principalment de manera local.

Les contrasenyes es guarden en un fitxer amb extensió `.kdbx`, protegit mitjançant xifratge.

El principal avantatge és que no necessita cap servei al núvol per funcionar.

A més, el fitxer es pot copiar a una memòria USB o traslladar a un altre ordinador, fet que facilita la portabilitat.

Tot i això, les còpies de seguretat i la sincronització entre equips s'han de gestionar per separat.

## 4. Avantatges i inconvenients

### 4.1. Bitwarden

**Avantatges:**

- Sincronització automàtica entre dispositius.
- Interfície senzilla i fàcil d'utilitzar.
- Accés des de diferents ordinadors i navegadors.
- Generador de contrasenyes segures.
- Emplenament automàtic amb l'extensió.
- Possibilitat d'utilitzar funcions de gestió empresarial.

**Inconvenients:**

- La sincronització depèn del servei al núvol.
- Algunes funcions empresarials són de pagament.
- Cal protegir molt bé la contrasenya mestra.
- Un dispositiu infectat podria comprometre les credencials si el cofre està desbloquejat.

### 4.2. KeePassXC

**Avantatges:**

- Funciona sense connexió a Internet.
- Les contrasenyes es poden mantenir únicament en un fitxer local.
- És totalment gratuït i de codi obert.
- Permet transportar la base de dades entre equips.
- No depèn d'un servidor extern per obrir la base de dades.

**Inconvenients:**

- No disposa de sincronització automàtica integrada al núvol.
- Les còpies de seguretat s'han de gestionar manualment.
- Si es perd el fitxer KDBX i no hi ha cap còpia, es poden perdre les credencials.
- Pot resultar menys còmode si s'utilitzen molts dispositius.

## 5. Seguretat, usabilitat i continuïtat del negoci

Des del punt de vista de la **seguretat**, les dues eines són bones opcions si s'utilitzen correctament.

KeePassXC permet tenir més control sobre la ubicació de les dades, mentre que Bitwarden facilita la gestió de contrasenyes mitjançant el núvol.

Pel que fa a la **usabilitat**, Bitwarden és més pràctic perquè es pot utilitzar fàcilment des del navegador i sincronitza les credencials entre dispositius.

Respecte a la **continuïtat del negoci**, Bitwarden permet recuperar l'accés al cofre des d'un altre dispositiu si un ordinador falla, sempre que el servei estigui disponible i es conservin els mètodes d'accés.

En canvi, amb KeePassXC és necessari disposar d'una còpia actualitzada del fitxer KDBX.

En tots dos casos és important establir una política de còpies de seguretat i controlar qui té accés a les credencials.

## 6. Recomanació final

Després de comparar les dues opcions, considero que **Bitwarden és la millor alternativa per al personal tècnic d'EverPia**.

El principal motiu és que permet gestionar les contrasenyes de manera senzilla i sincronitzar-les entre diferents dispositius.

A més, el generador de contrasenyes facilita la creació de credencials úniques i segures, evitant reutilitzar-les en diferents serveis.

KeePassXC també és una bona alternativa, sobretot en equips que treballen sense Internet o necessiten mantenir les dades localment. Tot i això, Bitwarden em sembla més pràctic per a una empresa on els tècnics han d'utilitzar diferents aplicacions i serveis web.

Per implementar-lo correctament a EverPia, també seria recomanable activar l'autenticació en dos passos, utilitzar un pla empresarial amb administració d'accessos i fer còpies de seguretat periòdiques.

## 7. Fonts consultades

- [INCIBE — Contrasenyes segures](https://www.incibe.es/ciudadania/tematicas/contrasenas-seguras)
- [INCIBE — Gestors de contrasenyes per a empreses](https://www.incibe.es/empresas/blog/gestores-de-contrasenas-que-son-y-como-pueden-mejorar-la-seguridad-de-las-empresas)
- [Bitwarden — Web oficial](https://bitwarden.com/)
- [Bitwarden — Documentació](https://bitwarden.com/help/)
- [KeePassXC — Web oficial](https://keepassxc.org/)
- [KeePassXC — Documentació](https://keepassxc.org/docs/)
