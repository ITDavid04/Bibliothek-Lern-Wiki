# LF9.2 – Dienste bereitstellen und betreiben

> **Zielgruppe:** Umschüler FISI, 2. Lehrjahr, Dualis-Institut Hamburg
> **Prüfungsrelevanz:** Schwerpunkt AP2, schriftlicher Bereich "Konzeption und Administration von IT-Systemen" (§ 21 FIAusbV: Inhalt und 90 Min. Dauer; § 24 FIAusbV: 10% Gewichtung); Grundlagen auch in AP1, sowie als Grundlage für projektbezogene Fragen im Fachgespräch, falls entsprechende Themen im Abschlussprojekt vorkommen.
> **Lernzeit:** Ca. 2–3 Stunden Lesen/Durcharbeiten, +2–3 Stunden Übungen (Diagnoseketten, Backup-/Restore-Szenarien, Zertifikatsprüfung), +1–2 Stunden Wiederholung mit Selbsttest
> **Status:** Final
> **Stand:** 2026-09-14
>
> Die 🔴/🟡/🟢-Markierungen in diesem Artikel sind eine didaktische Einschätzung zur Prüfungsvorbereitung, keine offizielle IHK-Gewichtung.
>
> **Hinweis zur Abgrenzung:** LF9.1 (Netze planen und adressieren) legt die Netzwerk-Grundlage (Adressierung, VLAN, Routing, Firewall) – dieser Artikel baut direkt darauf auf und behandelt die Dienste, die auf dieser Infrastruktur laufen. Grundlagen zu Anforderungsanalyse und Requirements Engineering wurden bereits in LF5.1 behandelt.

---

## IHK-Kernfragen dieses Artikels

