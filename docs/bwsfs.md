---
title: bwSFS
subtitle: Storage for Science Baden Württemberg
---

![bwSFS – Storage for Science](/img/bwsfs-logo.png)

# Storage for Science Baden Württemberg

Ein nachhaltiges Forschungsdatenmanagement (FDM) benötigt ausreichend Ressourcen mit einer gesicherten Perspektive für verstetigten Betrieb. Über das bwSFS wird die Hardware mit darauf aufsetzenden Basisdiensten bereitgestellt, die von den antragstellenden Kern-Communities der HPC-Cluster NEMO und BinAC sowie den beteiligten Universitäten Freiburg und Tübingen und den Projekten [BioDATEN](https://portal.biodaten.info) und [DataPLANT](https://nfdi4plants.de) genutzt werden.

Die Finanzierung von bwSFS erfolgt durch die DFG und das Land Baden-Württemberg. Der Anteil der DFG ist für die Verarbeitung von Daten in laufenden Forschungsprojekten vorgesehen, während der Landesanteil der Unterstüzung der Langzeitablage und -bereitstellung dient. Die kooperierenden Universitäten verfolgen ihr strategisches Ziel, als forschungsstarke und moderne Forschungseinrichtungen die nötigen Infrastrukturen für Langzeitarchivierung und Datenpublikation zu bieten.

## Ziele

bwSFS ist als technischer Kern gedacht, der von den Communities der HPC-Cluster NEMO (Elementarteilchenphysik, Neurowissenschaften, Materialwissenschaften, Microsystemtechnik) und BinAC (Bioinformatik, med. Informatik, Pharmazie, Geowissenschaften, Astrophysik), den zwei Universitäten und den Forschungsverbünden aus dem Bereich der Bioinformatik genutzt werden soll. Mit ihm sollen die Ansprüche an ein nachhaltiges Forschungsdatenmanagement einlösbar werden. Den Universitäten Freiburg und Tübingen hilft bwSFS, Forschungsstärke bei IT-gestützten Wissenschaftsfeldern zu signalisieren. Eine weitere Zielgruppe sind Forschende, die für ihre Daten einen generischen, öffentlich erreichbaren Speicherort benötigen, mit dem sie Anforderungen an Open-Science umsetzen können. Mitgliedern der Universitäten wird die Publikation ihrer Daten im Sinne von Open-Access sowie die Speicherung von Daten für Projekte ermöglicht, die auch nach Ablauf von Förderungen nach den FAIR-Prinzipien zur Nachnutzbarkeit offen stehen.

## Betriebsmodell

Bei bwSFS ist das Betriebsmodell weitergeführt, das in Baden-Württemberg bei vorherigen Projekten wie dem bwForCluster NEMO entwickelt wurde. Dieses Modell sieht vor, für den Betrieb technische Schichten anzunehmen, die für sich organisiert werden können. Das ist ein gängiges Prinzip, Arbeitsteilung über Internettechnologien zu realisieren. Durch die Schichtung können Aufgaben zwischen den beteiligten Rechenzentren und den Konsortien SDC BioDATEN und DataPLANT verteilt werden. Das Modell ist selbst Gegenstand von Fachdiskussionen, die sich mit Aspekten des [Managements](http://archiv.ub.uni-heidelberg.de/volltextserver/id/eprint/29643), der [Risikosteuerung und der Betriebssicherheit](https://freidok.uni-freiburg.de/data/218104) bei wissenschaftsorientierten IT-Diensten beschäftigen.

Für die Nutzer:innen werden Dienste entwickelt, die wichtige Schritte im Lebenszyklus von Forschungsdaten abdecken:

- Generierung von Daten durch Forschungsgroßgeräte (hochauflösende Mikroskope, Spektrometer, Sequencer, …) und Ablage zur Weiterverarbeitung
- Verarbeitung und Zwischenspeicherung während der Forschungsphase
- Publikation mit persistenten Identifikatoren
- Archivierung mit und ohne Zugriffsregelung

Das Betriebsmodell ist darauf ausgelegt, offen für die Beteiligung von Einrichtungen und Professuren zu sein. Diese Beteiligung kann auch aus Drittmitteln stammen, die eine Verausgabung in Hardwarekomponenten wie Speicher und Compute oder als IT-Service erlauben. Unter dem Schlagwort „Aufwuchsfinanzierung“ ist die Einbringung von Drittmitteln für Hardware in bwSFS vorbereitet. bwSFS ist an den beiden Standorten Freiburg und Tübingen in Maschinensälen untergebracht, in denen Maschinenschränke für die Aufnahme von Hardware vorgehalten werden. Die Einwerbung von Mitteln ist so möglich, ohne in fachfremde Infrastrukturplanungen einsteigen zu müssen. Die Hardware kann maschinennah bedient werden oder über aufsetzende Dienste, die Forschende von den Aufgaben der reinen IT-Administration entlasten. Auch die eigentliche Einkauf der Hardware wird durch die Rechenzentren organisiert, so dass der Serverpark möglichst homogen und aktuell gehalten wird.

## Dienste für Communities

Für die Fachcommunities werden  drei Dienste auf bwSFS vorbereitet, die teilweise im Self-Service-Verfahren genutzt werden können:

- InvenioRDM
- Gitlab
- OMERO

[InvenioRDM](https://inveniosoftware.org/products/rdm/) ist ein Software-Framework für die Publikation von Texten und Forschungsdaten, das im Zuge der Etablierung von Zenodo entwickelt wurde. Es ist Open-Source und kann selbst gehostet werden. Mitarbeiter:innen der beteiligten Rechenzentren engagieren sich in der Anwendungs-Community.

Für die beteiligten Universitäten und Konsortien werden dedizierte Instanzen von InvenioRDM eingerichtet. Sie sind mit den Kennungen der Heimeinrichtungen und Elixir-AAI zugänglich. Die Einbindung von [ORCID](https://orcid.org) sowohl als Authentifizierungsmechanismus als auch als automatisierten Publikationsnachweis für Forschungsdaten ist ebenso geplant.

InvenioRDM dient zur Publikation von Forschungsdaten, die sich als Entwurf hochladen lassen, bevor eine DOI mit der Stammnummer der angeschlossenen Universitätsbibliotheken beantragt wird. Für Daten kann eine Embargofrist mit selbst einstellbarer Dauer konfiguriert werden. Weil jedem Datensatz eine eigene DOI zuweisbar ist, lassen sich Versionen einzeln referenzieren, aber auch Stammnummern einrichten, die den jeweils aktuellsten Stand zurückliefern. Es ist auch vorgesehen, für Rohdaten und Metadaten unterschiedliche Zugangsregeln und Embargofristen einstellen zu können.

Die Nachverfolgung von Versionen ist zentrales Feature der Gitlab-Instanz auf bwSFS. Git hat sich im Bereich der Softwareentwicklung als Standard für das gemeinsame, verteilte Arbeiten und Versionieren von Code etabliert. Seine Vorteile sind ebenso für verteiltes und kollaboratives Arbeiten an Dateien in der Forschung nutzbar. Die Instanz kann in Zusammenarbeit mit einem AAI-Proxy verschiedene IDM-Systeme, wie die der beteiligten Universitäten, Elixir AAI benutzen. Die in GitLab hinterlegten Daten sind damit eindeutig zugeordnet. Forschende können weitere Forschende selbständig einladen und Gruppen aufbauen, die an einem Projekt arbeiten.

Als weiterer Dienst wird [OMERO](https://www.openmicroscopy.org/omero/) für die Bio-Imaging Community gehostet. OMERO ist eine Bilddatenbank, die von Wissenschaftfeldern mit hohem Anteil an bildverarbeitenden Methoden genutzt wird. Hochauflösende Bilder, erzeugt von Geräten wie Mikroskopen, sind speicherhungrig und benötigen schnelle Datenverbindungen. Die OMERO-Instanz auf bwSFS wird als Backbone für solche Wissenschaftsfelder angeboten, die diesen Dienst benötigen, ihn aber nicht selbst unterhalten möchten.

## Kunden

bwSFS ist eine Plattform, die Kund:innen auf mehreren Ebenen bedient. Primär werden die mitantragstellenden HPC-Communities für die aktiven Phasen des Datenlebenszyklusses und Mitglieder der beteiligten Universitäten und Konsortien adressiert, die Forschungsdaten ablegen wollen. Gründe für die Nutzung eines Repository an der eigenen Hochschule/Universität für die Publikation können sein:

- Mitglieder einer Forschungsgruppe gehören alle zur gleichen Universität. Es ist bei lokalen Diensten einfacher, Zugänge zu Netzwerkfreigaben einzurichten, die mit dem IDM der eigenen Einrichtung verknüpft sind.
- In der Phase, wo die Daten gewonnen und verarbeitet wurden, ist ein lokales Speichersystem der eigenen Hochschule von Vorteil, weil es eine schnelle Datenverbindung in dem Bereich hat, welcher dem Sicherheitsmanagement der Universität unterliegt.
- Die Policies der eigenen Universität schreiben bei bestimmten Projekten vor, Dienste der eigenen Hochschule zu verwenden.
- Aus Gründen des Datenschutzes müssen Daten zwingend auf einem System gespeichert werden, das unter der Kontrolle der eigenen Institution steht.

bwSFS als Speicherbasis ist für Datensets ein möglicher Ort, die auf anderen Systemen organisch gewachsen sind und dort wegen ihrer Komplexität im Betrieb an Grenzen stoßen, deren wissenschaftlicher Wert aber eine weitere Bereitstellung empfiehlt. bwSFS bietet solchen Projekten professionell betriebenen Speicher mit Zugriffsprotokollen, über die sich Workflows und Kuratierungsmethoden in den Phasen im Lebenszyklus von Forschungsdaten umsetzen lassen.

## Kooperation

Das Betriebsmodell mit seiner Schichtung von Diensten schafft die Basis für die Beteiligung zweier Universitäten und zweier Konsortien auf Landes- und Bundesebene. bwSFS  ermöglicht über die angebundenen IDM-Systeme Kooperation ebenso zwischen den Nutzer:innen. Sie können Speicher gemeinsam verwalten. Kooperationen sind möglich zwischen folgenden Partnern:

- Beteiligten des Science Data Center BioDATEN
- innerhalb des  NFDI-Konsortiums DataPLANT
- innerhalb der [BW-data.federation](https://www.alwr-bw.de/typo3temp/secure_downloads/114142/0/54ceb21cbc883949ddc7c7703738f7f5a717e4f3/bwDATA_III__1_001.pdf)

Der Fokus von bwSFS liegt darauf, auf diesen Ebenen und an den Universitäten Freiburg und Tübingen  ein niedrigschwelliges Angebot für Wissenschaftler:innen aus allen Fachbereichen zu schaffen.

## Forschungsdatenmanagement jenseits von Leuchttürmen

Als konkreter Dienst mit Abstraktion vom technischen Betrieb und der Nutzung vorhandener IDM-Systeme sollen übliche Hürden bei der Umsetzung von FDM beseitigt werden. Damit werden einige Kernprobleme angegangen:

- Zeitlich befristete Projekte können ihre Daten in Übereinstimmung mit Leitlinien wie den „[Leitlinien zur Sicherung guter wissenschaftlicher Praxis (Kodex)](https://www.dfg.de/foerderung/grundlagen_rahmenbedingungen/gwp/)“ nachhaltig speichern, auch nach Ende der Projektlaufzeit.
- Mit der Einführung von Diensten als Self-Service soll der Druck beseitigt werden, auf dem Markt geeignetes Fachpersonal für IT-Administration zu suchen oder Dienste zu nutzen, mit denen die Einhaltung gesetzlicher Bestimmungen wie beispielsweise für den Datenschutz kaum einzuhalten sind.
- Es sollen Angebote auch für Fachbereiche existieren, in denen ein dem Stand der Technik entsprechendes FDM bislang nicht verbreitet ist.
