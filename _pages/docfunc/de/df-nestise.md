---
layout: page
title: NEST/is-e
permalink: "docfunc/de/df/nestise"
language: de
---
{% include new-badge.html version="3.3.1" %} 

Diese Dokumentfunktion wird im Zusammenhang mit dem Produkt ["nest"](https://www.nest.ch/) benötigt.

Hinweis: Diese Dokumentfunktion ist nur Client-Seitig verfügbar.

Ein Aufruf über Connect ist bei dieser Funktion erforderlich, da dieser den Zielpfad im Dokument (in einem gesonderten Custom XML Part) ablegt.

```
<Function name="nest/is-e" id="d96565fd-b4f0-4939-910f-2b4962a235c5">
    <Arguments>
        <Path>D:\Test\Document.pdf</Path>
    </Arguments>
</Function>
```

Anmerkung zum 'Path':
* Muss ein vollständiger Dateipfad sein, welcher beim Klicken auf "Freigeben zur Weiterverarbeitung" ausgewertet wird und die Datei entsprechend gespeichert wird 
* Umgebungsvariablen wie z.&nbsp;B. %temp% werden unterstützt
* Dateiendung wird beim Speichern ausgewertet: Im Falle von einem .pdf wird über Word ein PDF/A erzeugt.
Ist der angegebene Ordner nicht vorhanden, wird die Option im Microsoft Word nicht angeboten.
