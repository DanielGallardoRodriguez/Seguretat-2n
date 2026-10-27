# NF1AA2 — Activitat: Selecció d'un SAI

## 1. Dispositius de l'empresa

Per fer l'estudi de TecnoGestió S.L., s'han seleccionat dispositius habituals d'un petit despatx.

| Quantitat | Dispositiu | Marca i model | Potència unitària |
|---:|---|---|---:|
| 4 | Ordinador de sobretaula | Dell OptiPlex Micro 7020 | 65 W |
| 4 | Monitor de 24 polzades | Dell P2425H | 11,7 W |
| 1 | Router | TP-Link Archer AX55 | 30 W estimats |
| 1 | Impressora multifunció làser | HP LaserJet Pro MFP 4102fdw | 510 W durant la impressió |

Els ordinadors, els monitors i el router es connectaran al SAI perquè són necessaris per continuar treballant durant una interrupció elèctrica.

La impressora multifunció no es connectarà a les sortides amb bateria del SAI. Pot tenir un consum elevat durant la impressió i provocar una sobrecàrrega. Es deixarà connectada a una presa elèctrica independent amb la protecció adequada.

## 2. Consum elèctric

Per estimar la potència aparent, s'utilitza un factor de potència aproximat de 0,9.

La fórmula és:

`VA = W / 0,9`

| Dispositiu | Quantitat | Potència unitària | Potència total | VA totals estimats |
|---|---:|---:|---:|---:|
| Dell OptiPlex Micro 7020 | 4 | 65 W | 260 W | 288,9 VA |
| Dell P2425H | 4 | 11,7 W | 46,8 W | 52 VA |
| TP-Link Archer AX55 | 1 | 30 W | 30 W | 33,3 VA |
| **Total** | | | **336,8 W** | **374,2 VA** |

Les potències dels ordinadors i dels monitors són valors de referència per dimensionar el SAI. El consum real pot variar segons l'ús i la configuració dels equips.

## 3. Càlcul de la potència necessària

La potència total dels dispositius que es protegiran és:

`P total = 260 + 46,8 + 30 = 336,8 W`

Afegim el marge de seguretat del 20%:

`P necessària = 336,8 × 1,20 = 404,16 W`

Calculem també la potència aparent amb el mateix marge:

`S necessària = 374,2 × 1,20 ≈ 449 VA`

Per tant, el SAI ha de proporcionar com a mínim **404,16 W i aproximadament 449 VA**, segons les estimacions utilitzades.

S'han estudiat models amb una capacitat superior per disposar de marge i buscar una autonomia adequada.

## 4. Comparació de tres models de SAI

### Opció 1: Salicru SPS ONE 1500 V2

