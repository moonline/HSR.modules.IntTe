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


