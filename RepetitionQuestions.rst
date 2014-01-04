============================
HS13 IntTe Repetitionsfragen
============================



Antworten zu den Repetitionsfragen
==================================
Falls vorhanden befinden sich diese im GitHub Repository. Ergänzungen oder ganze Antwortensets sind jederzeit herzlich willkommen. https://github.com/moonline/HSR.modules.IntTe



Symbolerklärung
===============
**[DP]**
Wissensfragen des Dozenten (Slides) für die Prüfung

**[EW]**
Wissensfrage, die über den Stoff der Vorlesung hinaus geht.



Java Webtechnologien
====================
**1.0.1.**
Machen Sie eine Skizze. Wie funktioniert "Web Application Request Handling". Zeichnen Sie mindestens folgende Komponenten in die Skizze ein: Web Client, Web Components, Java Beans Component.

**1.0.2.**
Was ist ein "Web Container"? Welche Services stellt er zur Verfügung?

**1.0.3.**
Was ist ein "Web Module"? Was enthält es alles?

**1.0.4.**
Was sind "Java Server Faces"?


Java Servlets
-------------
**1.1.1.**
Was sind Java Servlets? Aus welchen Komponenten setzen sie sich zusammen?

**1.1.2.**
Geben Sie mit Servlets eine Seite mit einem Titel und ein paar Lorem Ipsum Sätzen aus. Wieso ist diese Art der dynamischen Seitenausgabe ungeeignet und taugt nur zur Demonstration?

**1.1.3.**
Früher war die Konfigurationsdatei web.xml bei Servlet Containern zwingend notwendig. Mittlerweile gibt es noch eine andere Möglichkeit zur Konfiguration. Machen Sie ein Beispiel.

**1.1.4.**
Was passiert, wenn sie gar keine Konfiguration erstellen?

**1.1.5.**
Listen sie auf, was beim Aufruf eines Servlets im Browser im Hintergrund alles abläuft.

**1.1.6.**
Welche Methoden müssen sie implementieren, um Formulardaten verarbeiten zu können? Wie erhalten sie die Parameter vom Formular?

**1.1.7.**
Wie legen sie fest, das die Ausgabe "html" ist?

**1.1.8.**
Machen sie ein kleines Beispiel, wie sie ein Kontaktformular verarbeiten und die Daten serialisiert auf die Platte ablegen.

**1.1.9.**
Wie können sie benutzerdefinierte Error Pages erstellen/konfigurieren?

**1.1.10.**
Skizzieren Sie den Lebenszyklus eines Servlet Containers auf.

**1.1.11.**
Was sind der Servlet Context und der Context Path?

**1.1.12.**
Was sind Sessions? Wie setzten sie Sessions mit Servlets um? Wie beenden Sie eine Session?

**1.1.13.**
Wozu können Sie Cookies nutzen? Wie erzeugen Sie Cookies, setzen Attibute und versenden Cookies? Wie Empfangen Sie Cookies?

**1.1.14.**
Welche Aufgabe übernimmt der Request Dispatcher? Wie können Sie einen Request weiterleiten oder teilweise delegieren?

**1.1.15.**
Was können Sie mit Filter anstellen? Wie mappen Sie Filter? Wie programmieren Sie einen Flter?
Wie können Sie eine Response manipulieren?



Clientseitige Technologien
==========================

HTML
----
**2.1.1.**
Was sind Tags, was sind Attribute?

CSS
---
**2.2.1.**
Auf welche Arten können Sie Stylesheets in HTML Dokumente einbinden?

**2.2.2.**
Was sind Selektoren?


Javascript
----------

Grundlagen
..........
**2.3.1.1.**
Was bedeutet es, das JavaScipt Sandboxed läuft?

**2.3.1.2.**
Wie sind in Javascript Objekte intern aufgebaut (Konzeptuell)? Was ist das Prototype Konzept? Welche Konsequenzen hat Prototyping in Bezug auf Klassen, Vererbung, Overloading, Sichtbarkeit und static Typechecking bei einer interpretierten Sprache wie Javascript?

**2.3.1.3.**
Was ist das DOM? Was ist der Render Tree?

**2.3.1.4.**
Warum ist beim Traversieren des DOMs besonder auf White Spaces acht zu geben?

**2.3.1.5.**
Was sind Eventhandler? Wozu dienen sie?


JQuery
......
**2.3.2.1.**
Wie funktionieren jQuery Selektoren?

