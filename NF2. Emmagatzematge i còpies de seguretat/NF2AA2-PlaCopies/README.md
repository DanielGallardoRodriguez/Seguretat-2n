# DRP: Pla de còpies

**Mòdul:** Seguretat — NF2. Emmagatzematge i còpies de seguretat
**Cas:** Muntatges i Serveis Tècnics SL
**Alumne:** Daniel Gallardo Rodríguez

## Descripció

Aquesta activitat consisteix a dissenyar el pla de còpies de seguretat d'una petita empresa dedicada a la instal·lació i el manteniment d'equips industrials. L'empresa té un servidor de fitxers Ubuntu amb 420 GB de dades (projectes, bases de dades de comptabilitat i clients, i carpetes personals), 10 equips Windows 11 i una fibra de 600 Mbps.

El pla s'ha de ajustar a aquests requisits de recuperació:

- **RTO:** les dades de comptabilitat i clients han d'estar disponibles en menys de 4 hores.
- **RPO:** màxim 24 hores per a la majoria de dades i 4 hores per a comptabilitat i clients.
- **Retenció:** historial d'almenys un mes.

## Lliuraments

| Fase | Document | Contingut |
|---|---|---|
| Fase 1 | [Fase1-respostes-individuals.md](Fase1-respostes-individuals.md) | Respostes individuals: què copiar, periodicitat i tipus de còpia, mitjans i ubicació |
| Fase 2 | [Fase2-proposta-unificada.md](Fase2-proposta-unificada.md) | Proposta unificada amb l'esquema 3-2-1 |
| Fase 3 | [Fase3-proposta-final.md](Fase3-proposta-final.md) | Proposta final: dades, cronograma setmanal, mitjans i estratègia de recuperació |

## Resum de la proposta

| Aspecte | Decisió |
|---|---|
| Dades crítiques | Bases de dades de comptabilitat i clients, i documents de projectes |
| Bases de dades | Completa cada nit i diferencial cada 2 hores en horari laboral |
| Projectes i carpetes personals | Completa setmanal (diumenge) i incremental cada nit |
| Equips clients | Només la carpeta Documents, cap al servidor cada nit |
| Mitjà local | NAS amb RAID 1, en una sala diferent de la del servidor |
| Mitjà extern | Núvol a la Unió Europea, xifrat i amb versions |
| Retenció | 5 completes setmanals, 35 dies de còpies diàries i 3 mesos de completes mensuals al núvol |
| RPO | Menys de 2 hores per a les bases de dades |
| RTO | Menys d'1 hora des del NAS i unes 2–3 hores des del núvol |
| Proves | Restauració mensual i verificació setmanal |

## Notes

Les fases 2 i 3 estan pensades per fer-se en grup. Les he fet jo sol perquè no tinc company de grup, i per això no hi ha una discussió ni un consens amb altres persones.

## Ús de la IA

Aquesta activitat permet un ús parcial de la IA (semàfor taronja). He fet servir una eina d'IA com a suport per entendre l'enunciat i els conceptes (RPO, RTO, tipus de còpia i regla 3-2-1), per estructurar i redactar els documents i per donar-los format en Markdown. Els continguts els he llegit, revisat i adaptat, i els entenc.