| # | Frage | Abschnitt |
| --- | --- | --- |
| 1 | Wie hängen DNS, DHCP und NTP zusammen, und warum reicht "Dienst läuft" nicht als Test? | [→ 1. DNS/DHCP/NTP](#1-dns-dhcp-und-ntp) |
| 2 | Wie werden Plattform, Konten und Rechte nach Least Privilege sicher gestaltet? | [→ 2. Server & Berechtigungen](#2-serverbetriebssysteme-und-berechtigungen) |
| 3 | Wie vergleicht und testet man Web-, Datei-, Druck- und Verzeichnisdienste? | [→ 3. Anwendungsdienste](#3-web--datei--druck--und-verzeichnisdienste) |
| 4 | Wie funktionieren VPN, TLS und Zertifikatsprüfung, und warum reicht "HTTPS funktioniert" nicht? | [→ 4. VPN/TLS/Zertifikate](#4-vpn-remote-access-tls-und-zertifikate) |
| 5 | Wie werden Monitoring, Backup/Restore und Fehlersuche systematisch betrieben? | [→ 5. Monitoring/Backup/Troubleshooting](#5-monitoring-backup-und-troubleshooting) |

---

## 1. DNS, DHCP und NTP

> **Grundprinzip:** Ein Brief ohne korrekte Anschrift (DNS), ohne bekannten Absender-Rückweg (DHCP) und mit einem kaputten Datumsstempel (NTP) kommt nicht zuverlässig an – selbst wenn Postbote und Fahrzeug (das Netzwerk aus LF9.1) einwandfrei funktionieren. DNS, DHCP und NTP sind die unsichtbaren Basisdienste, ohne die alles andere in diesem Artikel nicht zuverlässig funktioniert.

### 1.1 DNS: autoritativ, rekursiv, Cache

Ein **autoritativer Server** verwaltet die Daten einer eigenen Zone; ein **rekursiver Resolver** beschafft Antworten im Auftrag von Clients und speichert sie zwischengespeichert (Cache) für die verbleibende Gültigkeitsdauer.

| Record-Typ | Bedeutung | IHK-Relevanz |
| --- | --- | --- |
| **A / AAAA** | Name → IPv4-/IPv6-Adresse | 🔴 |
| **PTR** | Adresse → Name (Reverse-Zone) | 🔴 |
| **CNAME** | Alias → kanonischer Name | 🔴 |
| **NS** | Zuständige Nameserver einer Zone | 🟡 |
| **MX** | Zuständiger Mailserver mit Priorität | 🟡 |

> **Wichtig für die Prüfung:** Die Zone **`.local`** ist für **Multicast DNS (mDNS)** vorgesehen und sollte deshalb nicht als normale interne Unicast-DNS-Domain verwendet werden – für Schulungs-/Testzwecke eignen sich die reservierten Namensräume `.example`/`.test`, produktiv wird meist eine Subdomain der eigenen Organisationsdomain genutzt. Ein DNS-Test muss immer mehrere Fragen getrennt beantworten: Ist überhaupt eine Antwort vorhanden? Aus welchem DNS-System stammt sie? Ist der befragte Server für die Zone autoritativ, oder arbeitet er als rekursiver Resolver? Stammt die Antwort möglicherweise aus dessen Cache? "Name lässt sich auflösen" allein sagt nichts darüber, welche dieser Ebenen tatsächlich geprüft wurde.

### 1.2 DHCP: Scope, Relay, Lease

Ein **Scope** enthält Präfix, Pool, Ausschlüsse, Lease-Dauer und Optionen (Gateway, DNS-Server, Domäne). Infrastrukturserver erhalten meist statische Adressen oder dokumentierte Reservierungen außerhalb des dynamischen Pools. Ein **DHCP-Relay** transportiert Anfragen über VLAN-/Routergrenzen zu einem zentralen Server (siehe auch LF9.1, Abschnitt 7 zum Relay-Prinzip).

> **Wichtig für die Prüfung:** Die Lease-Dauer ist eine bewusste Betriebsentscheidung, kein Standardwert: kurze Leases reagieren schneller auf Änderungen, erhöhen aber den Erneuerungsverkehr; lange Leases stabilisieren große, wenig veränderliche Netze. Schutz vor unautorisierten DHCP-Servern (Rogue DHCP) ist eine Aufgabe der Switching-Plattform (z. B. DHCP Snooping) – "wir haben nur einen DHCP-Server im Netz" ist eine organisatorische Behauptung, kein technischer Schutz.

### 1.3 NTP: Stratum und Abhängigkeitskette

**Konsistente Zeit** ist Voraussetzung für vergleichbare Logs, gültige Zertifikatsprüfung und nachvollziehbare Störungsanalyse. NTP organisiert Zeitquellen in **Stratum-Ebenen** – ein höheres Stratum bedeutet größere logische Entfernung von der Referenzquelle, **nicht** automatisch "schlechtere Zeit".

> **Wichtig für die Prüfung – die Abhängigkeitskette:** Ein Client erhält per DHCP Adresse, Gateway, DNS-Server und häufig auch Zeitserver-Informationen. DNS ermöglicht die Namensauflösung – dafür ist keine korrekte Systemzeit nötig. Korrekte Zeit wird aber für andere Zwecke kritisch: Zertifikatsprüfung (Abschnitt 4), Kerberos, vergleichbare Logs. NTP muss dafür auf Client, Resolver und Zielsystem konfiguriert **und geprüft** sein – es stellt Konsistenz nicht automatisch her, sondern synchronisiert gegenüber konfigurierten Quellen. DNS, DHCP und NTP bilden also eine **betriebliche** Abhängigkeitskette, aber keine strikt lineare technische Reihenfolge; sie dürfen deshalb **nicht als drei isolierte grüne Prozessanzeigen** abgenommen werden, sondern nur im Zusammenspiel geprüft werden.

> **IHK-Typfrage:** *"Ein Client erhält eine IP-Adresse, kann `portal.corp.example` aber nicht auflösen. Beschreiben Sie den Diagnoseweg."*
> **Musterantwort:** Da eine Adresse vorhanden ist, wird nicht sofort die Firewall verdächtigt oder vollständig geöffnet. Geprüft wird stattdessen schrittweise: (1) Welcher DNS-Server ist dem Client zugewiesen (Konfiguration/DHCP-Option)? (2) Ist dieser Resolver überhaupt erreichbar (Transporttest)? (3) Liefert der Resolver eine Antwort, und ist sie autoritativ oder aus dem Cache? (4) Stimmt der zurückgelieferte Record-Inhalt mit der Erwartung überein? Erst das Ergebnis jedes einzelnen Schritts bestimmt die nächste Hypothese – ein pauschales "DNS geht nicht" ersetzt diese Eingrenzung nicht.

---

## 2. Serverbetriebssysteme und Berechtigungen

> **Grundprinzip:** Ein Tresor mit dem stärksten Schloss der Welt nützt nichts, wenn jeder Mitarbeitende denselben Generalschlüssel trägt und niemand aufschreibt, wer wann welche Tür öffnen darf. Sichere Dienste brauchen genauso eine betreibbare Plattform **und** ein nachvollziehbares Identitätsmodell – eines ohne das andere reicht nicht.

### 2.1 Plattformwahl: Bare Metal, VM, Container

| Plattform | Stärke | Betriebliche Grenze | IHK-Relevanz |
| --- | --- | --- | --- |
| **Bare Metal** | Direkter Hardwarezugriff, klare Leistungsgrenze | Geringere Konsolidierung, Hardwarebindung | 🟡 |
| **Virtuelle Maschine** | Starke Trennung, unterschiedliche Gastbetriebssysteme möglich | Hypervisor und Ressourcenüberbuchung mitplanen | 🔴 |
| **Container** | Leichtgewichtig, reproduzierbar, schnell bereitstellbar | Gemeinsamer Kernel, Image-Lieferkette beachten (siehe auch LF5.4, Abschnitt 5) | 🔴 |

> **Wichtig für die Prüfung:** Virtualisierung/Containerisierung verbessert Auslastung und Wiederholbarkeit – sie **ersetzt weder Rechte-, Patch-, Backup- noch Monitoringkonzept**. RAID erhöht die Verfügbarkeit gegenüber einzelnen Datenträgerausfällen, ist aber **kein Backup** (siehe Abschnitt 5.2).

### 2.2 Konten, Gruppen, Least Privilege

**Konten** identifizieren Personen oder technische Prozesse, **Gruppen** bündeln Berechtigungen, **Rollen** beschreiben fachliche Aufgaben. **Least Privilege** bedeutet: Identitäten erhalten nur die Rechte, die sie für ihren Zweck brauchen – regelmäßig überprüft, nicht einmalig vergeben und vergessen.

> **Wichtig für die Prüfung – ein klassischer Fehler:** "Adminrechte geben" ist keine saubere Reparatur für ein Berechtigungsproblem, sondern verschleiert es. Entscheidend sind **effektive Rechte** (das Zusammenspiel aus Gruppenmitgliedschaften und Vererbung), nicht die schnellste Lösung. Dienstkonten werden strikt von persönlichen Administrationskonten getrennt; gemeinsame Kennwörter, fest im Code eingebaute Secrets und dauerhafte Vollrechte für Dienstkonten sind Warnsignale, keine akzeptablen Abkürzungen.

Eine **Berechtigungsmatrix** verbindet Ressource, Rolle/Gruppe, erlaubte Operation, Begründung, Freigabe und Prüftermin. Bei einem Zugriff über eine Windows-Netzwerkfreigabe müssen sowohl Freigabe- als auch Dateisystemrechte den Zugriff erlauben – als vereinfachter Prüfungsmerksatz gilt: Keine Ebene kann Rechte gewähren, die eine andere beteiligte Ebene wirksam verweigert. Innerhalb jeder Ebene spielen zusätzlich Gruppenmitgliedschaften, Vererbung sowie explizite Zulassungs-/Verweigerungsregeln eine Rolle.

> **Wichtig für die Prüfung:** LDAP, Kerberos und RADIUS werden oft verwechselt, erfüllen aber unterschiedliche Rollen: **LDAP** ist ein Zugriffsprotokoll für Verzeichnisdaten (kein Produktname), **Kerberos** ein ticketbasiertes Verfahren für zentrale Authentisierung, **RADIUS** ein verbreitetes AAA-Verfahren speziell für Netzwerkzugänge (siehe auch LF9.1, Abschnitt 4.3 zu 802.1X).

> **IHK-Typfrage:** *"Ein Mitarbeiter kann eine Datei nicht öffnen, obwohl seine Gruppe laut Freigabeberechtigung 'Vollzugriff' hat. Wie gehen Sie vor?"*
> **Musterantwort:** Der naheliegende, aber falsche Reflex wäre, dem Nutzer direkt Administratorrechte zu geben. Stattdessen wird die **effektive Berechtigung** geprüft: Freigabe- und Dateisystemrechte wirken häufig gemeinsam, und die tatsächlich wirksame Berechtigung ist die jeweils einschränkendere Ebene – ein "Vollzugriff" auf Freigabeebene wird z. B. von einem restriktiveren NTFS-/Dateisystemrecht überschrieben. Zu prüfen sind außerdem tatsächliche Gruppenmitgliedschaft (inkl. verschachtelter Gruppen) und eventuell greifende Vererbungsregeln. Erst nach dieser Eingrenzung wird eine gezielte, begründete Rechteänderung vorgenommen – keine pauschale Rechteausweitung.

---

## 3. Web-, Datei-, Druck- und Verzeichnisdienste

> **Grundprinzip:** Ein offener Hoteleingang beweist nicht, dass ein bestimmter Gast auch tatsächlich einchecken, sein Zimmer betreten und den Safe öffnen darf. Genauso beweist ein offener Netzwerkport nur die Erreichbarkeit des Transportendpunkts – nicht Identität, Berechtigung oder erfolgreiche fachliche Nutzung des Dienstes dahinter.

### 3.1 Dienstklassen im Vergleich

| Dienstklasse | Protokoll/Ports (Beispiele) | Fachlicher End-to-End-Test | IHK-Relevanz |
| --- | --- | --- | --- |
| **Web** | HTTP (TCP 80) / HTTPS (TCP 443) | Seite/API mit korrektem Status und Inhalt abrufen | 🔴 |
| **Datei** | SMB (TCP 445), NFS (TCP/UDP 2049, Unix/Linux-Umgebungen) | Datei mit vorgesehener Identität lesen, ändern **und** eine verweigerte Aktion nachweisen | 🔴 |
| **Druck** | IPP (TCP 631) | Auftrag einreichen, Warteschlangenstatus und Ausgabe nachvollziehen | 🟡 |
| **Verzeichnis** | LDAP | Objekt suchen und erlaubte Änderung getrennt prüfen | 🔴 |

> **Wichtig für die Prüfung:** Eine **Dienstematrix** (Anforderungen: Nutzergruppen, Datenmenge, Authentisierung, Autorisierung, Verschlüsselung, Verfügbarkeit, Backup, Monitoring, Clientunterstützung) verhindert, dass ein Dienst isoliert als "läuft" bewertet wird – viele Dienste hängen zusätzlich von DNS, Zeit, Identität, Zertifikaten, Speicher und erlaubten Netzpfaden ab (siehe LF9.1, Abschnitt 5, Firewall-Regeln).

### 3.2 Der mehrstufige Zugriffstest

Ein belastbarer Diensttest prüft nacheinander: **Namensauflösung → Routing/Erreichbarkeit → Firewall → Transport → (ggf.) TLS → Authentisierung → Autorisierung → fachliche Operation**. Bei einer Störung wird jeweils nur **ein** vermuteter Faktor geändert und derselbe Test wiederholt – nicht mehrere Änderungen gleichzeitig.

> **Wichtig für die Prüfung:** "Port offen" ist der Anfang der Diagnose, nicht das Ende. Ein erfolgreicher Porttest beweist ausschließlich, dass ein Transportendpunkt erreichbar ist – weder richtige Identität noch Berechtigung noch erfolgreiche fachliche Operation. Der eigentliche Diensttest beginnt **nach** dem erfolgreichen Porttest.

> **IHK-Typfrage:** *"Ein Fileserver-Dienst zeigt 'läuft' im Prozessmonitor, ein Nutzer kann aber keine Datei öffnen. Welche Prüfschritte folgen?"*
> **Musterantwort:** Der Prozessstatus "läuft" bestätigt nur, dass der Dienst gestartet ist – keine Aussage über Netzpfad, Authentisierung oder Berechtigung. Zu prüfen sind der Reihe nach: Ist der Server über das Netz erreichbar (Transporttest, ggf. Firewall-Freigabe)? Wird der Nutzer erfolgreich authentisiert? Ist die Freigabe für diesen Nutzer/diese Gruppe sichtbar? Stimmen effektive Freigabe- und Dateisystemrechte überein (siehe Abschnitt 2.2)? Erst nach dieser Eingrenzung lässt sich die tatsächliche Fehlerursache benennen – ein grüner Prozessstatus ersetzt diese Prüfkette nicht.

---

## 4. VPN, Remote Access, TLS und Zertifikate

> **Grundprinzip:** Ein Tunnel unter einer Grenze verhindert, dass jemand von außen mithört – er entscheidet aber nicht, wer am anderen Ende aussteigen und wohin gehen darf. Ein VPN schützt den Transportweg; welche Ziele dahinter erreichbar sind, entscheidet weiterhin eine eigene Zugriffsregelung.

### 4.1 Site-to-Site vs. Remote Access

| VPN-Typ | Einsatzfall | IHK-Relevanz |
| --- | --- | --- |
| **Site-to-Site** | Koppelt ganze Standorte (z. B. Hauptwerk ↔ Außenstelle) | 🔴 |
| **Remote Access** | Verbindet einzelne autorisierte Endgeräte (z. B. mobile Administration) | 🔴 |

**IPsec** schützt IP-Kommunikation durch Authentisierung, Integrität und Verschlüsselung: Im **Tunnelmodus** wird das vollständige ursprüngliche IP-Paket geschützt und in ein neues äußeres Paket eingebettet (typisch für Gateway-zu-Gateway-/Standortkopplung), im **Transportmodus** bleibt der ursprüngliche IP-Header grundsätzlich erhalten und nur die Nutzlast wird geschützt (typisch für Host-zu-Host-Verbindungen). **IKE** handelt dafür Sicherheitsparameter und Schlüsselmaterial aus. Modernere Alternativen sind TLS-basierte VPNs (z. B. OpenVPN) und **WireGuard** (schlanker Protokollkern, öffentliche Schlüssel als Gegenstellenidentität).

> **Wichtig für die Prüfung:** Ein aufgebauter VPN-Tunnel beweist noch nicht, dass **nur** die vorgesehenen Ziele erreichbar sind. Nach dem Tunnelaufbau gelten weiterhin Routing, Firewall, Rollen und Rechte – eine **Zugriffsmatrix** (Benutzer-/Gerätegruppe, Authentisierungsverfahren, Zielnetz, erlaubter Dienst, Protokollierung, Sperrprozess) legt das fest.

### 4.2 TLS: Schutzziele und Testgrenzen

**TLS 1.3** war ursprünglich in RFC 8446 spezifiziert; diese wurde im **Juli 2026 durch RFC 9846 ersetzt** (technisch weitgehend unverändert, aber die aktuell gültige Referenz für Neuentwicklungen). Ergänzend gilt seit 2026: **RFC 9852** verlangt, dass **neue** TLS-nutzende Protokolle TLS 1.3 verbindlich vorsehen müssen; **RFC 10015** untersagt bei TLS 1.2 veraltete Schlüsselaustauschverfahren (u. a. statisches RSA). Die grundsätzlichen IETF-Sicherheitsempfehlungen (**RFC 9325**, BCP 195) gelten weiter: veraltete SSL-/TLS-Versionen nicht verhandeln, TLS 1.2 nur sicher konfiguriert betreiben, TLS 1.3 bevorzugen. Für die Prüfung reicht die Grundregel: **TLS 1.3 bevorzugen, TLS 1.0/1.1/SSL nicht verwenden, TLS 1.2 nur mit sicherer Konfiguration**. TLS schützt Vertraulichkeit und Integrität und kann Endpunkte authentisieren.

> **Wichtig für die Prüfung – "HTTPS funktioniert" reicht nicht als Test:** Ein belastbarer TLS-Test prüft getrennt: ausgehandelte Protokollversion, **Zielname** im Zertifikat, vollständige **Zwischenkette**, **Vertrauensanker**, **Gültigkeitszeitraum**, **Sperrstatus** (soweit durch PKI und Client unterstützt, z. B. CRL/OCSP) – und die Ablehnung eines absichtlich **falschen** Namens als Gegenprobe.

### 4.3 Zertifikate und PKI-Grundidee

Ein Zertifikat bindet einen öffentlichen Schlüssel an eine Identität. Eine Zertifizierungsstelle signiert Zertifikate; Clients prüfen die Kette bis zu einem lokal vertrauenswürdigen Anker. Der **private Schlüssel verbleibt ausschließlich unter Kontrolle des vorgesehenen Schlüsselinhabers** und wird vor unbefugtem Zugriff/Export geschützt – bei einem Webdienst kann er z. B. auf dem Server, einem vorgeschalteten TLS-Terminator oder in einem HSM liegen (bei mTLS besitzt zusätzlich der Client ein eigenes Schlüsselpaar). Er gehört in jedem Fall niemals in eine Abgabe oder ein Repository.

> **Wichtig für die Prüfung – typischer Fehler:** Eine Zertifikatswarnung wird **nicht** durch Abschalten der Prüfung "behoben". Zielname, Kette, Zeit, Vertrauensanker und Sperrstatus werden **getrennt** untersucht. Ein selbst signiertes Zertifikat kann im kontrollierten Labor sinnvoll sein, braucht aber eine bewusst verteilte Vertrauenswurzel. Zertifikatsprüfung hängt außerdem direkt von **korrekter Systemzeit** (Abschnitt 1.3) und funktionierender **Namensauflösung** (Abschnitt 1.1) ab – die Abhängigkeitskette aus Abschnitt 1 setzt sich hier fort.

> **IHK-Typfrage:** *"Ein Client meldet beim Verbindungsaufbau zu `portal.corp.example` eine Zertifikatswarnung. Ein Kollege schlägt vor, die Zertifikatsprüfung im Browser einfach zu deaktivieren. Bewerten Sie den Vorschlag."*
> **Musterantwort:** Das Deaktivieren der Prüfung ist keine zulässige Fehlerbehebung, sondern beseitigt die zuverlässige Authentisierung des Servers – die Verbindung kann zwar weiterhin verschlüsselt und innerhalb der jeweiligen Sitzung gegen unbemerkte Veränderung geschützt sein, aber der Client kann nicht mehr sicher feststellen, ob er tatsächlich mit dem vorgesehenen Server oder mit einem Angreifer kommuniziert. Damit ist der Schutz vor Man-in-the-Middle-Angriffen aufgehoben. Stattdessen wird die Warnung systematisch eingegrenzt: Enthält das Zertifikat den korrekten Zielnamen? Liegt die aktuelle Systemzeit innerhalb des Gültigkeitszeitraums? Führt die Kette zu einem tatsächlich vertrauenswürdigen Anker? Ist das Zertifikat eventuell gesperrt? Je nach Ursache folgt eine gezielte Korrektur (z. B. Systemzeit richtigstellen, korrektes Zertifikat einspielen, Vertrauensanker verteilen) statt einer pauschalen Deaktivierung der Sicherheitsprüfung.

---

## 5. Monitoring, Backup und Troubleshooting

> **Grundprinzip:** Ein Rauchmelder ohne Batterie hängt trotzdem an der Decke und sieht funktionsfähig aus – erst der Test (Knopf drücken) zeigt, ob er im Ernstfall wirklich Alarm schlägt. Genauso ist ein Backup ohne geprüften Restore nur eine Vermutung von Sicherheit, kein Nachweis.

### 5.1 Monitoring und Alarmierung

Ein Alarm braucht **Schwelle, Messfenster, Empfänger, Priorität und Reaktion**. Geeignete Signale sind Verfügbarkeit, Latenz, Fehlerquote, Ressourcenauslastung, Lease-Auslastung, Zertifikatsrestlaufzeit, Backupstatus und Logereignisse.

> **Wichtig für die Prüfung:** Jede kurzfristige Abweichung als kritischen Alarm zu melden erzeugt **Alarmmüdigkeit** (echte Probleme gehen im Rauschen unter) – umgekehrt ist "grün" ohne fachlichen **End-to-End-Test** (siehe Abschnitt 3.2) kein Funktionsnachweis. Beide Fehler sind gleich problematisch.

### 5.2 Backup, Restore, RPO/RTO

| Sicherungsart | Prinzip | Restore-Aufwand | IHK-Relevanz |
| --- | --- | --- | --- |
| **Vollsicherung** | Alle ausgewählten Daten | Nur die Vollsicherung nötig | 🔴 |
| **Inkrementell** | Änderungen seit letzter Sicherung (beliebiger Art) | Vollsicherung + **komplette** Kette aller Inkremente | 🔴 |
| **Differenziell** | Änderungen seit letzter **Voll**sicherung | Vollsicherung + **nur letzte** Differenzsicherung | 🔴 |

**RPO** (Recovery Point Objective) beschreibt den maximal tolerierbaren Datenverlust in der Zeit, **RTO** (Recovery Time Objective) die angestrebte Wiederherstellungsdauer. Die **3-2-1-Regel**: mindestens drei Kopien, auf zwei unterschiedlichen Medientypen, davon eine Kopie an einem getrennten Ort.

> **Wichtig für die Prüfung – der wichtigste Merksatz dieses Abschnitts:** Ein erfolgreicher Sicherungslauf ist **kein** Wiederherstellungsnachweis. Erst ein **Restore-Test** (Ausgangslage, Sicherungsstand, Ziel, erwartetes Ergebnis, tatsächliche Dauer, Integritätsprüfung, Abweichungen dokumentiert) belegt die tatsächliche Nutzbarkeit. **RAID ist kein Backup.** Snapshots können eine schnelle Rückkehr zu älteren Zuständen ermöglichen, sind allein aber meist kein vollwertiges Backup: Sie liegen häufig im selben administrativen/technischen Ausfallbereich und können bei kompromittierten Rechten, Speicherverlust, Ransomware oder Standortverlust ebenfalls verloren gehen. Konfigurationsdaten von DNS, DHCP, Firewall und Verzeichnisdiensten gehören ausdrücklich zum Schutzumfang, nicht nur Nutzdaten.

### 5.3 Systematische Fehlersuche

Fehlersuche beginnt mit einer klaren, **reproduzierbaren** Fehlerbeschreibung. Danach werden Hypothesen **schichtweise** geprüft, vom Einfachen zum Spezifischen: Link/Schnittstelle → lokale Adresse/VLAN → Route/Erreichbarkeit → Namensauflösung/Zeit → Transportport/Firewall → Dienststatus/Authentisierung/Berechtigung → fachliche Operation/Daten.

> **Wichtig für die Prüfung:** Jeder Diagnoseschritt dokumentiert Werkzeug, erwartetes Ergebnis, tatsächliches Ergebnis und Folgerung. Eine Änderung wird **nur** vorgenommen, wenn sie eine konkrete Hypothese prüft – nicht als "einfach mal ausprobieren". Mehrere Änderungen gleichzeitig machen unmöglich nachzuvollziehen, welche davon tatsächlich gewirkt hat.

### 5.4 Abnahme und Übergabe

Die Gesamtabnahme verknüpft Anforderungen mit Testfällen: Positiv-, Negativ-, Sicherheits- **und** Restore-Tests, Soll-Ist-Vergleich, dokumentierte Restabweichungen und eine begründete Entscheidung – **angenommen**, **angenommen mit Auflagen** oder **nicht angenommen**.

> **IHK-Typfrage:** *"Warum reicht ein grüner Status in der Monitoring-Übersicht allein nicht für eine Abnahmeentscheidung aus?"*
> **Musterantwort:** Ein grüner Monitoring-Status bestätigt in der Regel nur technische Basiswerte wie Erreichbarkeit oder Prozessstatus – er sagt nichts darüber aus, ob die fachliche Funktion für die vorgesehenen Nutzergruppen tatsächlich funktioniert, ob Backups tatsächlich wiederherstellbar sind, ob Sicherheitsanforderungen (z. B. Negativtests für unautorisierten Zugriff) erfüllt sind, oder ob dokumentierte Restabweichungen ein akzeptables Risiko darstellen. Eine belastbare Abnahmeentscheidung braucht deshalb die vollständige Kombination aus End-to-End-Funktionstests, Sicherheitstests, einem geprüften Restore-Nachweis und einer bewussten Bewertung verbleibender Restabweichungen – nicht nur einen einzelnen technischen Indikator.

---

## 6. Typische Prüfungsfallen

| # | Falle | Richtigstellung |
| --- | --- | --- |
| 1 | `.local` ist eine neutrale Wahl für interne Testnamensräume | `.local` ist für Multicast DNS vorgesehen – `.example`/`.test` sind für Doku/Test geeignet, produktiv eine Subdomain der eigenen Organisationsdomain |
| 2 | Ein erfolgreicher Einzeltest von DNS, DHCP oder NTP beweist, dass der End-to-End-Betrieb funktioniert | Die drei bilden eine betriebliche Abhängigkeitskette – ein Einzeltest beweist noch nicht, dass abhängige Dienste (z. B. Zertifikatsprüfung, Kerberos) im Zusammenspiel funktionieren |
| 3 | Bei einem Berechtigungsproblem hilft am schnellsten, dem Nutzer Adminrechte zu geben | Das verschleiert das eigentliche Problem – entscheidend sind effektive Rechte aus Gruppen und Vererbung |
| 4 | LDAP, Kerberos und RADIUS sind austauschbare Begriffe für "Verzeichnisdienst" | Drei unterschiedliche Rollen: Zugriffsprotokoll, Authentisierungsverfahren, AAA-Verfahren für Netzwerkzugänge |
| 5 | Ein offener Port bzw. "Dienst läuft" beweist einen funktionierenden Dienst | Das beweist nur Transporterreichbarkeit – Identität, Berechtigung und fachliche Operation müssen separat getestet werden |
| 6 | Eine Zertifikatswarnung löst man am einfachsten durch Deaktivieren der Prüfung | Das schaltet den Schutzmechanismus komplett ab – stattdessen Zielname, Kette, Zeit, Vertrauensanker und Sperrstatus einzeln prüfen |
| 7 | Ein erfolgreicher VPN-Tunnelaufbau bedeutet automatisch Zugriff auf alle internen Netze | Nach dem Tunnel gelten weiterhin Routing, Firewall und eine eigene Zugriffsmatrix |
| 8 | Ein erfolgreicher Sicherungslauf ist ein ausreichender Backup-Nachweis | Erst ein geprüfter Restore-Test belegt die tatsächliche Wiederherstellbarkeit |
| 9 | RAID bzw. Snapshots sind ein vollwertiges Backup | RAID ist kein Backup; Snapshots helfen bei manchen Fehlern, liegen aber oft im selben Ausfallbereich und schützen nicht zuverlässig gegen Ransomware, kompromittierte Admin-Rechte oder Standortverlust – nur eine getrennte, getestete Sicherung erfüllt die Backupfunktion |

---

## 7. Vertiefung und Ausblick (freiwillig – für den ersten Durchgang nicht erforderlich)

- **DHCPv6/SLAAC**: Wie IPv6-Clients ihre Adresse zustandslos (SLAAC) oder zustandsbehaftet (DHCPv6) beziehen, im Vergleich zum klassischen IPv4-DHCP-Ablauf.
- **Sekundäre DNS-Server und Zonentransfer**: Wie Redundanz bei DNS über mehrere autoritative Server abgesichert wird.
- **NTP-Redundanz**: Mehrere Zeitquellen und Umgang mit Ausreißern/Ausfällen einzelner Quellen.
- **Zeitlich begrenzte Privilegien (Just-in-Time Access)**: Alternative zu dauerhaften Adminrechten, inkl. Rezertifizierungsprozess für bestehende Berechtigungen.
- **SMB vs. NFS im Detail**: Sperrverhalten, Performance und Identitätsintegration im direkten Vergleich.
- **mTLS (mutual TLS) und Schlüsselrotation**: Wie beide Seiten einer Verbindung sich gegenseitig zertifikatsbasiert authentisieren, und wie Schlüssel/Zertifikate turnusmäßig erneuert werden.
- **Generationenprinzip bei Backups**: Aufbewahrung in mehreren zeitlichen Ebenen (täglich/wöchentlich/monatlich) als Ergänzung zur 3-2-1-Regel.
- **SNMP im Detail**: Zustandsabfragen und Traps/Informs als Monitoring-Kanal neben aktiven End-to-End-Tests.

> 🔍 **Hinweis zur Selbstrecherche:** Konkrete Produktkonfiguration (DNS-/DHCP-Server-Software, Verzeichnisdienst-Produkte, Backup-Lösungen) ist stark plattformabhängig – für die praktische Umsetzung immer die aktuelle offizielle Dokumentation des jeweils eingesetzten Produkts konsultieren.

---

## 8. Selbsttest

| # | Frage | Kurzantwort |
| --- | --- | --- |
| 1 | Warum ist `.local` als interner Testnamensraum ungeeignet? | Die Zone ist für Multicast DNS reserviert |
| 2 | Was unterscheidet eine autoritative von einer rekursiven DNS-Antwort? | Autoritativ: direkt aus der zuständigen Zone; rekursiv: vom Resolver im Auftrag des Clients beschafft (ggf. aus Cache) |
| 3 | Beschreibe die betriebliche Abhängigkeitskette zwischen DHCP, DNS und NTP. | DHCP liefert Adresse/Gateway/DNS-Server; DNS ermöglicht Namensauflösung (unabhängig von der Systemzeit); NTP muss separat konfiguriert und geprüft sein – wichtig u. a. für Zertifikatsprüfung, Kerberos und vergleichbare Logs. Keine strikt lineare technische Reihenfolge |
| 4 | Was bedeutet Least Privilege? | Identitäten erhalten nur die Rechte, die sie für ihren Zweck benötigen – regelmäßig geprüft |
| 5 | Wie entsteht die effektive Berechtigung bei Freigabe- und Dateisystemrechten? | Aus dem Zusammenspiel beider Ebenen – die jeweils einschränkendere Ebene gilt |
| 6 | Was unterscheidet LDAP, Kerberos und RADIUS? | LDAP: Zugriffsprotokoll für Verzeichnisdaten; Kerberos: ticketbasierte Authentisierung; RADIUS: AAA-Verfahren für Netzwerkzugänge |
| 7 | Was beweist ein erfolgreicher Porttest – und was nicht? | Beweist nur Transporterreichbarkeit; nicht Identität, Berechtigung oder fachliche Funktion |
| 8 | Nenne die Reihenfolge eines mehrstufigen Zugriffstests. | Namensauflösung → Routing/Erreichbarkeit → Firewall → Transport → (ggf.) TLS → Authentisierung → Autorisierung → fachliche Operation |
| 9 | Was unterscheidet Tunnelmodus und Transportmodus bei IPsec? | Tunnelmodus: vollständiges ursprüngliches IP-Paket geschützt und in neues Paket eingebettet (Gateway-zu-Gateway); Transportmodus: nur Nutzlast geschützt, ursprünglicher IP-Header bleibt grundsätzlich erhalten (Host-zu-Host) |
| 10 | Was muss ein belastbarer TLS-Test prüfen? | Protokollversion, Zielname, Zwischenkette, Vertrauensanker, Gültigkeit, Sperrstatus, Ablehnung falscher Namen |
| 11 | Was unterscheidet RPO und RTO? | RPO: maximal tolerierbarer Datenverlust in der Zeit; RTO: angestrebte Wiederherstellungsdauer |
| 12 | Warum sind RAID und Snapshots kein vollwertiger Backup-Ersatz? | RAID schützt nur gegen Datenträgerausfall; Snapshots liegen oft im selben Ausfallbereich und schützen nicht zuverlässig gegen Ransomware oder kompromittierte Admin-Rechte |
| 13 | Was unterscheidet inkrementelle und differenzielle Sicherung beim Restore-Aufwand? | Inkrementell: komplette Kette aller Inkremente nötig; differenziell: nur Vollsicherung + letzte Differenzsicherung |

---

## 9. IHK-Cheatsheet

| Begriff | Kurzdefinition |
| --- | --- |
| **Autoritativ vs. rekursiv** | Zuständige Zone selbst vs. Auflösung im Auftrag des Clients (ggf. Cache) |
| **DHCP-Scope** | Präfix, Pool, Ausschlüsse, Lease-Dauer, Optionen |
| **Stratum (NTP)** | Logische Entfernung zur Referenzzeitquelle, nicht Qualitätsmaß |
| **Least Privilege** | Nur die für den Zweck nötigen Rechte, regelmäßig geprüft |
| **Effektive Rechte** | Zusammenspiel aus Gruppenmitgliedschaft, Vererbung, ggf. mehreren Ebenen (einschränkendste gilt) |
| **LDAP / Kerberos / RADIUS** | Zugriffsprotokoll / Ticket-Authentisierung / Netzwerkzugangs-AAA |
| **"Port offen" ≠ Dienst funktioniert** | Nur Transporterreichbarkeit bewiesen, nicht Identität/Berechtigung/Funktion |
| **Site-to-Site vs. Remote Access** | Standortkopplung vs. einzelne autorisierte Endgeräte |
| **IPsec Tunnel- vs. Transportmodus** | Vollständiges Paket eingebettet vs. nur Nutzlast geschützt (Header bleibt erhalten) |
| **TLS-Test** | Version, Zielname, Kette, Anker, Gültigkeit, Sperrstatus, Ablehnungstest |
| **RPO / RTO** | Max. Datenverlust in Zeit / angestrebte Wiederherstellungsdauer |
| **3-2-1-Regel** | 3 Kopien, 2 Medientypen, 1 Kopie getrennter Ort |
| **RAID/Snapshot ≠ vollwertiges Backup** | RAID schützt nur gegen Laufwerksausfall; Snapshots oft im selben Ausfallbereich, nicht ransomware-sicher |
| **Restore-Test** | Erst geprüfte Wiederherstellung belegt Backup-Nutzbarkeit |

---

## 10. Prüfungstaktik

| Aufgabentyp | Typische Formulierung | Was die Prüfung hören will |
| --- | --- | --- |
| Dienstabhängigkeiten erklären | "Warum scheitert X, obwohl Y funktioniert?" | Die konkrete Abhängigkeitskette benennen (z. B. DNS→NTP→Zertifikat), nicht nur "hängt zusammen" |
| Diagnoseweg beschreiben | "Wie gehen Sie bei Fehler X vor?" | Schichtweise Eingrenzung mit einer Änderung pro Schritt, nicht mehrere Vermutungen gleichzeitig |
| Berechtigungsproblem lösen | "Nutzer X hat keinen Zugriff auf Y" | Effektive Rechte über alle beteiligten Ebenen prüfen, keine pauschale Rechteausweitung vorschlagen |
| TLS/Zertifikat bewerten | "Zertifikatswarnung bei X" | Alle Prüfdimensionen einzeln nennen (Name/Kette/Zeit/Anker/Sperrung), Abschalten explizit als falsch benennen |
| Backup-Konzept bewerten | "Ist Backup-Konzept X ausreichend?" | Nach Restore-Nachweis fragen, RPO/RTO einordnen, RAID/Snapshot nicht als Backup akzeptieren |

---

## 11. Merksätze für das Fachgespräch

> Adresse, Name und Zeit bilden eine Abhängigkeitskette – jeder Abschnitt braucht einen eigenen Test.

> Adminrechte sind keine Reparatur für ein Berechtigungsproblem, sondern verschleiern es.

> Der eigentliche Diensttest beginnt erst nach dem erfolgreichen Porttest.

> Ein VPN schützt den Transportweg – die Zugriffsmatrix bestimmt, wer danach wohin darf.

> Eine Zertifikatswarnung wird untersucht, nicht abgeschaltet.

> Ein Sicherungslauf belegt nur die Erstellung – erst ein geprüfter Restore belegt die Nutzbarkeit.

---

```yaml
dokument: LF9.2-wiki-artikel
themenfeld: "LF9.2 - Dienste bereitstellen und betreiben"
kmk_bereich: "FISI-spezifisch: AP2 schriftlicher Bereich (Konzeption und Administration von IT-Systemen)"
titel: "Dienste bereitstellen und betreiben"
typ: "Typ A – Kompakter Prüfungs-Wiki (FISI-fokussiert)"
status: final
stand: 2026-09-14
quellen_intern:
  - "LF9.2.1-5 Lernskripte (User-Story/Kompetenz-Format, Szenario Müller Carbon Tech, durchgängig mit LF9.1 verbunden)"
  - "Bewusster Querverweis zu LF9.1 (Netzwerk-Grundlage), LF5.1 (Requirements), LF5.4 (Container), LF9.1 Abschnitt 4.3 (802.1X) statt Wiederholung"
quellen_fachlich:
  - titel: "RFC 9846 (TLS 1.3, ersetzt RFC 8446 seit Juli 2026), RFC 9852 (TLS 1.3 Pflicht für neue Protokolle), RFC 10015 (veraltete TLS-1.2-Schlüsselaustauschverfahren untersagt), RFC 9325/BCP 195 (TLS-Sicherheitsempfehlungen)"
    herausgeber: "IETF (rfc-editor.org, datatracker.ietf.org)"
    status: "in Runde 2 aktualisiert - RFC 8446 war zum Erstellungszeitpunkt bereits durch RFC 9846 (Juli 2026) ersetzt, lag außerhalb des Wissensstands (Trainingsstand Januar 2026); alle vier RFCs in Runde 2 verifiziert"
  - titel: "§ 21 FIAusbV (Inhalt/Dauer AP2-Bereich), § 24 FIAusbV (Gewichtung 10%)"
    herausgeber: "Bundesministerium der Justiz (gesetze-im-internet.de)"
    status: "in Runde 2 ergänzt/korrigiert - Gewichtung steht in § 24, nicht § 21"
  - titel: "DNS/DHCP/NTP-Grundlagen, Konten/Rechte-Modelle, Dienstklassen (Web/Datei/Druck/Verzeichnis), IPsec/VPN, Backup-Konzepte, Troubleshooting-Systematik"
    herausgeber: "Rohmaterial + etabliertes Systemadministrations-Standardwissen"
    status: "stabile, langjährig etablierte Konzepte"
review_historie:
  - runde: 1
    datum: 2026-09-13
    ergebnis: "Erstdraft erstellt, komplettes LF9.2-Rohmaterial (5 Lernskripte) eingearbeitet. Ein zusammenhängender Artikel gemäß etabliertem LF9.1-Muster (nicht 5 Einzelartikel), Vertiefungen im Deep-Dive statt Haupttext. TLS-RFC-Referenzen (8446, 9325) eigenständig web-verifiziert, beide aktuell und korrekt. Bewusste Querverweise zu LF9.1, LF5.1, LF5.4 statt Wiederholung. FISI-fokussiert (AP2 schriftlicher Bereich, § 21 FIAusbV)."
  - runde: 2
    datum: 2026-09-14
    ergebnis: "3 Reviews eingearbeitet. Wichtigster Fund: RFC 8446 war seit Juli 2026 durch RFC 9846 ersetzt - lag außerhalb meines Wissensstands (Trainingsstand Januar 2026), TLS-Abschnitt inkl. RFC 9852/10015 grundlegend aktualisiert. § 21/§24-FIAusbV-Zitat korrigiert (Gewichtung steht in §24). Weitere Präzisierungen: Zertifikatswarnung-Musterantwort (Authentisierung vs. Integrität unterschieden), Private-Key-Aussage verallgemeinert (HSM/TLS-Terminator/mTLS), DHCP-DNS-NTP-Kette weniger linear, RAID/Snapshot-Aussage nuanciert, IPsec-Modi präzisiert, effektive Rechte auf Windows-Freigaben bezogen, NFS-Port ergänzt, Route/Firewall als getrennte Diagnoseebenen. Eine Review behauptete kaputte Tabellen/abgeschnittenen Text - widerlegt."
  - runde: 3
    datum: 2026-09-14
    ergebnis: "Eigene Abschlussprüfung: Selbsttest 3 trug noch exakt die alte, lineare DHCP→DNS→NTP-Formulierung, die in Abschnitt 1.3 bereits korrigiert worden war (wichtigster Rest, da genau der von zwei Reviews kritisierte Punkt). Prüfungsfalle 1, Selbsttest 9 und Cheatsheet-IPsec-Eintrag ebenfalls an bereits präzisierte Haupttext-Formulierungen angeglichen. Stand-Datum auf 2026-09-14 aktualisiert. Keine weiteren Fachfehler gefunden."
freigabe: "Final gesetzt nach 3 Runden (2 externe Prüfrunden + 1 eigene Abschlussprüfung, 2026-09-13/14) – Freigabe durch Autor:in bestätigt"
```