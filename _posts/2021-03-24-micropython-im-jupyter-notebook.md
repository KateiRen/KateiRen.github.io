---
layout: post
title: "ESP32/ESP8266 mit MicroPython über Jupyter Notebook programmieren"
author: karsten
lang: de
translation_key: micropython-jupyter
permalink: /micropython-im-jupyter-notebook/
categories: [ tutorial, micropython ]
image: assets/images/photo-1586588833333-08622b507265.jpg
beforetoc: "Foto von Harrison Broadbent auf Unsplash"
---

## Kennst du Jupyter Notebook?

Beginnen wir mit der offiziellen Definition, hier auf Deutsch wiedergegeben:

> Jupyter Notebook ist eine Open-Source-Webanwendung, mit der sich Dokumente erstellen und teilen lassen, die ausführbaren Code, Gleichungen, Visualisierungen und erklärenden Text enthalten. Zu den Einsatzgebieten gehören Datenbereinigung und -transformation, numerische Simulation, statistische Modellierung, Datenvisualisierung, maschinelles Lernen und vieles mehr.

*[https://jupyter.org/](https://jupyter.org/)*

Jupyter unterstützt über 40 Programmiersprachen, wird meiner Einschätzung nach aber vor allem mit Python verwendet.

## Macht dir MicroPython Spaß?

Python gehört zu den meistgenutzten Programmiersprachen und wird immer beliebter. Da ist es kein Zufall, dass Python inzwischen auch auf Mikrocontrollern wie dem ESP32 oder ESP8266 verfügbar ist und häufig eingesetzt wird.

Die offizielle Beschreibung von MicroPython, auf Deutsch wiedergegeben:

> MicroPython ist eine schlanke und effiziente Implementierung der Programmiersprache Python 3. Sie enthält einen kleinen Teil der Python-Standardbibliothek und ist für den Einsatz auf Mikrocontrollern und in Umgebungen mit begrenzten Ressourcen optimiert.

*[https://www.micropython.org/](https://www.micropython.org/)*

Mit [REPL](https://docs.micropython.org/en/latest/wipy/tutorial/repl.html?highlight=repl) und [WebREPL](https://docs.micropython.org/en/latest/esp8266/tutorial/repl.html?highlight=webrepl) lassen sich Mikrocontroller bereits sehr bequem ansprechen und steuern.

Bei meinem Programmierstil nach dem Prinzip Versuch und Irrtum kosten die häufigen Codeübertragungen, Neustarts und erneuten Verbindungen trotzdem Zeit.

## Verbinden wir beides!

Was wäre, wenn wir MicroPython und Jupyter kombinieren könnten? Die komfortable Webanwendung mit Unterstützung für ausführbaren Code zusammen mit einer auf Mikrocontroller zugeschnittenen Programmiersprache? Hieß es nicht, Jupyter Notebook unterstütze über 40 Sprachen?

Die Antwort findet sich hier: [https://github.com/jupyter/jupyter/wiki/Jupyter-kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels).

Tatsächlich gibt es einen speziellen Jupyter-Kernel, der MicroPython auf ESP8266-/ESP32-basierten Boards unterstützt!

### Installation

Das verlinkte [GitHub-Repository](https://github.com/goatchurchprime/jupyter_micropython_kernel/) enthält die Dokumentation zur Installation und zu den ersten Schritten.

In meinem Fall, unter Windows 10 mit bereits installiertem Python und Jupyter, genügten diese Befehle:

```cmd
git clone https://github.com/goatchurchprime/jupyter_micropython_kernel.git
pip install jupyter_micropython_kernel
python -m jupyter_micropython_kernel.install
```

Um die erfolgreiche Installation zu prüfen, lässt du alle installierten Kernel auflisten:

```cmd
jupyter kernelspec list
```

Beim Erstellen eines neuen Notebooks fragt Jupyter nun, welcher Kernel für das Dokument verwendet werden soll:

- MicroPython - USB **- juhu! -** oder
- Python 3

### Erste Schritte

Trage eine Zeile wie diese mit dem passenden Port in die erste Zelle ein, um dich mit deinem MicroPython-Board zu verbinden. Voraussetzung ist, dass die MicroPython-Firmware bereits geflasht ist und funktioniert:

```python
%serialconnect to --port=COM4 --baud=115200
```

Der Code in diesem Notebook wird dann direkt an dein ESP32-/ESP8266-Board gesendet und dort ausgeführt.

Eine ausführliche Anleitung mit weiteren Möglichkeiten findest du [hier](https://lemariva.com/blog/2019/01/micropython-programming-an-esp-using-jupyter-notebook).
