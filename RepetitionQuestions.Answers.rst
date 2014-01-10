======================================
HS13 IntTe Repetitionsfragen Antworten
======================================

Dieses Dokument wird vorzu erweitert. Ergänzungen und Antworten sind herzlich willkommen.
Repetitionsfragen: https://github.com/moonline/HSR.modules.IntTe/blob/master/RepetitionQuestions.rst


1 Java Webtechnologien
======================

**1.0.1 Web Application Request Handling**

1) Client sendet "HTTP Request" an Server
2) Auf dem Server wird daraus ein "HTTP Servlet Request" erzeugt
3) Der WebContainer leitet die Requests an die WebComponents weiter
4) Die WebComponents greifen auf die JavaBeans Components zu. Diese greifen auf weitere Business Logic oder eine Datenbank zu.
5) Die WebComponents erzeugen eine "HTTP Servlet Response".
6) Der Server liefert die Response an den Client aus

:: 
	
	 .-------WebContainer------------.
	 |                               |
	--> HTTP S. Req. --> WebComp.    |
	 |                   /    |      |
	 |                  /     |      |
	 |                 /      |      |
	 |                /       |      |
	 |               /        v      |
	 |              v      JavaB.    |
	<-- HTTP S. Resp.      Comp.     |
	 |                        |      |
	 '------------------------|------'
	                          v
	                          DB


**1.0.2 WebContainer**

Der Webcontainer ist zuständige für die komplette Abarbeitung des Requestes. Er verwaltet und führt Servlets aus. Er Stellt Request Dispatching, Livecylce Management, Security, ... zur Verfügung.


**1.0.3 Web Module**

Ist eine komplette JSF Web Applikation.


**1.0.4 JSF**

Komponenten Framework mit Template Engine, Tag Libraries, ... für den Aufbau komplexerer Web Applikationen.


1.1 Java Servlets
-----------------

**1.1.1 Servlets**

Serverseitige Java Komponente, die dynamisch Web Inhalte erzeugen. Komponenten:


**1.1.2 Inhaltausgabe**

.. code-block:: java

	@WebServlet("/index")
	public class IndexServlet extends HttpServlet {
		public void doGet(HttpServlet request, HttpServletResponse response) throws ServletException, IOException {
			response.setContentType("text/html");
			PrintWriter content = response.getWriter();
			content.print("<html>");
			content.print("<head><title>Example</title></head>");
			content.print("<body><h1>Hello Example</h1></body>");
			content.print("</html>");
			content.close();
		}
	}


Den HTML Content selbst zu generieren ist viel zu unflexibel und fehleranfällig und sollte deshalb auch nicht gemacht werden.


**1.1.3 Konfiguration**

Mit Annotations.

.. code-block:: java

	@WebServlet(displayName = "Example", urlPatterns = { "/example" }, initParams	= { 
		@WebInitParam(name = "param1", value="irgendwas") 
	})
	
	
**1.1.4 Ohne Konfiguration**

Dann werden anhand der Pfade und Klassen die Servlets aufgelöst. Diese müssen entsprechend übereinstimmen.


**1.1.5 Request Details**

1) HTTP Get Request von einem Client
2) Anfrage geht an Servlet
3) Aufruf von Service -> doGet() wird aufgerufen
4) Innerhalb von doGet() wird die Response zusammengebaut


**1.1.6 Formulare**

doPost() muss implementiert werden. Die Parameter können über request.getParameter() abgefragt werden.

.. code-block:: java

	@WebServlet("/index")
	public class IndexServlet extends HttpServlet {
		public void doPost(HttpServlet request, HttpServletResponse response) throws ServletException, IOException {
			String username = request.getParameter("username");
			
			response.setContentType("text/html");
			PrintWriter content = response.getWriter();
			// ...
			content.close();
		}
	}


**1.1.7 Content Type**

.. code.block:: java

	response.setContentType("text/html");
	
	
**1.1.8 Formulare**

.. code-block:: java

	@WebServlet("/index")
	public class IndexServlet extends HttpServlet {
		public void doPost(HttpServlet request, HttpServletResponse response) throws ServletException, IOException {
			String name = request.getParameter("name");
			String address = request.getParameter("address");
			String email = request.getParameter("email");
			
			if(name != null && name != "" && email != null && email != "") {
				this.store(new User(name, street, email));
			}
			
			response.setContentType("text/html");
			PrintWriter content = response.getWriter();
			// ...
			content.close();
		}
		
		private void store(User u) {
			try {
				FileOutputStream fileOut = new FileOutputStream("/tmp/userStorage");
				ObjectOutputStream objectOut = new ObjectOutputStream(fileOut);
				objectOut.writeObject(u);
				objectOut.flush();
				objectOut.close();
			} catch (Exception e) {
				e.printStackTrace();
			}
		}
	}

	
**1.1.9 Benutzerdefinierte Error Pages**

Im web.xml werden benutzerdefinierte Error Pages definiert.

.. code-block:: xml

	<error-page>
		<exception-type>java.lang.Exception</exception-type>
		<location>/view/error.xhtml</location>
	</error-page>
	
	
Die Seite error.xhtml enthält dann den benutzerdefinierten Error-Content.


**1.1.10 Servlet Lebenszyklus**

1) init: Web Container lädt Servlet Klasse und instanziiert sie um anschliessend init() aufzurufen
2) service: Für jeden Client Request wird service() aufgerufen
3) destroy: Aufruf von destroy(), unload der Klasse


**1.1.11 Servlet Context**

Umgebung, in der das Servlet läuft. Bietet Zugriff auf Resourcen.


**1.1.12 Sessions**