**2.3.2.2.**
Wann erhalten Sie in jQuery eine Liste und wann direkt das gesuchte Objekt? Wie greifen Sie in der List auf das erste Element zu? Wie iterieren Sie über die Elemente?

**2.3.2.3.**
Wie verhindern Sie bei jQuery Konflikte mit andern Frameworks, die ebenfalls den Dollar verwenden?

**2.3.2.4.**
Wie können Sie mit jQuery Attribute setzen und auslesen?


Vertiefung
..........
**2.3.3.1.**
Welche Datentypen gibt es in Javascript? Was sind in Objekte? Was sind Konstruktoren?

**2.3.3.2.**
Erklären Sie das Prototype Konzept und wie die Objektvererbung damit funktioniert. Zeigen Sie den Unterschied zwischen einer Prototypemethode und einer Objektmethode auf.

**2.3.3.3.**
Wie ist das Scoping in Javascript? In welchem Bereich gelten Variablen?

**2.3.3.4.**
This funktioniert in Javascript etwas anders als in Java. Erklären Sie und zeigen Sie auf, wie this bei unvorsichtiger Programmierung auf den globalen Scope zeigen kann.

**2.3.3.5.**
Wie wird in Javascript Kapselung realisiert?

**2.3.3.6.**
Was sind Closures und wie funktionieren sie?

**2.3.3.7.**
Was passiert, wenn Methoden überladen werden?

**2.3.3.8.**
Wozu dient der Aufruf:

.. code-block:: Javascript

	var initialize = (function(){ ... })();

**2.3.3.9.**
Welche (vordefinierten) Objekte können in Javascript verändert werden?

**2.3.3.10.**
Was macht die eval() Funktion und warum sollte sie nie verwendet werden?

**2.3.3.11.**
Namespaces gibt es in Javascript explizit nicht. Wie können Objekte und Funktionen trotzdem sauber strukturiert und ähnlich wie bei Namespacing in andern Programmiersprachen aufgerufen werden?

**2.3.3.12.**
Schauen Sie den folgenden Code an und beantworten Sie die Fragen dazu:

.. code-block:: Javascript

	Car = function() {
		var name = "Car"; // b
		this.prototype = new Vehicle(); // a
		this.drive = function() { return "car drive"; }
	}

	Vehicle = function() {
		var name = "Vehicle";
		this.drive = function() { return "vehicle driving"; }
	}

	Car.prototype.turnLightOn = function() { return "Lights are active"; }
	Car.prototype.getName = function() { return this.name; }

	// test objects // k
	car = new Car(); // i
	console.log(car.drive()); // e
	console.log(car.turnLightOn());
	console.log(car.getName()); // g

	car.name = "Car2";
	console.log(car.getName()); // g
	console.log(car); // h


a. Erhält Car eine korrekte prototype Verknüpfung zu Vehicle, obwohl Vehicle nach Car definiert wird? Begründung!
b. In welchem Scope sind Car, car, Vehicle, drive(), turnLightOn und name definiert? Begründung!
c. Kann von Aussen auf die Variablen "name" zugegriffen werden? Begründung!
d. was wird bei den einzelnen "console.log"'s ausgegeben?
e. Was passiert intern (call / search Hirarchy) wenn car.drive() aufgerufen wird?
f. Ist die Funktion turnLightOn für Car definiert? Begründung!
g. was gibt getName() zurück und warum?
h. Wie sieht das Objekt ganz am Schluss aus?
i. Was passiert, wenn bei der Instanzierung von "car" das "new" oder die Klammern vergessen werden?
j. Definieren Sie einen Namespace App.Model.Domain, dem sie Car und Vehicle zuordnen.
k. Definieren Sie einen Namespace App.Controller, dem Sie einen Controller zuordnen. Verschieben Sie die untersten 8 Zeilen in diesen Controller und sorgen sie dafür, das er nach der Definition gleich ausgeführt wird, ohne dies als Befehl in einer neuen Zeile zu definieren.
l. Wie müssen Sie das Programm abändern, damit der Controller erst ausgeführt wird, wenn das "Window" geladen ist?
m. Warum ist die Objektinstanzierung langsamer, wenn die Methoden direkt im Objekt definiert sind (vgl. car.drive() ) als wenn sie vom Prototypen übernommen werden (vgl. car.turnLightOn() )?

