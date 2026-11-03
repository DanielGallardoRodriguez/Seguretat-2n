
# NF1AA3 - Autenticació multifactor (MFA)

**Nom:** Daniel Gallardo  
**Cicle:** Sistemes Microinformàtics i Xarxes (SMX)  
**Mòdul:** Seguretat Informàtica  
**Activitat:** Configuració de Google Authenticator

---

## 1. Introducció

L'autenticació multifactor (MFA) és un sistema de seguretat que serveix per protegir els nostres comptes.

Normalment, per iniciar sessió utilitzem un correu electrònic i una contrasenya. Amb l'autenticació en dos passos afegim una segona comprovació per evitar que una altra persona pugui entrar al nostre compte encara que conegui la contrasenya.

En aquesta pràctica configurarem Google Authenticator al compte de Google del centre educatiu.

## 2. Instal·lació de Google Authenticator

Primer de tot, necessitem instal·lar l'aplicació Google Authenticator al nostre telèfon mòbil.

**Passos:**

1. Obrim la Play Store (Android) o l'App Store (iPhone).
2. Busquem l'aplicació **Google Authenticator**.
3. Premem el botó d'instal·lar.
4. Un cop instal·lada, obrim l'aplicació.

**Captura 1:** Aplicació Google Authenticator a la botiga d'aplicacions.



## 3. Accedir a la seguretat del compte

Ara hem d'entrar a la configuració de seguretat del nostre compte de Google.

**Passos:**

1. Obrim el navegador de l'ordinador.
2. Entrem a https://myaccount.google.com/.
3. Iniciem sessió amb el compte del centre educatiu (`mataro.epiaedu.cat`).
4. Anem a l'apartat **Seguretat i inici de sessió**.
5. Busquem la secció **Com inicies sessió a Google**.

**Captura 2:** Apartat de seguretat del compte de Google.



## 4. Activar la verificació en dos passos

Dins de la configuració de seguretat trobarem l'opció de verificació en dos passos.

**Passos:**

1. Seleccionem **Verificació en dos passos**.
2. Si encara no està activada, premem l'opció per activar-la.
3. Seguim les instruccions que ens mostra Google.
4. Confirmem la nostra identitat amb el mètode que ens demani.

Un cop acabat aquest procés, tindrem activada la verificació en dos passos.

**Captura 3:** Configuració de la verificació en dos passos.



## 5. Configurar Google Authenticator

Ara vincularem el nostre compte amb l'aplicació que hem instal·lat al mòbil.

**Passos:**

1. Dins de la verificació en dos passos, busquem l'opció **Aplicació Authenticator**.
2. Premem l'opció per configurar-la.
3. Google ens mostrarà un codi QR.
4. Obrim Google Authenticator al mòbil.
5. Premem el botó **+** i seleccionem **Escanejar un codi QR**.
6. Escanegem el codi que apareix a la pantalla de l'ordinador.

D'aquesta manera, el nostre compte quedarà vinculat a Google Authenticator.

**Captura 4:** Pantalla de configuració de l'aplicació Authenticator.



**Important:** No hem de compartir el codi QR amb ningú, ja que serveix per configurar l'accés al nostre compte.

## 6. Confirmar el codi de verificació

Quan hàgim escanejat el QR, l'aplicació començarà a generar codis de sis dígits.

**Passos:**

1. Mirem el codi que apareix a Google Authenticator.
2. Tornem a l'ordinador.
3. Introduïm el codi de sis dígits quan Google ens el demani.
4. Confirmem la configuració.

Aquests codis canvien automàticament cada pocs segons, per això hem d'introduir un codi que encara sigui vàlid.

**Captura 5:** Confirmació de Google Authenticator.



## 7. Codis de recuperació

Google també permet generar codis de recuperació per poder entrar al compte si algun dia perdem el mòbil o no podem utilitzar Google Authenticator.

**Passos:**

1. Tornem a l'apartat **Verificació en dos passos**.
2. Busquem l'opció **Codis de recuperació**.
3. Generem els codis.
4. Els descarreguem o els imprimim.
5. Els guardem en un lloc segur.

Normalment, Google genera 10 codis de recuperació de vuit dígits que es poden utilitzar una sola vegada cadascun.

**Captura 6:** Apartat dels codis de recuperació, sense mostrar els codis.



**Important:** No hem de compartir aquests codis ni guardar-los en un lloc accessible per altres persones.

## 8. Comprovar que funciona

Finalment, comprovarem que hem configurat correctament Google Authenticator.

**Passos:**

1. Obrim una finestra d'incògnit al navegador.
2. Entrem al nostre compte de Google.
3. Introduïm el correu i la contrasenya, si se'ns demanen.
4. Quan Google ens demani una segona verificació, seleccionem **Provar una altra manera**.
5. Triem l'opció d'utilitzar un codi de Google Authenticator, si està disponible.
6. Obrim l'aplicació al mòbil i introduïm el codi de sis dígits.

Si podem iniciar sessió correctament, haurem comprovat que Google Authenticator funciona.

**Captura 7:** Pantalla de verificació durant l'inici de sessió.



## 9. Conclusió

Amb aquesta pràctica hem après a configurar l'autenticació en dos passos al nostre compte de Google utilitzant Google Authenticator.

Aquest sistema ens ajuda a tenir el compte més protegit, perquè no n'hi ha prou amb saber la contrasenya per accedir-hi.

També hem vist la importància de guardar els codis de recuperació per si algun dia no podem utilitzar el mòbil.

## 10. Fonts consultades

- Google. Ajuda del compte de Google: https://support.google.com/accounts/answer/185839?hl=ca
- Google Authenticator: https://support.google.com/accounts/answer/1066447?hl=ca
- Configuració del compte de Google: https://myaccount.google.com/
