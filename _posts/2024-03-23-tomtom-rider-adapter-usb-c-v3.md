---
layout: post
title: "Tomtom-Rider-Adapter V3"
author: karsten
lang: de
translation_key: tomtom-rider-adapter-v3
permalink: /tomtom-rider-adapter-usb-c-v3/
categories: [ 3D-Printing, motorbike ]
image: assets/images/2024-03-23/20240322_125245.jpg
featured: yes
beforetoc: "Strom für dein Smartphone"
---

# Das Maximum herausholen: V3 mit USB-C-Buchse

Mit USB-A und 5 V scheint der maximale Ladestrom bei 1 A zu liegen.
Mit USB-C und 5 V sind offenbar 1,5 A möglich. Also bauen wir noch eine weitere, vielleicht letzte Version.

Zumindest laut dieser [Quelle](https://forum.digikey.com/t/simple-way-to-use-usb-type-c-to-get-5v-at-up-to-3a-15w/7016/33) sollte ein höherer Strom bei USB-C mit 5 V leicht zu erreichen sein. Laut Überschrift sollten sogar bis zu 3 A möglich sein.

Ich bestellte zwei USB-C-Buchsen. Schon ohne einen Versuch war klar, dass sich ihre winzigen Anschlüsse nicht mit meiner frei verdrahteten Widerstandsschaltung vertragen würden.

Es gibt aber auch praktische kleine Platinen mit bereits aufgelöteten USB-C-Buchsen.
Also noch einmal bestellen und warten.

Als die kleinen Platinen ankamen, musste ich nur das 3D-Modell erneut anpassen und ein passendes Gehäuse für die Platine daraus machen.

## Zusammenbau

![Die Platine passt gut in das Unterteil](/assets/images/2024-03-23/20240321_231059.jpg)

![Ich habe die Höhe des Oberteils reduziert, um Druckzeit zu sparen](/assets/images/2024-03-23/20240321_231104.jpg)

![Der USB-C-Anschluss von der Seite](/assets/images/2024-03-23/20240322_091409.jpg)

![Alles wird mit Zweikomponentenkleber fixiert](/assets/images/2024-03-23/20240322_103936.jpg)

## Ergebnis

Messungen am Labornetzteil zeigen, dass mein Galaxy S21 etwa 1,5 A aus der Buchse zieht, wenn sie mit 5 V versorgt wird. Nicht ganz die ebenfalls möglichen 3 A, aber immerhin mehr als die 1 A der vorherigen Version.

![Alle drei Versionen des Entwurfs nebeneinander](/assets/images/2024-03-23/20240322_125245.jpg)

Die 3D-Modelle findest du [hier](https://www.printables.com/de/model/817171-tomtom-rider-mount-adaptor-remix-v3).