**2.3.3.13.**
Was ist die Javascript Object Notation und wozu kann sie verwendet werden?

**2.3.3.14.**
Was passiert, wenn Sie ein Objekt mit "new Object()" anlegen?

**2.3.3.15.**
Warum sollte jede Funktion einen return besitzen? Was ist wenn nicht?

**2.3.3.16.**
Wie funktionieren in Javascript Parameterlisten?


Ajax
----
**2.4.0.1.**
Was ist Ajax? Welche Verschiedenen Technologien werden unter Ajax zusammengefasst? Welche Art von Daten kann mit welcher Technologie übertragen werden?

**2.4.0.2.**
Wie funktioniert ein XHR Request? Machen Sie ein Beispiel. Was sind die Bedingungen für einen Cross-Domain Request?

**2.4.0.3.**
Welche Zustände gibt es beim XHR Request?

**2.4.0.4.**
Machen Sie einen XHR Request Beispiel für Get und Post sowie für synchrone und asynchrone kommunikation.

**2.4.0.5.**
Was ist On-Demand JS? Wie funktioniert es?

**2.4.0.6.**
Was ist JSONP? Wie funktioniert es?

**2.4.0.7.**
Warum wird häufig JSON XML Daten vorgezogen? Nennen Sie zwei Gründe.

**2.4.0.8.**
Wie wird aus den übertragenen Daten wieder HTML, das dem Benutzer angezeigt werden kann? Nennen Sie dies für jede mögliche Art von Übertragungsformat.

**2.4.0.9.**
Was ist XSS? Wo lauern XSS Schlupflöcher und wie können Sie XSS wirksam eindämmen?

**2.4.0.10.**
Was ist Client Seitiges Templating? Erklären Sie die Grundidee von Handlebars und AjaxPages.

**2.4.0.11.**
Wie funktioniert Ajax mit jQuery? Wie machen Sie die Unterscheidung zwischen HXR, JSONP, ... ?


Server Push
...........
**2.4.1.1.**
Was ist Serverpush? Welche Probleme gibt es heute?

**2.4.1.2.**
Welche Technologien gibt es um diese Probleme zu lösen?



REST
====

**3.0.1.**
Was ist REST?

**3.0.2.**
Was soll REST besser umsetzen (konsequenter) als SOAP?

**3.0.3.**
Erklären Sie die vier Level von REST. Was sind Ressourcen?

**3.0.4.**
Machen Sie zu folgenden Szenarien je ein Beispiel inkl. Aufrufdomain und korrekter Aufrufmethode (Übertragungsformat XML):

a) von einem Service eine Liste mit Produkten abrufen
b) von einem Service Informationen über ein bestimmtes Produkt abrufen
c) auf einem Service ein bestimmtes Produkt löschen
d) auf einem Service ein neues Produkt anlegen
e) auf einem Service ein bestimmtes Produkt bearbeiten


**3.0.5.**
Was bedeutet HAETOAS? Zweck?

**3.0.6.**
Warum sollten GET Requests nie Veränderungen auf dem Server vornehmen? Wie müssen Sie verändernde Requests gestalten?

**3.0.7.**
Warum ist bei REST eine statuslose Kommunikation bewusst gewollt? Wo liegen die Vorteile?

**3.0.8.**
Welche zwei Möglichkeiten gibt es trotz Statuslosigkeit einen Warenkorb umzusetzen?


JSF
===
**4.0.1.**
Was sind JSF?

**4.0.2.**
Skizzieren Sie das MVC Pattern für Webanwendungen auf.

**4.0.3.**
Was sind JSF Komponenten?

**4.0.4.**
Was sind Beans? Warum werden für das JSF Templating Beans benötigt?

**4.0.5.**
Wie funktioniert das Templating bei JSF grundsätzlich?

**4.0.6.**
Wie funktioniert der Lebenszyklus eines JSF Requests? Was passiert wenn ein Validator fehlschlägt?

**4.0.7.**
Erklären Sie detailiert die 6 Phasen des Lebenszyklus eines JFS Requests.

**4.0.8.**
Was bewirkt das "immediate" Attribut?

**4.0.9.**
Was sind Facelets?


UI Komponenten
--------------
**4.1.1.**
Was sind JSF UI Komponenten?

**4.1.2.**
Wie ist das JFS UI Komponenten Model aufgebaut?

**4.1.3.**
Wie wird der Component Tree erzeugt?

