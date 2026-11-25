---
layout: post
title: "Wie der DNS-Rebind-Schutz lokale HTTPS-Dienste ausbremst"
author: karsten
lang: de
translation_key: dns-rebind-protection
permalink: /dns-rebind-schutz-und-lokales-https/
categories: [ tutorial, raspi, docker ]
image: assets/images/photo-1605745341112-85968b19335b.jpg
beforetoc: "Foto von Ian Taylor auf Unsplash"
---

## Worum es geht

Seit einiger Zeit suchte ich nach einer komfortablen Lösung, um *1Password* und *KeePassXC* als meine zentralen Passwortmanager abzulösen.
Der vielversprechende Kandidat war die selbst gehostete Variante *bitwarden_rs*, inzwischen in *vaultwarden* umbenannt.

Das Ziel: ein komfortabler und sicherer Passworttresor ohne Erreichbarkeit von außen, aber mit automatischer Synchronisierung auf allen eingerichteten Geräten.

## Wo liegt das Problem?

Moderne Browser erlauben die Nutzung der Web-Crypto-APIs nicht in unsicheren Kontexten, also ohne HTTPS.

Deshalb enthalten alle Anleitungen zum selbst gehosteten *vaultwarden*-Dienst auch die Einrichtung einer weiteren Docker-Instanz mit einem Reverse Proxy, der die HTTPS-Verbindung bereitstellt.

Damit HTTPS funktioniert, fehlen zunächst zwei Dinge:

- ein gültiger Domainname und
- die erforderlichen Zertifikate.

Durch die Anleitungen lernte ich [DuckDNS](https://www.duckdns.org/domains) kennen, einen einfachen und kostenlosen Dienst zur Registrierung von Subdomains unter [duckdns.org](https://www.duckdns.org/domains).

Am einfachsten kommt man heutzutage über [Let's Encrypt](https://letsencrypt.org/) an Zertifikate.

**Der Betrieb ausschließlich im privaten Netzwerk bringt zwei Hindernisse mit sich. Eines ist in allen Anleitungen dokumentiert, das andere nicht.**

Das erste Hindernis: Da kein Datenverkehr von außen den lokalen Dienst erreichen soll, können wir keine HTTP-Validierung verwenden. Dabei würde eine bestimmte Datei auf unserem Webserver beweisen, dass uns die Domain gehört.

Das ist dokumentiert. Als Alternative wird die DNS-Challenge beschrieben: Ein bestimmter TXT-Eintrag in der DNS-Zone der Domain dient als Nachweis, dass sie uns gehört.

Hier begannen jedoch die Schwierigkeiten, und ich verlor mehrere Abende mit der Fehlersuche.

Keine der Varianten funktionierte richtig: weder Caddy mit DNS-Authentifizierungsmodul noch nginx mit separat beschafften Zertifikaten noch die nicht empfohlene Rocket-Variante. Stattdessen gab es merkwürdige Ergebnisse.

Ping, nslookup und traceroute auf meine frisch angelegte Subdomain lieferten innerhalb und außerhalb meines Netzwerks unterschiedliche Ergebnisse. Ich dachte, den Übeltäter in [Pi-hole](https://pi-hole.net/) gefunden zu haben. Also verbrachte ich mehrere Abende damit, Einstellungen und die Freigabeliste anzupassen, die Software zu aktualisieren, andere Docker-Parameter auszuprobieren und [Pi-hole](https://pi-hole.net/) sogar vollständig zu deaktivieren.

Ich leerte alle DNS-Caches und stellte sicher, dass der vorgeschaltete DNS-Server meiner [FRITZ!Box](https://avm.de/produkte/fritzbox/) verwendet wurde. Trotzdem ließ sich der Domainname weiterhin nicht auflösen!

Bei einem weiteren Versuch, die DNS-Einstellungen meiner [FRITZ!Box](https://avm.de/produkte/fritzbox/) zu ändern, stieß ich auf eine Funktion, von der ich bis dahin noch nichts gehört hatte: den DNS-Rebind-Schutz!

*Das ist kein Fehler, sondern eine Funktion!*

Zum Schutz vor [DNS-Rebinding-Angriffen](https://www.ceilers-news.de/serendipity/37-DNS-Rebinding-Ein-altbekannter-Angriff-kompromittiert-Router.html) verhindert der Router, dass Domainnamen zu lokalen IP-Adressen aufgelöst werden.

Wir müssen lediglich unsere Domain einschließlich Subdomain als Ausnahme eintragen, und schon funktioniert alles wie von Zauberhand!

So einfach ist das! Da es sich um eine Sicherheitsfunktion handelt, dürften andere Router ein ähnliches Verhalten zeigen. Warum wurde das in keiner der Anleitungen erwähnt?

**Für mich ist das Problem jedenfalls endlich gelöst.**
