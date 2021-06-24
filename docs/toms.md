---
title: Technisch Organisatorische Maßnahmen
---

--8<-- "includes/abbr.md"

# Technisch Organisatorische Maßnahmen

|            |            |
| ---------- | ---------- |
| Gültig ab: |            |
| Version:   | 0.4.1      |
| Datum:     | 21.06.2021 |

<!-- !!! error  "TLP:RED"
    Not for disclosure, restricted to participants only. Distribution outside this audience requires written permission from the originator. -->

!!! attention "TLP:AMBER"
    Limited disclosure, restricted to participants' organizations.
    Distribution outside this audience requires written permission from the originator.

<!-- !!! important "TLP:GREEN"
    Limited disclosure, restricted to the community/sector. Distribution outside this audience requires written permission from the originator. -->

<!-- !!! note "TLP:WHITE"
    Disclosure not limited. -->

Version | Datum      | Autor*innen     | Änderungen
------- | ---------- | --------------- | ----------
0.4.1   | 21.06.2021 | MJ              | Buchstabendreher und Anführungszeichen korrigiert. eduPersonPrincipalName statt EPPN und eduPersonEntitlement statt Entitlement verwendet.
0.4.0   | 26.05.2021 | MJ,JL           | Einleitung gelöscht. Text entspricht Passagen aus ADV-Vertragsentwurf.
0.3.0   | 04.05.2021 | MJ,JL           | Einleitung besteht aus Textpassagen aus dem ADV-Vertragsentwurf von JL. Zusätzliche Korrekturen von JL eingearbeitet. Fußnoten vervollständigt.
0.2.0   | 22.04.2021 | MJ              | Kleinere Korrekturen, Anpassungen und Entfernen von Redundanzen.
0.1.0   | 20.04.2021 | MJ              | Erster Entwurf basierend auf Version 0.3.2 des Betriebskonzepts NEMO.


## Vertraulichkeit

### Zutrittskontrolle

Die Zutrittskontrolle ist über die Maschinensaalbenutzungsordnung (MsBO)
festgelegt[^1]. Diese regelt folgende Aspekte:

- Zugang ist nur für berechtigte Personen mittels Transponder möglich.
- Die Vergabe der Schlüssel und Transponder ist dokumentiert.
- MSII ist alarmgesichert gegen Zutritt unbefugter Personen.
- MSII verfügt über Rauchmelder.
- Während der Öffnungszeiten ist Zutritt in MSII nur über eine
  Magnetkarte oder einen Schlüssel möglich.
- Außerhalb der Öffnungszeiten wird das Rechenzentrum regelmäßig von
  einem Sicherheitsdienst kontrolliert.
- Zutritt für Wartungen durch externe Parteien wird dokumentiert.

### Zugangskontrolle

Die Zugangskontrolle zum bwForCluster NEMO erfolgt über mehrere Stufen.
Das genauen Verfahren ist im bwHPC-Wiki beschrieben[^2]. Die genauen
Maßnahmen beinhalten:

- Wissenschaftler*innen müssen von der Universität berechtigt sein,
  die Ressource zu nutzen. Hierzu wird von den Universitäten das
  "Entitlement" `http://bwidm.de/entitlement/bwForCluster`
  vergeben[^3].
- Wissenschaftler*innen müssen einen Projektantrag stellen
  (Rechenvorhaben) oder sich einem bestehendem Projekt zuordnen[^4].
- Wissenschaftler*innen müssen sich beim Cluster registrieren, dazu
  werden Daten beim jeweiligen Identity-Provider (IdP) abgefragt[^5].
  Diese Daten können auch jederzeit von den Nutzer*innen abgefragt
  werden[^6]. Hierzu gehören:
  - Name
  - E-Mail-Adresse
  - `eduPersonPrincipalName` (EPPN)[^7]
  - Berechtigungen über "Entitlements" (`eduPersonEntitlement`).
  - Eine Unix-Gruppe, falls diese vom IdP ausgeliefert wird.
- Der Zugang ist über eine Firewall auf Belwü-Netze beschränkt[^8].
  - Außerhalb dieser Netze ist der Zugriff nur über Jump-Hosts
    (beispielsweise Proxy-Jump) oder VPN möglich.
- Wissenschaftler*innen haben Zugriff von außen über SSH mit
  Nutzername und Dienst-Passwort beziehungsweise SSH-Schlüssel auf
  Login- und Visualisierungsknoten.
- Zugang über das Openstack-Dashboard über HTTPS mit Nutzername und
  Dienst-Passwort.
- Administrator*innen haben nur mit Nutzername und SSH-Schlüssel
  Zugriff.
- Eine Zwei-Faktor-Authentifizierung wird derzeit ausgerollt.
- Der Zugang wird über Log-Dateien lokal und im Monitoring-Server
  protokolliert.

### Zugriffskontrolle