- **Potència aparent:** 1500 VA.
- **Potència activa:** 900 W.
- **Tipus:** línia interactiva amb regulació automàtica de tensió (AVR).
- **Sortides:** 4 preses Schuko.
- **Preu consultat:** 166,99 €.
- **Distribuïdor:** [PcComponentes](https://www.pccomponentes.com/salicru-sps-one-1500va-v2-sai).
- **Fabricant:** [Salicru](https://www.salicru.com/catalog-en/sps-1500-one-1.html).

És una opció econòmica amb potència suficient per a la càrrega calculada. Cal comprovar l'autonomia real i el nombre de connexions necessàries.

### Opció 2: Eaton 5E Gen2 1600 USB

- **Potència aparent:** 1600 VA.
- **Potència activa:** 900 W.
- **Tipus:** línia interactiva.
- **Funcions:** regulació automàtica de tensió i gestió per USB.
- **Sortides:** 6 preses IEC C13.
- **Preu consultat:** 186,99 €.
- **Distribuïdor:** [PcComponentes](https://www.pccomponentes.com/eaton-5e-gen2-1600-usb-sai-de-linea-interactiva-1600va-900w-con-6-salidas-ac-tipo-c13-acoplador).
- **Fabricant:** [Eaton](https://www.eaton.com/es/es-es/skuPage.5E1600UI.html).

És una alternativa amb bona relació entre potència i preu. Cal comprovar que les connexions siguin compatibles amb els equips i que l'autonomia arribi als 10 minuts.

### Opció 3: CyberPower CP1600EPFCLCD

- **Potència aparent:** 1600 VA.
- **Potència activa:** 1000 W.
- **Tipus:** línia interactiva amb sortida sinusoidal pura.
- **Funcions:** regulació automàtica de tensió i pantalla LCD.
- **Sortides:** 6 preses Schuko.
- **Preu consultat:** 323,45 €.
- **Distribuïdor:** [PcComponentes](https://www.pccomponentes.com/cyberpower-cp1600epfclcd-sai-linea-interactiva-formato-torre-1600va-1000w-con-6-salidas-schuko).
- **Fabricant:** [CyberPower](https://www.cyberpower.com/eu/es/product/sku/cp1600epfclcd).

És el model amb més potència activa dels tres i ofereix una sortida sinusoidal pura. El seu principal inconvenient és el preu més elevat.

### Taula comparativa

| Model | Potència activa | Potència aparent | Preu consultat | Autonomia mínima de 10 minuts |
|---|---:|---:|---:|---|
| Salicru SPS ONE 1500 V2 | 900 W | 1500 VA | 166,99 € | Cal verificar-la |
| Eaton 5E Gen2 1600 USB | 900 W | 1600 VA | 186,99 € | Cal verificar-la |
| CyberPower CP1600EPFCLCD | 1000 W | 1600 VA | 323,45 € | Cal confirmar-la a la càrrega calculada |

L'autonomia depèn de la càrrega connectada i de l'estat de les bateries. Abans de comprar qualsevol model, cal consultar la corba d'autonomia del fabricant per comprovar que manté els equips en funcionament durant almenys 10 minuts.

## 5. Selecció del SAI més adequat

Escolliria el **CyberPower CP1600EPFCLCD**, sempre que es confirmi que proporciona almenys 10 minuts d'autonomia amb la càrrega prevista i que el pressupost de l'empresa ho permet.

Els motius principals són:

- Té una potència activa de 1000 W, superior als 404,16 W calculats amb marge de seguretat.
- Disposa de 1600 VA de potència aparent.
- La sortida sinusoidal pura és adequada per a molts equips informàtics.
- Inclou una pantalla LCD i funcions de regulació de tensió.
- Ofereix marge per a possibles ampliacions futures.

El seu inconvenient és el preu, superior al de les altres alternatives. Si l'empresa vol reduir la despesa, l'Eaton 5E Gen2 1600 USB pot ser una alternativa, sempre que es verifiquin l'autonomia i les connexions.

Cal tenir en compte que els nou dispositius que es volen protegir necessiten nou connexions. Cap dels tres models ofereix nou preses de bateria directament. Abans de la compra, s'hauria de comprovar la distribució de les connexions i utilitzar només accessoris compatibles amb les indicacions del fabricant.

## Conclusió

Per a TecnoGestió S.L., un SAI de 1500–1600 VA és una opció raonable segons els consums estimats. La decisió final s'ha de basar en la potència activa, la potència aparent, l'autonomia, les connexions i el pressupost disponible.

El CyberPower CP1600EPFCLCD ofereix bones prestacions, mentre que l'Eaton 5E Gen2 1600 USB pot ser una alternativa més econòmica. En tots dos casos, cal verificar l'autonomia mínima de 10 minuts a la càrrega real abans de prendre una decisió definitiva.

## Fonts consultades

- [Dell OptiPlex Micro 7020 — especificacions tècniques](https://www.dell.com/support/manuals/es-es/optiplex-7020-micro/optiplex-micro-7020-owners-manual/adaptador-de-alimentaci%C3%B3)
- [Dell P2425H — fitxa tècnica](https://www.delltechnologies.com/asset/en-us/products/electronics-and-accessories/technical-support/dell-24-monitor-p2425h-datasheet.pdf.external?ref=cpcl_p2425h-monitor-dellbuyercontentlayoutitems-worlds0_cta_primaryv2_downloadbrochure)
- [HP LaserJet Pro MFP 4102fdw](https://www.hp.com/es-es/shop/product.aspx?id=2Z624F&opt=B19&sel=PRN)
- [Salicru SPS ONE 1500 V2 — distribuïdor](https://www.pccomponentes.com/salicru-sps-one-1500va-v2-sai)
- [Eaton 5E Gen2 1600 USB — distribuïdor](https://www.pccomponentes.com/eaton-5e-gen2-1600-usb-sai-de-linea-interactiva-1600va-900w-con-6-salidas-ac-tipo-c13-acoplador)
- [CyberPower CP1600EPFCLCD — distribuïdor](https://www.pccomponentes.com/cyberpower-cp1600epfclcd-sai-linea-interactiva-formato-torre-1600va-1000w-con-6-salidas-schuko)