Sessions sind Sitzungen, die über Mehrere Requests hinweg leben. Z.B. der Inhalt eines Warenkorbes.

.. code-block:: java

	//  liefert die Session.
	HttpSession session = request.getSession(true);
	
	// liefert Session objekte
	session.getAttribute("shoppingCart");
	
	//  speichert Session Objekte
	session.setAttribute("shoppingCart", cart);

	// session beenden (In web.xml gesetztes Timeout beendet Session ebenfalls, default gibt es jedoch keines)
	session.invalidate();


**1.1.13 Cookies**

.. code-block:: java

	// cookie setzen
	Cookie c = new Cookie("shoppingCart", cartId);
	response.addCookie(c); // muss gesetzt sein, bevor der Content über den Writer eingefügt wird, da es in den Header eingefügt wird
	
	// get cookies
	Cookie[] cookies = request.getCookies();
	
	// read value
	cookies[i].getValue();
	
	
**1.1.14 Request Dispatcher**

Ist zuständig für die Resourcenidentifizierung.

* Ganzen Request weiterleiten -> forward()
* Teilverarbeitung delegieren -> include()


**1.1.15 Filter**

Mit Filtern können Vor- und Nachverarbeitung eines Requestes gemacht werden sowie Header und Dateiinhalt verändert werden.

Filter Mapping
	erfolgt in der web.xml.

	.. code-block:: xml
	
		<filter-mapping>
			<filter-name>Image Filter</filter-name>
			<servlet-name>ImageServlet</servlet-name>
		</filter-mapping>

Filterklasse
	.. code-block:: java
	
		public final class CartFilter implements Filter {
			public void init(FilterConfig filterConfig) throws ServletException { }
			public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) { }
		}
		
		
Verändern der Response
	Implementieren von Klassen, die von ServletRequestWrapper oder HttpServletRequestWrapper bzw.ServletResponseWrapper oder HttpServletResponseWrapper ableiten
		
		
		
2 Clientseitige Technologien
============================

2.1 HTML
--------

**2.1.1 Tags, Attribute**

.. code-block:: html

	<!-- Tags schliessen Inhalte ein und werden mit einem / geschlossen -->
	<h1>Titel</h1>
	
	<!-- Tags die nicht geschlossen werden, werden mit /> geschlossen -->
	<!-- Tags enthalten Eigenschaften als Attribute: -->
	<img src="bild.png" class="pigPicture" />
	
	
2.2 CSS
-------
	
**2.2.2 CSS Stylessheets**

.. code-block:: html

	<head>
		<!-- externes Stylesheet -->
		<link rel="stylesheet" type="text/css" href="mystyle.css">
		
		<!-- inline Stylesheet -->
		<style>
			p { color: red; }
		</style>
	</head>
	<body>
		<!-- inline Style -->
		<h1 style="color: blue; ">Title</h1>
	</body>
	
	
**2.2.3 Selektoren**

Selektoren definieren die Elemente, auf die ein Style angewendet wird.

.. code-block:: css

	/* wird auf alle p Elemente angewendet */
	p { color: red; }
	
	/* wird auf alle Elemente mit der Klasse "vip" angewandt */
	.vip { color: blue; }
	
	/* wird auf Elemente mit der id "first" angewandt */
	#first { color: green; }
	
	/* Selektoren können kombiniert werden. */
	div.gogo { color: black; } 	/* div's mit der Klasse gogo */
	div p { color: yellow; } 	/* p die in der Hierarchie innerhalb eines divs sind */
	div>p { color: orange; }	/* p die direktes Kind von div sind */
	
	
2.3 Javascript
--------------

2.3.1 Grundlagen
................

**2.3.1.1 Sandboxing**

Javascript läuft in einem abgeschotteten Container und hat nur sehr beschränkten und wohlregulierten Zugriff auf Resoucen. Kein I/O.


**2.3.1.2 JS Objektorientierung**

Objekte
	Sind Hash-Tabellen. Objekte werden direkt erstellt nicht anhand von Templates (Klassen). Alles sind Objekte, auch Funktionen.
Prototype
	Es gibt keine Klassen (Templates) sondern nur Objekte. Objekte erben direkt von andern Objekten und nicht von Klassen.
Overloading
	Es gibt kein Overloading. Methoden mit gleichem Namen überschreiben sich trotz unterschiedlicher Parameterlisten.
Sichtbarkeit
	Variablen sind innerhalb der umgebenden Funktion und nicht nur im Block gültig. Kapslung ist nur durch anonyme Funktionsrümpfe möglich, Sichtbarkeitsattribute gibt es nicht.
Typechecking
	Javascript wird interpretiert. Static Typechecking gibt es nicht. Objekte die beim Zugriff nicht existieren werden angelegt.
	

**2.3.1.3 DOM und RenderTree**

DOM
	Das "Document Object Model" ist die interne Abbildung der HTML Seite als Baumstruktur. Javascript besitzt Methoden um diese Struktur zu traversieren.
Render Tree
	Der Elementbaum der grafischen Darstellung.
	
	
**2.3.1.4 White Spaces**

White Spaces ausserhalb der Tags (z.B. Einrückungen, Zeilenumbrüche) werden als eigene Knoten in den DOM aufgenommen.

.. code-block:: html

	<p>Text
		<!-- Greift man über den Dom auf das erste Kind Element des p Tags zu, 
		so erhält man nicht den span sondern einen Whitespance-Tag! -->
		<span>Fett</span>
	</p>
	
	
Beim Traversieren des DOMs muss deshalb immer mit den Knotentypen oder Attributen gearbeitet werden, und nicht mit positionen (erstes Kind, letztes Kind, ...)

__ https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Whitespace_in_the_DOM


**2.3.1.5 Eventhandler**

