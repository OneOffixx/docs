---
layout: page
title: Client Installation
permalink: "install/de/client/"
language: de
---

Sie finden hier die Systemvoraussetzungen und eine Anleitung für die Installation der OneOffixx Client Komponenten.

## <i class="fa fa-wrench" aria-hidden="true"></i> Systemvoraussetzung {% include anchor.html name="system-requirements" %}

__Betriebssystem__

Sie können den OneOffixx Client auf folgenden Betriebssystemen installieren:

* Windows 8.1 oder höher (sowohl 32-bit oder 64-bit) 
* Windows Server 2012 oder höher (sowohl 32-bit oder 64-bit)
* [Citrix XenApp/TS oder Windows Terminal Server (Versionskompatibel ab Version 8.1 32-bit und 64-bit)]({{ site.baseurl }}/install/de/client-citrix-ts)

__Unterstützte Microsoft Office Versionen__

OneOffixx unterstützt alle Microsoft Office Versionen __ab Office 2010__, sowohl in der 32-bit als auch in der 64-bit-Variante. __[OneOffixx unterstützt die früheren Versionen von Microsoft Windows und Microsoft Office bis zu deren Lebensende, d.h. bis zum Ende der „Erweiterten Supportdauer", die von Microsoft kommuniziert wird.](http://oneoffixx.com/lifecycle/)__

__.NET Framework__

Für den OneOffixx Client ist mindestens das __[.NET Framework 4.7.2](https://dotnet.microsoft.com/download/thank-you/net472)__ vorausgesetzt.

__CPU & Arbeitsspeicher__

Es wird empfohlen, mindestens eine CPU mit 1.6 GHz und 2 Cores zu verwenden. Als Arbeitsspeicher sollte mindestens 4 GB RAM verwendet werden. 

Grundsätzlich funktioniert OneOffixx auf jedem System, welches auch Microsoft Office ausführen kann.

__Festplattenspeicher__

Die Software selbst benötigt etwa 200 MB Festplattenspeicher. Der OneOffixx Client speichert zudem Log-Dateien zur Fehlerbehandlung und Einstellungen und legt zusätzlich einen lokalen Cache für die Offline-Nutzung an. Die Größe des Caches ist abhängig von der Anzahl und Größe der Vorlagen. 

__Auflösung__

Für den OneOffixx Client wird eine Bildschirmauflösung von mindestens 1024 \* 768 vorausgesetzt.

## Client Authentifizierung

__Windows Authentication:__

Für den Betrieb mit "Windows Authentication" ist es erforderlich, dass der OneOffixx Client und Server sich in der gleichen bzw. über Vertrauensstellung authorisierten Domäne befinden.

__"OneOffixx":__

In der "Software as a Service"-Variante wird eine weitere Komponente eingesetzt, welche die Anmeldeanfragen direkt zu Ihrem Azure Active Directory weiterleitet. Bei dieser Variante muss eine `ClientId` und ein `ClientSecret` während der Installation angegeben werden.

## <i class="fa fa-power-off" aria-hidden="true"></i> 32-bit oder 64-bit Installation {% include anchor.html name="bitness" %}

Den OneOffixx Installer gibt es in einer 32- oder 64-bit Variante. Die benötigte Version ist __abhängig von der verwendeten Microsoft Office Version__. Wird ein 64-bit Microsoft Office verwendet, __muss__ die 64-bit OneOffixx Version installiert werden. 

## <i class="fa fa-plug" aria-hidden="true"></i> Ports & Server-Verbindung {% include anchor.html name="ports" %}

Der OneOffixx Client kommuniziert __ausschliesslich über HTTP/HTTPS__ mit dem OneOffixx Server, damit wird in der Standard-Konfiguration nur Port 80 bzw. 443 benötigt.

## <i class="fa fa-windows" aria-hidden="true"></i> MSI Parameter {% include anchor.html name="msi" %}

Das OneOffixx MSI-Paket enthält den OneOffixx Client und die verschiedenen Microsoft Office AddIns. 

__OneOffixx-spezifische Parameter:__

* APPLICATIONFOLDER = install folder (default C:\Program Files (x86)\OneOffixx)
* INSTALLDESKTOPSHORTCUT = 1 / 0 for yes or no
* AUTOSTART = 1 / 0 for yes or no
* SERVICEENDPOINTURL = Service Endpoint (\*can be overwritten via registry)
* SERVICESPN = SPN for the user, which runs the Service (advanced setting, might only be needed when the Service runs under a Service-Account and SQL Integrated Authentication is used. (\* can be overwritten via registry) 
* ADDLOCAL = Features
     * WordAddInFeature = Word Add-In
     * OutlookAddInFeature = Outlook Add-In
     * ExcelAddInFeature = Excel Add-In
     * PowerPointAddInFeature = PowerPoint Add-In
     * OfferOfEvidenceAddInFeature = OneOffixx Law Add-In
     * RegulationsAddInFeature = OneOffixx Booklet Add-In

Die folgenden Parameter werden nur in bestimmten Installationsvarianten (z.&nbsp;B. Installation auf Terminal-Servern) benötigt und sind optional: 

* DATAINLOCALAPPDATAFOLDER = False/True (must be True on Network Share)
* CACHEFOLDER = Path e.g. \\Share\... (with Placeholders like %username% from environment-variables etc.)
* SETTINGFOLDER = Path e.g. \\Share\... (with Placeholders like %username% from environment-variables etc.)
* SHUTDOWNONDISCONNECT = true / false (Allows to configure OneOffixx to shutdown when a disconnect happens (such as disconnecting from an RDP Session) )
* LOGFOLDER = Path for storing logfiles e.g. \\Share\... (with Placeholders supported by [NLog](https://github.com/NLog/NLog/wiki/Layout-Renderers))

Folgende Parameter werden in der "Software as a Service"-Variante benötigt:

* AUTHCLIENTID = ClientId to access the OneOffixx Auth System (\*can be overwritten via registry)
* AUTHCLIENTSECRET = ClientSecret to access the OneOffixx Auth System (\*can be overwritten via registry)

Es gelten ansonsten die normalen __[MSIEXEC Command-Line Options](https://docs.microsoft.com/de-de/windows/desktop/Msi/command-line-options)__.

Beispiel:

    msiexec /qb /i "OneOffixx.Install(x86).msi" APPLICATIONFOLDER="C:\Program Files (x86)\OneOffixx" SERVICEENDPOINTURL="http://appurl/Service/OneOffixxService.svc" INSTALLDESKTOPSHORTCUT=1 AUTOSTART=1 OneOffixxInstall.log AddLocal=WordAddInFeature,OutlookAddInFeature

__ServiceEndpointUrl via Registry:__ {% include anchor.html name="serviceendpoint-registry" %}

OneOffixx sucht in der Registry nach einem String-Value "ServiceAddress" unter diesem Schlüssel (HKCU & HKLM):

    [HKEY_CURRENT_USER\Software\Sevitec Informatik AG\OneOffixx]
    "ServiceAddress"="http..."

bzw. 

    [HKEY_LOCAL_MACHINE\Software\Sevitec Informatik AG\OneOffixx]
    "ServiceAddress"="http..."

bzw. (für __Group-Policies__ geeignet)

    [HKEY_CURRENT_USER\Software\Policies\Sevitec Informatik AG\OneOffixx]
    "ServiceAddress"="http..."
  
Findet der Client diesen Wert, wird dieser anstelle der ServiceAddress aus der OneOffixx.exe.config genommen. Um diese Einstellungen via Gruppenrichtlinien steuern zu können, stehen __[ OneOffixx ADMX Vorlagen]({{ site.baseurl }}/assets/content-files/OneOffixxGroupPoliciesTemplate.zip)__ zur Verfügung.

__ServiceSpn via Registry:__ {% include anchor.html name="servicespn-registry" %}

Ähnlich der "ServiceEndpointUrl" kann der Service Principal Name (SPN) des Services über den Schlüssel "ServiceSpn" angegeben werden:

    [HKEY_CURRENT_USER\Software\Sevitec Informatik AG\OneOffixx]
    "ServiceSpn"="http\FQDN-Of-The-Service"
    
    bzw.
    
    [HKEY_LOCAL_MACHINE\Software\Sevitec Informatik AG\OneOffixx]
    ...
    
    bzw.
    
    [HKEY_CURRENT_USER\Software\Policies\Sevitec Informatik AG\OneOffixx]
    ...


OneOffixx sucht in der Registry nach einem String-Value "ServiceSpn". Findet der Client diesen Wert, wird dieser anstelle des ServiceSpn aus der OneOffixx.exe.config genommen. Der SPN ist eine optionale Angabe und wird nur gebraucht wenn der Service unter einem gesonderten Windows-Service-Account läuft und Kerberos-Authentifizierung erforderlich ist. 

Hinweis zum Erstellen von einem SPN: Ein SPN kann auf jeder Maschine innerhalb der Domäne angelegt werden, allerdings benötigt man Domain-Administrator-Rechte. Die Syntax ist wie folgt:

    Setspn -s http/<computername>.<domainname>:<port> <domain-user-account>  

In der OneOffixx-Konfiguration kann das in folgender Form angegeben werden:

    http\oneoffixx.corp.local

wobei "oneoffixx.corp.local" dem Full Qualified Name (FQDN) des OneOffixx Service entspricht. Weitere Informationen zum Thema SPN findet sich in der [MSDN](https://msdn.microsoft.com/en-us/library/ms677949%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396).

__OneOffixx Ribbon Position:__ {% include anchor.html name="oneoffixxribbon" %}

Die OneOffixx Ribbons in den Office Programmen werden immer an die erste Stelle positioniert. Möchte man dieses Verhalten anpassen, so müssen folgende Registry-Schlüssel gesetzt werden:

    "AddinWordRibbonPosition"=""
    "AddinPowerPointRibbonPosition"=""
    "AddinExcelRibbonPosition"=""
    "AddinOutlookRibbonPosition"=""

Die Standardeinstellung, die im OneOffixx implizit angewendet wird, wäre: 

    "AddinWordRibbonPosition"="TabHome"

Möchte man den Ribbon ans Ende setzen, reicht es aus, den Wert leer ("") zu lassen. Es können auch andere Werte (z.&nbsp;B. TabInsert) genutzt werden. In diesem Fall wird unser Ribbon __vor__ dem angegebenen Ribbon gesetzt. 

Eine Auflistung der verfügbaren Tabs befindet sich [hier](https://docs.microsoft.com/en-us/office/dev/add-ins/reference/manifest/officetab).

__Terminalserver: OneOffixx unsichtbar machen__ {% include anchor.html name="oneoffixx_hidden" %}

Das Word-AddIn von OneOffixx kann deaktiviert werden. Das ist häufig auf Terminalserver-Umgebungen notwendig, wenn nicht alle Benutzer OneOffixx überhaupt sehen sollen. Achtung: das hat keinerlei Einfluss auf Berechtigungen. Einzig und allein die Sichtbarkeit kann damit gesteuert werden.

Das kann wie folgt erreicht werden:
    
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Office\Word\AddIns\OneOffixx.Word.Addin]
    "LoadBehavior"="3" (immer 3)
    
    und
    
    [HKEY_CURRENT_USER\Software\Microsoft\Office\Word\Addins\OneOffixx.Word.Addin]
    "LoadBehavior"="3" (anzeigen)
    "LoadBehavior"="0" (nicht anzeigen)


Falls ein 32-Bit Office auf einem 64-Bit Windows läuft, dann sind die Keys unter folgendem Pfad zu finden:

    [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Office\Word\Addins\OneOffixx.Word.Addin]
    
    und
    
    [HKEY_CURRENT_USER\SOFTWARE\WOW6432Node\Microsoft\Office\Word\Addins\OneOffixx.Word.Addin]

Dasselbe Prinzip gilt auch für das Outlook-AddIn.

## <i class="fa fa-cogs" aria-hidden="true"></i> Installationsszenarien {% include anchor.html name="install" %}

Die Standardeinstellungen des OneOffixx-Installers zielen auf eine "normale" Installation auf einem System ab, welches von einem oder mehreren Windows-Benutzern gestartet werden kann. Dabei werden sowohl der Cache als auch die Einstellungen unter *%AppData%* gespeichert. Jede OneOffixx-Client-Instanz muss für die vollständige Funktionalität exklusiven Zugriff auf den Cache bekommen - ist das nicht der Fall (z.&nbsp;B. für einige Citrix/Terminal-Server Konfiguration), muss der Speicherort des Caches angegeben werden. 

### <i class="fa fa-cog" aria-hidden="true"></i> OneOffixx Client: Standard Installation

Es werden keine weiteren Parameter benötigt, Cache und Einstellungen werden unter *%AppData%* gespeichert.

__Empfohlen für:__

☑ Keine serverseitig gespeicherten Roaming Profile

### <i class="fa fa-cog" aria-hidden="true"></i> OneOffixx Client: Cache & Einstellungen lokal

Sowohl der Cache als auch die Einstellungen können unter *%AppDataLocal%* gespeichert werden, wenn dieser Parameter gesetzt wird:

    DATAINLOCALAPPDATAFOLDER = True

__Empfohlen für:__

☑ Serverseitig gespeicherte Roaming Profile

☑ *%AppDataLocal%* Ordner wird nicht gelöscht

☑ Benutzer ist immer auf derselben Maschine eingeloggt

### <i class="fa fa-cog" aria-hidden="true"></i> OneOffixx Client: Cache in spezifischen Ordner

Sollte sowohl *%AppData%* als auch *%AppDataLocal%* nicht in Frage kommen oder kann es passieren, dass mehrere OneOffixx Instanzen pro Benutzer offen sein könnten, kann über diese Einstellung der Speicherort für den Cache spezifiziert werden:

    CACHEFOLDER = Path e.g. //Share/... (with Placeholders like %username% etc.)

__Empfohlen für:__

☑ Terminal-Server Installation mit mehreren offenen Sessions pro Benutzer

__Voraussetzung:__

☑ Angegebenes Netzwerk-Laufwerk ist vorhanden und immer erreichbar

### <i class="fa fa-cog" aria-hidden="true"></i> OneOffixx Client: Einstellungen in spezifischen Ordner

Die Einstellungen können genauso wie der Cache in einen eigenen Ordner gespeichert werden. 

    SETTINGFOLDER = Path e.g. //Share/... (with Placeholders like %username% etc.)

## <i class="fa fa-life-ring" aria-hidden="true"></i> Troubleshooting {% include anchor.html name="troubleshooting" %}

__OneOffixx AddIns in Microsoft Office starten nicht {% include anchor.html name="troubleshooting-vcredist" %}__

Falls in einer Office Anwendung bei den Registerkarten kein OneOffixx-Ribbon zu sehen ist, kann das verschiedene Ursachen haben:

* Das OneOffixx AddIn ist nicht installiert: 
    * Sollte das OneOffixx AddIn unter "Datei - Optionen - AddIns" unter den COM AddIns nicht auftauchen, ist es eventuell nicht installiert. Prüfen Sie, ob das entsprechende AddIn bei der Installation ausgewählt wurde.
* Office ist in der 64-Bit Variante installiert, aber es wurde die 32-Bit-Msi-Datei benutzt.
    * Wird Microsoft Office in der 64-Bit-Version benutzt, muss auch der 64-Bit-Installer von OneOffixx verwendet werden.
* Für OneOffixx Version 2.0: das Visual C++ Redistributable 2015 Package fehlt oder ist nicht richtig installiert 
    * Ab OneOffixx Version 2.3.40140: Das VC++ Redistributable 2015 Package ist im OneOffixx enthalten, es kann allerdings passieren, dass eine "korrupte" Systeminstallation das Packets die Ausführung unseres AddIns unterbindet. In dem Fall sollte das [VC++ Redistributable 2015 Package](https://www.microsoft.com/de-ch/download/details.aspx?id=48145) noch einmal installiert werden.

__Das Starten von Microsoft Office ist seit der Installation von OneOffixx stark verzögert {% include anchor.html name="troubleshooting-ngen" %}__

Falls Office langsam starten sollte, kann es daran liegen, dass der "Native Image Generator (ngen)"-Prozess noch nicht vollständig durchgelaufen ist. Dieser Prozess wird nach der Installation automatisch angesteuert und ist ein Bestandteil des .NET Frameworks. 

Ausführung "Native Image Generator" (ngen): Der Prozess ngen.exe befindet sich unter diesem Pfad: c:\windows\microsoft.net\framework\v4.0.30319\ngen.exe

Über "display" kann der Status geprüft werden, das sollte so aussehen:

    C:\Windows\Microsoft.NET\Framework\v4.0.30319\ngen.exe display "C:\Program Files (x86)\OneOffixx\OneOffixx.exe"
    Microsoft (R) CLR Native Image Generator - Version 4.6.1087.0
    Copyright (c) Microsoft Corporation.  All rights reserved.

    NGEN Roots:

    C:\Program Files (x86)\OneOffixx\OneOffixx.exe

    NGEN Roots that depend on "c:\Program Files (x86)\OneOffixx\OneOffixx.exe

    C:\Program Files (x86)\OneOffixx\OneOffixx.exe

    Native Images:

    OneOffixx, Version=2.3.40190.0, Culture=neutral, PublicKeyToken=null

Sollte OneOffixx als "pending" aufgeführt werden, sollte der Kompilierungsvorgang über "update" nochmals gestartet werden:

    c:\Windows\Microsoft.NET\Framework\v4.0.30319\ngen.exe update

DAs sollte das Laden des AddIns wesentlich beschleunigen. 

{% include alert.html type="warning" text="Bei Fragen oder Problemen helfen wir Ihnen natürlich gern weiter - melden Sie sich einfach bei unserem Support." %}