**4.1.4**
Wozu dienen composition und component?

**4.1.5.**
Wie werden Resources im Tempate angesprochen?

**4.1.6.**
Welche Attribute besitzen alle Komponenten?

**4.1.7.**
Wie teilen Sie dem User Fehlermeldungen mit?

**4.1.8.**
Was ist das Render-Kit und was tut es?


Expression Language
-------------------
**4.2.1.**
Was ist die Expression Language? Wozu dient sie?

**4.2.2.**
Auf welche Objekte können Sie mit Expression Language zugreifen?

**4.2.3.**
Erklären Sie die Scopes

a) @RequestScoped
b) @ViewScoped
c) @SessionScoped
d) @ApplicationScoped
	
**4.2.4.**
Wie können Sie Expression Language innerhalb von Java Beans einsetzen?

**4.2.5.**
Wie greifen Sie mit EL auf Methoden zu? Wie übergeben Sie Parameter? Wie verwenden Sie arithmetische und logische Operatoren?

**4.2.6.**
Was sind implizite Objekte? Welche gibt es und wozu dienen Sie? Welche Informationen stellen sie zur Verfügung?


Converter
---------
**4.3.1.**
Was sind Converter? Wozu werden Sie gebraucht?

**4.3.2.**
Wieso und wozu besitzt ein Converter zwei Sichten?

**4.3.3.**
Wie werden custom Converter regisitriert und implementiert?


Validatoren
-----------
**4.4.1.**
Was ist ein Validator? Welche Standardvalidatoren gibt es?

**4.4.2.**
Wie registrieren und verwenden Sie custom Validators? Wie setzen Sie sie um?

**4.4.3.**
Was ist Bean Validation? Warum ist dies designtechnisch geschickter als Template Validation?


EventListener
-------------
**4.5.1.**
Wozu dienen Event Listener?

**4.5.2.**
Erklären Sie die Begriffe "EventObjekt", "Value Change Event", "Action Event" und "Data model Event".

**4.5.3.**
Skizzieren Sie den Event Handling Lebenszyklus.

**4.5.4.**
Wie registrieren Sie EventListener?


Internationalisierung
---------------------
**4.6.1.**
Wie binden Sie über ein Resource Bundle Übersetzungen ein?

**4.6.2.**
Wie übersteuern Sie die browsereinstellungen?

**4.6.3.**
Wie greifen Sie in einer Bean auf das Bundle zu?


Ajax
----
**4.7.1.**
Was ist ajax und wie funktioniert es?

**4.7.2.**
Wie aktualisieren Sie eine Ausgabe mit ajax, nachdem ein Feld geändert wurde?

**4.7.3.**
Welche Events gibt es bei Ajax?

**4.7.4.**
Wie verwenden Sie Ajax über die Javascript API?



Web Architektur
===============
**5.0.1.**
Erklären Sie die grundlegende Architektur einer Webapplikation.

**5.0.2.**
Zeigen Sie die Unterschiede auf zwischen einer Client zentrierten Architktur und einer Server zentrierten Architektur.

**5.0.3.**
Was sind die Hauptmerkmale von "Action/Request based" und "Component based" Web Frameworks? Wo liegen die wichtigsten Unterschiede?


Patterns
--------
**5.1.1.**
Erklären Sie die folgenden Patterns:

- Template View: 
	- Prinzip
	- two Step View
	- Umsetzung in PHP, ASP.net, JSF
	- Expression Language
- MVC im Web Bereich:
	- Grundkonzept
	- Umsetzung in Struts, Spring MVC, Ruby on Rails und JSF
- Front Controller
- Page Controller

**5.1.2.**
Nennen Sie die wichtigsten ROCCA Architektur Richtlinien



Client Architektur Frameworks
=============================
**6.0.1.**
Welchen Vorteil bringen Bootstrap und Modernizer dem Entwickler? Was kann man damit machen?

**6.0.2.**
Welchen Vorteil bringt jQuery Mobile dem Entwickler? Was kann man damit machen?

**6.0.3.**
Erklären Sie das MVVM Pattern? Welches sind Domain Objekte, welches Views?

**6.0.4.**
Wie funktioniert Templating mit DotJS?

**6.0.5.**
Welchen Vorteil bringt Backbone? Was kann man damit machen?



Plugin Technologien
===================
**7.0.1.**
Nennen Sie 6 alternative Möglichkeiten zur WebApp-Entwicklung mit Javascript.