Die Zugriffskontrolle erfolgt auf Ebene von Unix-Rechten und "Access
Control Lists" (ACL). Folgende Regeln gelten:

- Wissenschaftler*innen haben nur auf ihre eigenen Daten Zugriff.
  Diese Berechtigungen können selbst für weitere Nutzer*innen,
  beispielsweise über ACLs, erweitert werden.
- Administratoren haben auf alle Daten und Bereiche Zugriff, wenn sie
  nicht von Client- oder Nutzerseite verschlüsselt werden.
- Externe Supportmitarbeiter haben nur Zugriff auf die von ihnen zu
  wartenden Komponenten.
- Testsysteme werden nur ausgewählten Personengruppen zur Verfügung
  gestellt und beeinträchtigen nicht den Produktivbetrieb.
- Zentrales Benutzer-/Berechtigungs-Management folgt dem
  Need-to-have-Prinzip für Wissenschaftler*innen, externen Support,
  Projektmitarbeiter*innen im bwHPC, Testnutzer*innen und
  Administrator*innen.
- Die Wissenschafler*innen verlieren ihren Zugriff, sobald dieser von
  der Universität entzogen wird beziehungsweise der Account nicht mehr
  gültig ist. Die Regeln, wann dies erfolgt, werden von den
  Universitäten festgelegt.
- Weitere zusätzliche Rechte, beispielsweise Administratorrechte,
  werden manuell entzogen. Derzeit wird ein Protokoll für die
  Administration entwickelt, das diesen Aspekt regelt. Die Einführung
  des Protokolls zum Ein- beziehungsweise Austritt von
  Administrator*innen ist für den Start des Nachfolgeclusters 2022
  geplant.

### Trennungskontrolle

Die Trennungskontrolle gewährleistet, dass zu unterschiedlichen Zwecken
erhobene Daten getrennt verarbeitet werden können. Hierzu zählen
folgende Maßnahmen:

- Physikalische und logische Trennung von Diensten, die nicht
  unmittelbar miteinander in Bezug stehen.
- Physikalische und logische Trennung von Diensten und Netzen, die
  nicht aufeinander zugreifen müssen.

## Integrität

### Weitergabekontrolle

Als Weitergabekontrolle werden Maßnahmen bezeichnet, die ein unbefugtes
Lesen, Kopieren, Verändern oder Entfernen bei elektronischer Übertragung
oder Transport verhindern. Das bwForCluster NEMO ist nur über folgende
Protokolle und Wege erreichbar:

- Zugriff auf das Cluster ist auf das Belwü-Netz beschränkt.
- Außerhalb des Belwü-Netzes muss VPN oder ein Jump-Host im Belwü
  verwendet werden.
- Zugriffe werden auf Serverseite protokolliert.
- Zugriff kann nur über verschlüsselte Dienste wie SSH und HTTPS
  erfolgen.
- Die lokale Festplatte der Rechenknoten wird beim Booten
  verschlüsselt und kann nach Entfernen nicht ausgelesen werden.
- Die Platten im Parallelspeicher enthalten nur Teile von Blöcken und
  eine Rekonstruktion ist nur möglich, wenn Teile entwendet werden,
  aus denen Daten ausreichend vollständig zusammengesetzt werden
  können. Jedoch ist für das Nachfolgesystem evtl. eine
  Verschlüsselung geplant.

## Verfügbarkeit, Belastbarkeit

### Verfügbarkeitskontrolle

Zur Verfügbarkeitskontrolle zählen Maßnahmen, die eine zufällige
Zerstörung oder Verlust von Daten und die Nutzbarkeit der
Rechenressourcen beschreiben. Das bwForCluster NEMO implementiert
folgende Schutzmaßnahmen:

- Feuer- und Rauchmeldeanlagen in MSII und Infrastrukturräumen wie
  Kühlung und Strom.
- Redundante Kühlung bis zur Abschaltung für Cluster-kritische Dienste
  wie Parallelspeicher, HOME-Speicher und Server für Dienste.
  Rechenknoten sind nicht geschützt, können aber nach Behebung der
  Störung sofort wieder hochgefahren werden. Da im Desasterfall der
  Parallelspeicher und die Dienste vermutlich nicht benötigt werden,
  können diese unter Umständen vorsichtshalber sicher herunter
  gefahren werden.
- Für Server und Speicher des bwForClusters NEMO besteht
  unterbrechungsfreie Stromversorgung sowie Notstromversorgung.
- Die Temperatur, Feuchtigkeit und Stromverbrauch der Maschinen und
  der Datenschränke werden überwacht.
- Das HOME-Verzeichnis der Wissenschaftler*innen ist georedundant
  gespeichert und bietet automatische Snapshots[^9].