Eventhandler erlauben das Auslösen von Aktionen Abhängig von Veränderungen durch den User (Mouse, Keyboard) oder Events im DOM.


2.3.2 JQuery
............

**2.3.2.1 JQuery Selektoren**

Funktionieren gleich wie CSS Selektoren, bzw. der document.querySelector(...) von Javascript.

.. code-block:: javascript

	/* Selektiert p's innerhalb von div's mit der Klasse vip und vertauscht 
	ihre sichtbarkeit (visible -> hidden, hidden -> visivle) */
	$('div.vip p').toggle();
	

**2.3.2.2 Sets und Elemente**

JQuery arbeitet immer mit Sets von DOM Elementen, auch wenn es nur eines gibt. Die meissten Operationen werden immer auf allen angewandt. 
Operationen wie val() oder attr(), die etwas zurückliefern arbeiten jeweils mit dem ersten Element.

.. code-block:: javascript

	/* erstes DOM Element aus dem Set abfragen */
	var el = $('div.vip p').get(0);
	
	/* das Element enthält keinen JQuery Wrapper mehr. 
	Für JQuery Operationen muss es erneut gewrappt werden */
	$(el).show();
	
	/* iterieren */
	$('div.vip p').each(function(index, element) {
		/* element or this get the current item */
		console.log($(this).text());
	}


**2.3.2.3 $-Konflikte**

.. code-block:: javascript

	
	/* definieren und sofortiges Auführen einer Anonymen Funktion mit jQuery als Parameter.
	$ ist damit nur innerhalb dieser Funktion gültig und referenziert "jQuery". */
	(function($) {
		/* use JQuery here */
	})(jQuery);
	
	
**2.3.2.4 Attribute**

.. code-block:: javascript

	var attributeValue = $('div.vip p').attr('src'); /* read attribute src */
	$('div.vip p').attr('src', 'image.png'); /* set attribute src */


2.3.3 Vertiefung
................

**2.3.3.1 Datentypen, Objekte, Konstruktoren**

Datentypen
	* number (Floatingpoint): Zahlen, NaN
	* string
	* bool
Objekte sind
	* Funktionen
	* Arrays
	* Date
	* Regex
	* Null
	* Eigene Typen
Konstruktoren
	mit new aufgerufene Funktionen erzeugen neue Objekte
	
**2.3.3.2 Prototype**

Objekte erben direkt von ander Objekten.


Java

	::
		
		class Vehicle <-- class Car extends Vehicle
			|                         |
			v                         v
		concrete vehicle A          concrete Car B
	
	
Javascript

	::
	
		vehicle A <---.        Car B
		               `--- B.prototype
			
			
Prototypemethoden
	* Werden im Prototypobjekt gespeichert
	* Können nicht auf Variablen im Konstruktor zugreifen und somit nicht auf über diesen Weg angelegte private Variablen
Objektmethoden
	* Werden in der Klassenbeschreibung (Template) gespeichert
	* Besitzen Zugriff auf private Member
	
**3.4.4.3 Scoping**

* Variablen (var) werden angehoben und sind in der umfassenden Funktion gültig. Auch wenn sich noch Blöcke dazwischen befinden.
* Ohne var definierte Variablen sind global gültig

**3.4.4.4 this**

This zeigt in Javascript immer auf die umgebende ausführende Funktion.

.. code-block:: javascript

	var f = function() {
		this.name = "abc";
	}
	
	var a = new f();
	// this zeigt auf das Objekt a
	console.log(a.name); // "abc"
	

Wird eine Funktion als Parameter übergeben, so gilt beim Ausführen die Funktion, in der die aufgerufene ausgeführt wird als umgebende.

.. code-block:: javascript

	var c = function(func) {
		func();
	}
	
	c(f); // this von f zeigt auf den globalen space, da dieser die Funktion c umgibt
	
	
**3.4.4.5 Kapselung**

Sichtbarkeitsattribute gibt es nicht. Private Attribute können nur über das Scoping erreicht werden, jedoch mit einigen Nachteilen:

.. code-block:: javascript

	function Container(param) {
		var secret = 3;
		
		this.getSecret = function() {
			return secret;
		}
	}

Nachteile
	* Private Methoden können nicht im Prototype abgelegt werden und werden deshalb in jedes Objekt kopiert
	* Mit Prototype-Methoden können nicht auf solche Variablen zugegriffen werden
	
**3.4.4.6 Closures**

Closures sind Variablen, die Javascript an Funktionen anhängt (unsichtbar), sodass sie noch verfügbar sind, selbst wenn die umgebende Funktion mit ihren Variablen längst nicht mehr exisitert.

.. code-block:: javascript

	var getNumbers = (function() {
		var numbers = [2,3,4];
		
		return function(index) {
			return numbers[index];
		}
	})();
	
	
Wird getNumbers(2) aufgerufen, so wird 3 zurückgegeben, obwohl die äussere Funktion schon längst abgeräumt wurde. die Variable numbers wurde für die innere Funktion in einer Closure gespeichert.

**2.3.3.7 Overloading**

Overloading gibt es in JS nicht. Erneut definierte Methoden mit gleichen Namen überschreiben verherig definierte trotzt unterschiedlicher Signatur.

**2.3.3.8 Anonyme Funktionen**

Dienen dazu, eine Funktion zu definieren und gleich auszuführen. Werden vor Allem zur Kapselung eingesetzt, weil darin verwendete Variablen (var) ausserhalb nicht sichtbar sind.

**2.3.3.9 System Objects**

In JS kann jedes Objekt überschrieben werden. Auch sämmtliche vom System definierte wie window oder navigator. Dies kann dazu benutzt werden, beim Testing ein eigenes Test Environment zu bauen und es anstelle der Systemobjekte zu benutzen.

**2.3.3.10 Eval**

Eval() führt Strings als JS Code aus. Damit ist es möglich zur Laufzeit Programmcode zusammenzubauen und auszuführen. Entsprechend gefährlich ist diese Methode und sollte im Normalfall nicht verwendet werden.

**2.3.3.11 Namespacing**

Mit Objekthierarchien	

	.. code-block:: javascript
	
		window.controller = {}:
		
		window.controller.CarController = function() { /* ... */ }
		window.controller.ReservationController = function() { /* ... */ }
		
		window.domain = {}; windo.domain.model = {};
		window.domain.model.Car = function() { /* ... */ }
		window.domain.model.Reservation = function() { /* ... */ }
	
Übere eine Library, z.B. require.js
	Domain/Model/Car.js:
	
	.. code-block:: javascript
	
		define(function() {
			'use strict';

			var Car = function() { /* ... */ };
			return Car;
		});
		
		
	Main.js:
	
	.. code-block:: javascript
	
		(function() {
			require(["Domain/Model/Car"], function(Car) {
				'use strict';

				var car = new Car();	
			});
		})();
		
		
Die zweite Variante ist zu bevorzugen, da die Includes nicht von Hand nachgeführt werden müssen und nur wirklich benötigte Klassen eingebunden werden.


**2.3.3.12**

a) Ja, da das effektive Objekt erst mit new erstellt wird und dann Vehicle exisitert.
b) 	* Car, car, Vehicle: global Space, da keine umgebende Funktion
	* drive(), turnLightOn(): Vehicle
c) Nur wenn es Getter oder Setter gibt
d) 	* car drive
	* lights ar active
	* undefined
	* Car2
	* siehe h
		
e) Zuerst wird die Funktion im lokalen Objekt gesucht, dann im Prototype, dann in dessen Prototyp, ...
f) Für Car selbst nicht, sie wird jedoch automatisch aufgerufen, da der Interpreter auch im Prototyp sucht
g) getName kann nicht auf die variable name zugreifen, da diese nach aussen nicht sichtbar ist. Darum wird beim ersten Mal undefined ausgegeben.
h) .. code-block:: javascript
	
	Car {
		prototype: Vehicle, 
		drive: drive: function () { return "car drive"; }, 
		name: "Car2", 
		getName: function
		__proto__: Object
	}
		
i) Funktion wird als Funktion aufgerufen und nicht als Konstruktur -> Da die Funktion keinen Rückgabewert besitzt, wird die globale Variable Car mit undefined belegt.
j) .. code-block:: javascript
	
	window.App = {
		Model: {
			Domain: {}
		}
	}; 
	
	window.App.Model.Domain.Car = function() { /* ... */ };
	window.App.Model.Domain.Vehicle = function() { /* ... */ };
		
k) .. code-block:: javascript

	window.App.Controller = {};
	window.App.Controller.VehicleController = (function() {
		car = new Car(); // i
		console.log(car.drive()); // e
		console.log(car.turnLightOn());
		console.log(car.getName()); // g
		car.name = "Car2";
		console.log(car.getName()); // g
		console.log(car); // h
	})();
	
l) .. code-block:: javascript

	window.onload = function() {
		// define VehicleController above without the self extracting function wrapper
		window.App.Controller.VehicleController(); 
	};
	
m) Weil die Methoden in jedes Objekt kopiert werden.
		
**2.3.3.13 JSON**

Ist eine Strukturierte Textdarstellung, in die Objekte abgebildet werden können (JSON.parse(), JSON.stringify()). Es werden allerdings nur die Daten der Objekte abgelegt, keine Funktionen bei JSON.stringify(). 

JSON kann einerseits als Datenformat zur Kommunikation oder Speicherung verwendet werden, andererseits können innerhalb von JS auch Objekte in JSON Notation definiert werden:

.. code-block:: javascript

	var car = {
		name: "Alpha",
		turnLightOn: function() { /* ... */ }
	}
	
**2.3.3.14 new Object()**

Es wird ein Objekt angelegt, das von Object erbt und ansonsten leer ist.

**2.3.3.15 return**

Weil sonst undefined zurückgegeben wird.

**2.3.3.16 Parameterlisten**

In Javascript werden alle Parameter in die variable "arguments" gesteckt die wie ein Array ausgelesen werden kann.


2.4 Ajax
--------

**2.4.0.1 Ajax**

Asynchrones Nachladen von Daten mit Javascript.

* XHR XmlHttpRequest (Asynchrones Laden von HTML/XML)
* On Demand JS (Nachladen von Javascript)
* Iframe nachladen
* Image nachladen

**2.4.0.2 XHR Request**

.. code-block:: javascript

	var req = new XMLHttpRequest();
	req.onreadystate = function() {
		if (req.readyState == 4) {
			if (req.status == 200 || req.status == 304) {
				alert(req.responseText);
			}
		}
	};
	req.open('get', 'url', false);
	req.send(null);

	
Für Cross-Domain XHR muss der Server dies erlauben (Allow im Header).
	
**2.4.0.3 Zustände**

* uninitialized: Request wurde erst definiert, noch nicht geöffnet
* open: Request wurde initialisiert aber noch nicht abgesetzt
* sent: Request wurde abgesetzt
* receiving: Antwortteile sind verfügbar
* completet: Request ist abgeschlossen

**2.4.0.4 Beispiel**



**2.4.0.5 On-Demand JS**

Neuer Script Tag wird in Seite eingefügt und dadurch JS Code geladen. Der JS Code kann auch Daten in JSON Form enthalten.

**2.4.0.6 JSONP**

Das mit On-Demand JS geladene Skript enthält einen Methodenaufruf mit den angeforderten Daten.

.. code-block:: javascript

	loadPersonCallback({ name: "Anton Brauer", age: 27 });
	
	
Das Skript wird ausgeführt, sobald es geladen wurde und ruft damit die Callbackfunktion auf.

**2.4.0.7 JSON vs XML**

JSON kann direkt als Javascript Objekte interpretiert werden und ist einfacher zu transportieren mit On-Demand JS. XML müsste in String gepackt werden.


**2.4.0.8 Ajax -> HTML**

* Als Text übertragenes HTML wird mit innerHTML eingefügt
* als JSON übertragene Daten werden zu HTML zusammengebaut und durch DOM Manipulation eingefügt

**2.4.0.9 XSS**

Cross-Site-Scripting. Eine Lücke erlaubt es einem Benutzer ein Script einzuschleusen, das bei einem andern Benutzer ausgeführt wird. Damit kann z.B. die Session, Zugangsdaten oder Trackinginformation gestohlen werden.

Massnahmen:
* CSP: Mit Content Security Policy Skriptausführungen beschränken -> sehr effektiv
* Escaping von sämmtlichen Eingabeparametern (HTML Charachter replacement) -> kein absoluter schutz
* Parsen von Eingabedaten nach script, scriptinhalten, onclick, etc. -> kein absoluter schutz

**2.4.0.10 Clientseitiges Templating**

Das HTML Template enthält Platzhalter, die durch eine JS Templatengine mit Daten gefüllt werden.

**2.4.0.11 Ajax mit jQuery**

* $.ajax({ url, type, callback, successfunction })
* Type wird über dataType gesteuert


2.4.1 Server Push
.................

**2.4.1.1 Server Push**

* Senden von Daten an den Client durch den Server.
* Problem: HTTP Verbindung muss von Client geöffnet werden und stirbt auch gleich wieder
* Push Server->Client ist nicht vorgesehen

**2.4.1.2 Lösungen**

* Polling
* Long Polling, HTTP Streaming (COMET)
* WebSockets, EventSource (HTML5)
* Socket.io, SignalR (Libraries)



3 REST
======

**3.0.1 REST**

* REpresentational State Transfer
* Resourcen werden über eine eindeutige URL angesprochen
* HTTP Statuscodes werden verwendet

**3.0.2 REST vs SOAP**

* HTTP Status Codes nutzen
* Weniger aufgebläht
* AUch andere Formate als XML möglich

**3.0.3 REST Level**

* 0: Es gibt Service Endpoints, die auf Anfragen Antworten liefern (ähnlich wie SOAP), keine Resourcen, alles POST
* 1: Es gibt Resourcen -> Daten auf dem Server sind über die URL adressierbar (domain.tld/car/1234), alles POST
* 2: Korrekte Verwendung von POST, GET und HTTP Return Codes
* 3: HAETOAS, Der Server schickt in der Antwort Links mit, was mit den Daten gemacht werden kann (Der Server kann intern die Links ändern, ohne das die Clients damit Probleme bekommen), Entwickler verstehen API besser

**3.0.4 Beispiel**



**3.0.5 HAETOAS**

* Hypertext As The Engine Of Application State
* Der Server liefert jeweils eine List mit möglichen Operationen. Der Clien verwendet diese URLs. So kann der Server sie ohne Probleme anpassen.

**3.0.6 GET**

Für verändernde Requests wurde POST, PUT oder DELETE geschaffen. GET verspricht nichts auf dem Server zu verändern. Jeder GET Resquest auf die gleiche URL sollte den gleichen Inhalt zurückiefern.

**3.0.7 Statuslose Kommunikation**

* geringere Kopplung zwischen Client und Server
* Server und Client können zwischen Kommunikation Verbindung oder sich selbst wechseln ohne Probleme
* Anfragen können auf mehrere unabhängige Server verteilt werden.

**3.0.8 Warenkörbe**

* Warenkorb als eigene Rescource (Status als Resourcenzustand)
* Status Clientseitig halten


4 JSF
=====

**4.0.1 JSF**

Java Server Faces: Komponenten basiertes Framework zur serverseitigen Erzeugung von Websites

**4.0.2 MVC Web**

::

	.-------------------.
	|                   |
	|      View         |  Client Side
	|                   |
	'-------------------'
	
	---------------------------------------------------------
	
	.-------------------.
	|                   |
	|    Controller     |  Server Side
	|                   |
	'-------------------'
	
	.-------------------.
	|                   |
	|      Model        |
	|                   |
	'-------------------'
	
	
**4.0.3 JSF Komponenten**

Funktionsbibliotheken, die z.B. Kalender ermöglichen.

**4.0.4 Beans**

Klassen die für jedes Property getter und setter besitzen. JSF benötigt diese, um die Platzhalter im Template mit deren Daten zu füllen.

**4.0.5 Templating**

Das XHTML Template besitzt Platzhalter, in die JSF Daten aus Model und Beans einfüllt und anschliessend die Page rendert.

**4.0.6 JSF Lebenszyklus**

1) Restore View: Komponentenbaum erstellen oder wiederherstellen
2) Apply Request Values: Parameter aus Request extrahieren und in entsprechende Komponenten übernehmen
3) Process Validations: Übergabevariablen der Komponenten werden in interne Darstellung überführt und validiert, anschliessend als Local Value der Komponenten gesetzt -> Validierunsfehlermeldung: Live Cylce springt direkt zu 4
4) Update Model Values: Komponentenbaum wird durchlaufen und aktualisiert (Local Values werden ind Backing Beans kopiert)
5) Invoke Application: Ausführen von Actions
6) Render Response: Komponentenbaum durchlaufen und rendern, Antwortszustand für zukünftige Requests speichern

**4.0.7 Siehe 4.0.6**

**4.0.8 immediate**

Damit lässt sich der Zyklus anpassen. immediate=true bei Steuerkomponenten lässt Actions in "Aply Request Value" Phase ausführen, z.B. für Abbruch bei falschen Parametern

**4.0.9 Facelets**

Standard View Description Language für JSF.

* XHTML
* Tags von Tag Libraries
* Platzhalter (Expresion Language EL)


4.1 UI Komponenten
------------------

**4.1.1 JSF UI Komponenten**

konfigurierbares, wiederverwendbares Element

**4.1.2 UI Komponenten Model**

Komponent enthält Klassen für Komponenten, Rendering, 	EventListening, Datenkonvertierung, Validierung

**4.1.3 Component Tree**

Template wird geparst -> Für Component Tags Komponenten erzeugt und als Baum aufgebaut

**4.1.4 composition & component**

Erstellen von Untertemplates, die als Komponenten verwendet werden können (Wiedervernwendbarkeit)

**4.1.5 Resources**

über #{resource[...]} kann auf Rescourcen im resources Folder zugegriffen werden.

**4.1.6 Attribute**



**4.1.7 Fehlermeldungen**

h:message und h:messages rendern Fehlermeldungen für Komponenten oder die ganze Seite.

**4.1.8 Render-Kit**

Das Render Kit erlaubt das rendern von beliebigem Code. In JSF wird standardmässig das HTML Render Kit verwendet.

Erlaubt Web Autoren das Anpassen der Ausgabe ohne wursteln im Komponentencode. -> Komponenten sollten nie innerhalb der Komponenten gerendert werden.


4.2 Expression Language
-----------------------

**4.2.1 EL**

EL ist eine Sprache zum Zugriff auf Backing Beans. 

.. code-block:: HTML

	<h1>#{customer.name}</h1>
	
	
**4.2.2 Zugriff**

* Beans
* Collections
* Enumeration Types
* Implizite Objekte wie Scope Inhalte, Params, Context, ...

**4.2.3 Scopes**

a) @RequestScoped: Lebt nur für die Dauer eines Requestes
b) @ViewScoped: Lebt in Session solange die gleiche Seite verwendet wird
c) @SessionScoped: Lebt für die Dauer einer Benutzersession
d) @ApplicationScoped: Lebt solange App lebt, ist für alle Benutzer gleiche

**4.2.4 EL innerhalb Klassen**

.. code-block:: java

	value = "#{resource[...]}";
	

**4.2.5 EL Methodenaufruf**

.. code-block:: HTML

	<f:link action="#{customer.save(customer.id)}" >save</f:link>
	
	
**4.2.6 implizite Objekte**

Objekte wie Scope Inhalte, Params, Context, ... . Stellen Informationen über das Environment und den Request zur Verfügung.


4.3 Converter
-------------

**4.3.1 Convert**

Werden verwendet zur Konvertierung von Daten zwischen localView der Bean und der Presentationview.

.. code-block:: HTML

	<h:outputText value="#cashier.shipDate}">
		<f:convertDateTime pattern="dd.MM.yyyy" />
	</h:outputText>
	

**4.3.2 Converter Sichten**

* Model View (local Value)
* Presentation View

**4.3.3 custom Converter**

* Regisitrierung mit Converter-Block im web-xml
* aufruf mit converter="MyConstomConverter" in einer Komponente
* Klasse implementieren, die Converterinterface implementiert (getAsObject(), getAsString()).


4.4 Validatoren
---------------

**4.4.1 Validatoren**

Validatoren dienen zur Validierung von Eingabedaten. Z.B. min / max bei RangeInput. Es gibt Standardvalidatoren für Wertlängen, Ranges, Required-Fields, Regex, ... .

**4.4.2 Cutom Validator**

* Registirierung im web-xml mit einem Validator-block
* Verwenden als Custom Tag innrhalb eines Input Feldes.
* Implementieren einer Klasse, die das ValidatorInterface implementiert (validate()).

**4.4.3 Bean Validation**

Validierung im Template ist teilweise redundant, da sie in den Beans wieder vorkommt. Deshalb ist Validierung innerhalb der Bean Klassen mit Annotations besser. -> Constraints


4.5 EventListener
-----------------

**4.5.1 EventListener**

EventListener regieren auf Events im UI, in der Applikation oder im Model.

**4.5.2 Begriffe**

EventObject
	Komponente, die den Event auslöst
Value Change Event
	Wert einer Input Komponente hat sich verändert.
Action Event
	Eine Action wurde ausgelöst
Data Model Event
	Event im Datenmodel, z.B. erhöhen eines Wertes.

**4.5.3 Event Handling Lebenszyklus**

1) Events werden in Queue eingereiht
2) Am Ende jeder JSF Zyklus Phase werden die Eventlistener aufgerufen
3) EventHandler können duch Context.renderResponse() oder Contect.responseComplete() den Zyklus abkürzen

**5.4.4 EventListener registrieren**

Mit MethodExpression in valueChangeListener-Attribut oder einem KindTag f:valueChangeListener


4.6 Internationalisierung
-------------------------

**4.6.1 Bundle Einbinden**

Im web-xml locale-config und resource-bundle Blöcke einfügen.

**4.6.2 Browsereinstellungen übersteuern**

Mit f:view locale="..."

**4.6.3 Bundlezugriff in Bean**

.. code-block:: java

	ResourceBundle.getBundle(
		context.getApplication().getMessageBundle(), 
		context.getViewRoot().getLocale()
	);


4.7 Ajax
--------

**4.7.1 Ajax mit JSF**

JSF lädt im Hintergrund die Entprechenden Daten nach und ersetzt die entsprechenden Teile in der View.

**4.7.2 Beispiel**

.. code-block:: HTML

	<f:ajax event="keyup" render="cars" />
	<h:outputText id="cars" ... ></h:outputText>
	
**4.7.3 Events**

action, valueChanged, mouseOut, mouseOver, ...

**4.7.4 JS API**

* onchange="jsf.ajax.request(this,event, {render:'options'});"
* jsf.js muss im Header eingebunden sein
* nebst reqest gibt es noch response, addOnError und addOnEvent


5 Web Architektur
=================

**5.0.1 Web App Architektur**

::

	        .-------------------------------------------.
	        |              HTML / CSS Views             |
	Browser |------------^--------+---------------------|
	        |            |        | ViewModels / Contro.|
	        |            |        |---------------------|
	        |            '        |       Models        |
	        |             \       |---------------------|
	        |              \      |     Data Service    |
	--------+---------------\-----+----------^----------+----------
	                         \               |
	                        html           json
	                           \             |
	--------+-------------------v------------v----------+----------
	        |    Presentation (Web-UI oder Web-API)     |
	        |-------------------------------------------|
	Server  |             Business Layer                |
	        |-------------------------------------------|
	        |               Data Layer                  |
	        '-------------------------------------------'


**5.0.2 Client- / Serverzentrierte App**

Clientzentriert
	* Server bietet eine Web-API an
	* Server ist sehr schlank und kümmert sich nur um Daten
	* Client besitzt MV* Framework und baut das UI auf
Serverzentrierte Architktur
	* Server ist ziemlich gross und rendert das UI
	* UI wird direkt an den Client gesendet
	* Clientseitig keine MV* Architktur
	
**5.0.3 Web Frameworks**

Action/Request based
	* Requests lösen Actions aus
	* HTTP direkt verwendet
	* Einfacher MVC Control Flow
Component based
	* UI aus Komponenten aufgebaut
	* HTTP wird abstrahiert verwendet
	* komplexes MVC
	* Wiedervernwendbare Komponenten
	

5.1 Patterns
------------

**5.1.1 Patterns**

Template View
	Prinzip
		* Ein Template wird mit Daten gefüllt
		* Template und Daten werden unabhängig definiert
	Two Step View
		1) Model Daten in einen logische Präsentationsstruktur (Formatunabhängig) überführen
		2) Präsentationsstruktur als spezifisches Format rendern
	Umsetzung
		PHP
			.. code-block:: php
				
				<h1><?php echo $title ?></h1>
				
				
		ASP.NET
			.. code-block:: HTML
			
				<h1><asp:Label runat="server" id="title" /></h1>
				
		JSF
			.. code-block:: HTML
			
				<h1>#{welcome.title}</h1>
				
				
	EL
		Expression Language sieht in allen Sprachen unterschiedlich aus, der Funktionsumfang ist meistens jedoch relativ ähnlich. Geboten werden Zugriff auf Model / Daten und UI Features.
MVC Web
	Konzept
		* Unterschied zur klassischen MVC Architktur: UI wird nicht von Controller über Observer über Änderungen informiert
		* Request / Response basiert -> UI Generation ist Response		
Front Controller
	* ein einziger Input Controller, aufgespalten in Handler und Commands
Page Controller
	* Ein Controller für jede Web Page
	
**5.1.2 ROCCA Architektur**

Nutzung von

* REST
* HTTP
* TLS Authentication
* Cookies nur für Authentifizierung

Beachten von

* Accessibility
* JS Free usable Frontend
* No Business Logic duplication



6 Client Architektur Frameworks
===============================

**6.0.1 Botstrap, Modernizr**

Modernizr
	Feature Detection zur selection was für Funktionen dem Benutzer angeboten werden können und welchen nicht
Bootstrap
	Responsive Layout, Komponenten
	
	
**6.0.2 jQuery Mobile**

Einfach Bauen von Apps, die wie native Apps wirken. Übernimmt Routing, Rendering. Touchoptimierung.


**6.0.3 MVVM**

Model View ViewModel: Anstelle des Controllers bei MVC tritt ein ViewModel, das Aggregierte und verarbeitete Daten für die View bereithält.


**6.0.4 Templating mit Dot.js**

* Im Template werden Platzhalten verwendet.
* Das Template wird eingelesen
* Template wird Kompiliert
* Template wird mit ViewModel "befüllt"
* View wird gerendert und rausgeschrieben.


**6.0.5 Backbone**

Hilft die App zu strukturieren. Bringt Routing, Templatengine, Server Connection Support.



7 Plugin Technologien
=====================

**7.0.1 JS WebApp Alternativen**

* Native App
* Browser Plugin
* Native->JS Compiler
* JS Erweiterung (TypeScript)


7.1 Browser Plugins
-------------------

**7.1.1 Java Applets**

Vorteile
	* Entwicklung mit Java
Nachteile
	* Sicherheitslöcher
	* Viele Benutzer erlauben keine Applets mehr
	* Sicherheitswarnung verwirrt Benutzer
	* Applet hat zu viele Rechte auf Client
	* API wird Ende 2013 abgeschaltet


**7.1.2 NPAPI**

Alle Plugins, die über die "Netscape Plugin API" laufen werden nicht mehr lauffähig sein (z.B: Java Applets).


**7.1.3 Nicht installierte Plugins**

1) Browser zeigt Platzhalter für das Plugin an und meldet ein fehlendes Plugin
2) Der Benutzer wird aufgefordert das Plugin zu installieren
3) Die Meldung linkt den Benutzer auf die Downloadseite, wo der Benutzer das Plugin findet
4) Wird der Browser und das BS unterstützt, kann der Benutzer das Plugin herunterladen und installieren
5) Der Benutzer kann mit dem Benutzen der Seite mit den Plugin Inhalten fortfahren


7.2 Silverlight
---------------

**7.2.1 Silverlight**

* Databinding, Integration mit exist. .NET Anwendungen
* Entwickeln von WebAnwendungen mit .NET Skills
* Transport vielfälltiger als bei HTTP only


**7.2.2 SL Probleme**

* Plugin nicht für alle Plattformen verfügbar
* Business-to-Consumer Plattforms
* Mobile Plattforms
* Inhouse APPs


**7.2.3 SL Architektur**

* homogen: Server & Client gleiche Technologie
* heterogen: Server & Client unterschiedlich (Server z.B. REST API)


**7.2.4 Vorteile homogene Architektur**

* Entwicklung in einem Guss
* Engenere Kopplung möglich -> Protokoloptimierungen
* Kein Zusatzaufwand für API, Technologie übernimmt dies


**7.2.5 ungeeignete SL Anwendungen**

* Mobile Web App's
* Zielgruppe vorwiegend Linux oder Mac (meisst SL Plugin nicht installiert)
* Schlanke Applikationen


**7.2.6 SL Anwendung aufbauen**

* Generieren eines Clients anhand von Database & Models
* Validiert werden sollte auf allen Layern, Eingabedatenvalidierung erfolg direkt auf dem Client


7.3 Flash
---------

**7.3.1 Flash**

* Flash nutzt nicht die NPAPI sondern die PPAPI
* Flash wurde vor Allem durch Flash Videos verbreitet -> jeder hatte Flash, endlich konnte man alle Videos abspielen
* Games


**7.3.2 Flash migration**

* Adobe Tools: ActionScript to JS Compilation


7.4 Cross-Compilation
---------------------

**7.4.1 Google Web Toolkit GWT**

* Compiling von Java App to Browser & Locale Specific JS
* Web UI Class Library
* Unit Testing
* Testing without Compiling
* Navigation & History Managemet


**7.4.2 GWT UI**

Es müssen die GWT eigenen UI Klassen verwendet werden, sonst kann die App nicht compiled werden.


**7.4.3 GWT RPC**

GWT bietet RPC an, aber nur für ein Subset von Typen.


**7.4.4 GWT Einschränkugnen**

* GWT Web UI Beschränkung
* Kein Multithreading
* Kein dynamic Class Loading
* GWT Kompiliert für jeden Browser und jede Lokalisierung eine eigene App -> Neue Browser müssen zuerst von GWT supported werden


**7.4.5 GWT Cross-Browser**

Durch die Browser spezifischen Editions werden die einzelnen Editions sehr schlank und laufen optimal im entsprechenden Browser. JQuery schleppt z.B. IE Optimierungen mit sich herum, auch wenn es in einem FF läuft.


**7.4.6 Vorteil Cross-Compilation**

* Läuft in fast jedem Browser
* Benutzer müssen kein Plugin installieren
* Benutzer blockieren JS eigentlich nie, Plugins jedoch immer mehr



8 Performance Optimierung
=========================

**8.0.1 Page Loading**

1) Dokument laden
2) Dekomprimieren
3) Lexing
4) Pasing, DOM Aufbau
5) Ausführen von Scripts, DOM Manipulation
6) Layout generation
7) Rendering


**8.0.2 DOM Manipulations**

* Jedes Mal wird der DOM verändert, das Layout angepasst, der RenderTree neu aufgebaut und die Darstellung neu gerendert.
* Besser wäre das Einfügen von allen Elementen auf's Mal -> nur einmal Rendern
	* schlecht: create parent, insert parent, add child1, add child2, add child3
	* gut: create parent, add child1, add child2, add child3, insert parent
	
	
**8.0.3 Element Placement**

a) CSS: Header, damit es vor der Struktur gerendert wird (verhindert Flackern)
b) LESS: Header, vor Script, damit es bereits geladen wurde
c) LESS Script: Header, damit das CSS compiled wurde bevor der Content geladen wird
d) 	I) Domain Logic on startup: Header
	II) Domain Logic über require.js: Am Ende der Seite
e) main Script: Ende der Seite


**8.0.4 Positioning Alternativen**

HTML bietet das "defer" Attribut, das den Browser dazu anhält, das Skript erst aufzuführen wenn die Seite fertig geparst wurde.


**8.0.5 Content First**

* Für das Ausführen der Skripte wird der Content benötigt -> Inhalte sind möglicherweise noch gar nicht vorhanden
* Parsen der Skripte verzögert den Content.


**8.0.6 Performance Tipps**

* Unbenutzten Code (CSS/JS) entfernen
* Skripts und CSS minifien
* CSS in header, JS at Bottom
* CDN (Content Delivery Network) nutzen
* DOM
	* DOM Refs in lokale Variablen cachen
	* DOM nicht iterativ manipulieren
	
	
9 Typescript
============

**9.0.1 Typescript**

Typescript ist eine Scriptsprache, die zu JS compiliert wird und sich an die Syntax zukünftiger Emacscript Drafts hält. 


**9.0.2 Dart**

* Dart ist eine komplett eigene Sprache, für die es auch einen eigene Laufzeitumgebung (DartVM) gibt. Dart kann allerdings auch nach JS Cross-compiled werden.
* Für Typescript wird keine eigene Laufzeitumgebung benötigt da es darauf ausgelegt ist, nach JS compiliert zu werden.


**9.0.3 Vorteile**

* Static Type checking
* Classes, Interfaces
* Class-based Inheritance



