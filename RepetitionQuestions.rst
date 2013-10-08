===============================
HS13 IntTe Repetitionsfragen
===============================


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
Wann erhalten Sie in jQuery eine Liste und wann direkt das gesuchte Objekt? Wie greifen Sie in der List auf das erste Element zu? Wie iterieren Sie über die Element?

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