- Der Parallelspeicher ist mit RAID-6 abgesichert.
- Der Zugriff auf die Login- und Vis-Knoten ist durch eine Firewall
  und Fail2ban gesichert. Der Zugriff auf das Cloud-Dashboard über
  eine Firewall abgesichert. Die restlichen Komponenten sind nur
  Cluster-intern erreichbar. Ausfälle durch Angriffe externer Parteien
  können so minimiert werden.
- Der Zugriff auf das bwForCluster NEMO ist nur Wissenschaftler*innen
  aus baden-württembergischen Universitäten, wenigen
  Administrator*innen und Support-Mitarbeitern erlaubt, was den
  Angriffsvektor zusätzlich verkleinert.
- Die Verfügbarkeit des bwForClusters NEMO wird überwacht.
- Das Wiederanfahren des Systems kann nach "Ausfällen ohne
  Datenverlust" innerhalb weniger Stunden erfolgen.

## Regelmäßige Überprüfung, Bewertung, Evaluation

### Datenschutz- und Informationssicherheits-Management

Die Universität Freiburg nimmt den Schutz der ihr anvertrauten
personenbezogenen Daten sehr ernst und behandelt diese vertraulich und
entsprechend der gesetzlichen Vorschriften. Neben den Regelungen der
Europäischen Datenschutz-Grundverordnung (DSGVO) richtet sich die
Verarbeitung personenbezogener Daten an der Universität nach dem
Landesdatenschutzgesetz (LDSG) sowie den einschlägigen Regelungen des
Landeshochschulgesetzes (LHG).

Die Datenschutzbeauftragte Person der Universität Freiburg kann unter
der E-Mail-Adresse

```
datenschutzbeauftragter@uni-freiburg.de
```

sowie unter der Postadresse der Universität mit dem Zusatz "Der
Datenschutzbeauftragte" erreicht werden. Allgemeinen Fragen zum Thema
Datenschutz können an die E-Mail-Adresse

```
datenschutz@uni-freiburg.de
```

gerichtet werden.

Dazu arbeitet die Universität weiterhin mit der Zentralen
Datenschutzstelle der baden-württembergischen Universitäten (ZENDAS)
zusammen.

### Incident-Response-Management

Das Incident-Response-Management unterstützt bei der Reaktion auf
Sicherheitsverletzungen. Hierzu zählen beim bwForCluster NEMO:

- Meldung von Sicherheitsvorfällen beim Sicherheitsbeauftragten und
  Datenschutzbeauftragten der Universität, bei den Projektpartnern im
  bwHPC und dem DFNCert.
- Das DFNCert untersucht Angriffe durch externe Parteien.

### Datenschutzfreundliche Voreinstellungen

Für die Registrierung beim bwForCluster NEMO werden nur so viele
personenbezogene Daten erhoben, wie für den Dienst notwendig sind, siehe
Abschnitt [Zugangskontrolle](#zugangskontrolle).

## Referenzen

[^1]: SCHULZ, Janne Chr., Dirk von SUCHODOLETZ, Ulrich GEHRING,
    Willibald MEYER und Jan LEENDERTSE, 2020.
    *Maschinensaalbenutzungsordnung des Rechenzentrums der Universität
    Freiburg: Richtlinien für das Hosting und Housing von Hardware in
    den Räumen desRechenzentrums der Universität Freiburg* \[online\].
    techreport. Rechenzentrum der Universität Freiburg. Verfügbar unter:
    <https://www.rz.uni-freiburg.de/inhalt/dokumente/pdfs/msbo>

[^2]: Registrierungsprozedur im zentralen HPC-Wiki:
    <https://wiki.bwhpc.de/e/BwForCluster_User_Access>, besucht am
    20.04.2021.

[^3]: Beschreibung des Attributs `eduPersonEntitlement`:
    <https://www.bwidm.de/attribute.php#Berechtigung>, besucht am
    04.05.2021.

[^4]: Hierzu werden an der "Zentralen Antragsseite" (ZAS) folgende
    Attribute abgefragt: <https://www.bwidm.de/dienste.php>, besucht am
    04.05.2021.

[^5]: Siehe Dienst bwForCluster für genaue Beschreibung der abgefragten
    Daten: <https://www.bwidm.de/dienste.php>, besucht am 04.05.2021.

[^6]: Wissenschaftler*innen aus Baden-Württemberg können sich
    registrieren und danach ihre gespeicherten Daten auf der
    Registrierungsseite überprüfen:
    <https://bwservices.uni-freiburg.de/user/index.xhtml>, besucht am
    20.04.2021.

[^7]: Beschreibung des Attributs `eduPersonPrincipalName`:
    <https://www.bwidm.de/attribute.php#Principal%20Name>, besucht am
    04.05.2021.

[^8]: Der Zugriff ist auf die IPv4-Prefixe des Belwü-Netzes beschränkt:
    <https://bgpview.io/asn/553>, besucht am 16.04.2021.

[^9]: Snapshots sind regelmäßige Speicherstände des HOME-Verzeichnisses
    zum Zeitpunkt der Aufnahme.