Browser Plugins
---------------
**7.1.1.**
Nennen Sie Vor- und Nachteile von Java Applets.

**7.1.2.**
Was passiert sobald, sobald die Browserhersteller die NPAPI abschalten.

**7.1.3.**
Erklären Sie das Vorgehen des Browsers, wenn ein Benutzer ein Plugin nicht installiert hat, der Browser das Plugin jedoch benutzen möchte.


Silverlight
-----------
**7.2.1.**
Welchen Vorteil bietet Silverlight gegenüber andern Plugin Technologien? Welche Grundidee steckt dahinter?

**7.2.2.**
Mit welchen Problemen haben Silverlight Plugins zu kämpfen?

**7.2.3.**
Welche Architekturvarianten lässt Silverlight zu?

**7.3.4.**
[DP] Welche Vorteile bietet eine homogene Server/Client Technologie allgemeint?

**7.3.5.**
[DP] Für welche Anwendungen ist Silverlight sicher nicht geeignet?

**7.3.6.**
[DP] Wie lässt sich aufbauend auf einer MS SQL Datenbank sehr schnell eine 3-Tier Silverlight Anwendung entwickeln? Wo wird validiert und wie?

Flash
-----
**7.3.1.**
Welche Möglichkeiten bietet Flash? Warum wird es Flash noch längere Zeit geben obwohl die NPAPI demnächst abgeschaltet wird?

**7.3.2.**
Welche Möglichkeiten gibt es ohne grosse manuelle Eingriffe eine Flash Applikation zu migrieren?


Cross-Compilation
-----------------
**7.4.1.**
Was bietet GWT?

**7.4.2.**
Wie weit bietet GWT GUI Unterstützung?

**7.4.3.**
Inwieweit ist mit GWT RPC möglich?

**7.4.4.**
Welche Einschränkungen gibt es bei GWT gegenüber normalen Java Desktop UI Applikationen? Welche Probleme gibt es?

**7.4.5.**
Was ist die Google App Engine?

**7.4.5.**
GWT bietet unterschiedlichen Code für verschiedene Browser an. Warum ist die von GWT benutzte Variante trotzdem effizienter als die von jQuery benutzte?

**7.4.6.**
[DP] Welche Vorteile bietet Cross-Compilation gegener Browser Plugins?



Performance Optimierung
=======================
**8.0.1.**
Skizzieren Sie mit einem Ablaufdiagramm auf, wie und in welcher Reihenfolge das laden und rendern von Inhalten und Scripts abläuft.

**8.0.2.**
Was passiert, wenn Sie über eine Schleife nacheinander mehrere Elemente in den DOM einfügen? Ist dies empfehlenswert? Wenn nicht, was wäre eine bessere Vorgehensweise?

**8.0.3.**
Wo (Header, Body Top, Body Bottom) sollten Sie die folgenden Elemente platzieren, um möglichst gute Siteperformance zu erhalten?

a) CSS Dateien
b) LESS CSS Dateien, die anschliessend vom LESS Parser zu CSS kompiliert werden (keine Abhänigkeiten zu den Dateien in a)
c) LESS Parser Script, das die LESS Dateien kompiliert
d) Die Scripts, die ihre Domain- und Controller Logik beinhalten, abhängig davon ob sie 
	I) die Domain Logik gleich instanzieren im main Script und die Dateien deshalb bereits geladen sein müssen
	II) ihr main Script über ein Framework wie require.js oder ähnlichen die Dateien der Domain Logik erst nächlädt, sobald die Logik verwendet wird
e) Ihr main script

**8.0.4.**
[EW] Welche Alternativen Möglichkeiten als die Positionierung haben Sie um zu verhindern, das Skripte vor dem Content gerendert werden?

**8.0.5.**
Warum ist es grundsätzlich schlecht für die Performance, wenn die Scripte vor dem Content geparst werden?

**8.0.6.**
Nennen Sie einige weitere Performance Tipps aus dem Buch "High Performance Javascript", die ind er Vorlesung behandelt wurden.



Typescript (Vortrag Microsoft)
==============================
**9.0.1.**
Was ist Typescript?

**9.0.2.**
Wie unterscheidet sich Typescript grundsätzlich von Dart, der von Google entwickelten Javascript Alternative?

**9.0.3.**
Wo liegt der Hauptvorteil von Typescript gegenüber Javascript?

