class: middle
task: asd
# Grundlagen
1. [Strict Mode](#grundlagen-strict-mode)
2. [Zahlen](#grundlagen-zahlen)
3. [Implizite Typumwandlung](#grundlagen-implizit)
4. [Literale Schreibweise statt Konstruktor](#grundlagen-literal)
5. [Vergleichen von unterschiedliche Typen nie mit ==](#grundlagen-vergleich)
6. [JavaScript ergänzt automatisch Semikolons](#grundlagen-semikolon)
7. [JavaScript nutzt 16-Bit Unicode](#grundlagen-unicode)


---
template: default
layout: true
#### .bobo[Grundlagen]


---
name: grundlagen-strict-mode
### Strict Mode
**Definition:**
- Der Strict Mode ist ein bereinigtes JavaScript Subset. Er führt nichts neues ein, entfernt aber einige problematische Altlasten

**Was macht der Strict Mode?:**
- Das with-Statement wird abgeschafft
- Es wird ein Fehler ausgelöst, wenn durch ein vergessenes var unbeabsichtigt eine globale Variable angelegt wird
- Die befremdlichen Eigenheiten von this, eval() und arguments wurden reduziert
- Unlogische Anweisungen wie NaN = 17 lösen einen Fehler aus
- Die oktale Syntax (0644 == 420) ist verboten


---
### Strict Mode 
**Voraussetzung für funktionierenden Strict Mode:** 
- Deklaration am Anfang der Datei
- Deklaration am Anfang des Skriptes
- Deklaration am Anfang der Funktion
- Bestimmung der JS-Version auf der die Anwendung lauffähig sein soll 
- Browser ohne Unterstützung ignorieren den String
- Testen nur  in Umgebungen, die den Strict Mode unterstützen

**Vorsicht, mögliche Fehlerquelle!:**
- Beim Verketten von Skripten, die unterschiedliche Modi erwarten, sollte Strict Code in IIFEs / AMD / CommonJS  Module gekapselt werden

**Konstruktorfunktionen á Pseudoklassen**
- this ist bei einfachen Funktionsaufrufen undefined anstatt window.
- Dadurch wird die versehentliche falsche Behandlung von Methoden als Funktionen erschwert, da der Versuch, auf Methoden von undefined zuzugreifen, zu einem Fehler führt


---
### Strict Mode
**Beispiele:**


---
name: grundlagen-zahlen
### Zahlen
- Fließkommazahlen mit doppelter Genauigkeit (64-Bit-Codierungen Zahlen nach IEEE 754)
- Integer in JavaScript sind lediglich eine Teilmenge der Fließkommazahlen doppelter Genauigkeit und kein eigenständiger Datentyp
- Wertebereich Integer (-253 und 253)
- Bitweise Operatoren behandeln Zahlen als 32-Bit-lnteger mit Vorzeichen
- Arbeitsweise Bitweise Operator: Fließkommazahl --> Konvertierung in Integer --> Konvertierung in Integer


---
name: grundlagen-implizit
### Implizite Typumwandlung
- NaN ist der einzige Wert, der als ungleich mit sich selbst behandelt wird (NaN === NaN --> false)
- isNaN führt eine implizite Typumwandlung des Arguments in eine Zahl vor dem Test durch
- Der +-Operator ist überladen (Addition & Stringverkettung)
- Objekte werden mit  valueOf in Zahlen und mit toString in Strings umgewandelt
- Objektliterale mit einer valueOf-Methode sollten eine toString Methode implementieren, die eine Stringdarstellung der von valueOf ausgegebenen Zahl liefert.
- Als Test auf undefinierte Werte sollten typeof oder ein Vergleich mit undefined verwenden werden und keine Truthiness-Prüfung


---
name: grundlagen-literal
### Literale Schreibweise statt Konstruktor
- Objektwrapper für primitive Datentypen weisen beim Test auf Gleichheit nicht dasselbe Verhalten auf wie die entsprechenden primitiven Datentypen
- Beim Abrufen und Setzen von Eigenschaften für primitive Datentypen werden implizit Objektwrapper erstellt
- Implizite Wrapperstellung: JS wandelt primitive Typen in ihre Objektpendants um. Dadurch können die Methoden zur Verfügung gestellt werden (Bsp: „string“.toUpperCase())


---
name: grundlagen-vergleich
### Vergleichen von unterschiedliche Typen nie mit ==
- Der Operator == wendet nicht unmittelbar einsichtige Konvertierungen an, wenn seine Argumente von unterschiedlichem Typ sind
- Verwenden von ===, um den Lesern des Codes klarzumachen, dass bei dem Vergleich keine impliziten Typumwandlungen stattfinden
- Verwendung von Werten unterschiedlichen Typs durch selbst geschriebene explizite Konvertierungen, um das Programmverhalten deutlicher zu machen


---
name: grundlagen-semikolon
### JavaScript ergänzt automatisch Semikolons
1. Semikolons werden in folgenden Situationen automatisch eingefügt:
	- vor einem } -Token
	- nach einem oder mehreren Zeilenumbrüchen
	- am Ende eines Block oder der Programmeingabe 

1. Semikolons werden nur dann automatisch ergänzt, wenn das nächste Token nicht geparst werden kann
	- die Semikoloneinfügung ist ein Mechanismus zur Fehlerkorrektur
	- Folgenden Eingabetoken werden geparst (, [, +, und / --> Es wird somit kein Semikolon eingefügt

1. Semikolons werden niemals als Trennzeichen im Kopf einer for-Schleife oder als leere Anweisungen eingefügt
	- Semikolons werden nur dann automatisch ergänzt, wenn das nächste Token nicht geparst werden kann
	- Beim Verketten von Skripten ausdrücklich Semikolons zwischen den einzelnen Dateien einfügen
	- Schleifen mit leerem Rumpf erfordern ein ausdrückliches Semikolon
	- Wegen der Restricted Productions niemals einen Zeilenumbruch vor den Argumenten return, throw, break, continue, ++ und einfügen


---
name: grundlagen-unicode
### JavaScript nutzt 16-Bit Unicode
- JavaScript-Strings bestehen aus 16-Bit-Codeeiheiten, nicht aus Unicode-Codepunkten
- Die Unicode-Codepunkte ab 216 werden in JavaScript durch zwei Codeeinheiten dargestellt, die zusammen ein sogenanntes Ersatzpaar bilden.
- Ersatzpaare verschieben die Elementzählung und beeinträchtigen das Funktionieren von length, charAt, charCodeAt und Mustern für reguläre Ausdrücke wie “.“
- Um Strings auf der Ebene der Codepunkte zu bearbeiten, verwenden Sie Bibliotheken von Drittanbietern
- Wenn Sie eine Bibliothek für Stringoperationen verwenden, sollten Sie sich ihre Dokumentation ansehen, um herauszufinden, wie sie mit dem vollständigen Bereich der Unicode-Codepunkte umgeht



<!-- ######################### Kapitel 2 ############################ -->

---
layout: false
class: middle
# Gültigkeitsbereiche
1. [Globales Objekt](#valid-global)
2. [Vermeiden von with](#valid-with)
3. [Closures](#valid-closurest)
4. [Literale Schreibweise statt Konstruktor](#grundlagen-literal)
5. [Vergleichen von unterschiedliche Typen nie mit ==](#grundlagen-vergleich)
6. [JavaScript ergänzt automatisch Semikolons](#grundlagen-semikolon)
7. [JavaScript nutzt 16-Bit Unicode](#grundlagen-unicode)

---
template: default
layout: true
#### Gültigkeitsbereiche


---
name: valid-global
### Globales Objekt
- Vermeiden von globalen Variablen, lokale sind besser
- Vermeiden, Eigenschaften zum globalen Objekt hinzuzufügen
- Das globale Objekt eignet sich, um den Funktionsumfang einer Plattform zu bestimmen (dynamische Reflektion)
- Deklarieren einer neuen lokale Variablen immer mit var
- Verwenden von Lint-Tools, um nach ungebundenen Variablen zu suchen


---
name: valid-with
### Vermeiden von with
- Ein with-Block durchsucht die ganze Prototypen Kette nach der jeweiligen Methode / Eigenschaften 
- schlechtere Performance
- Der Konflikt zwischen dem lexikalischen Gültigkeitsbereich der Variablen und dem Objekt-Namespace machen with-Blöcke extrem labil


---
name: valid-closures
### Closures
**Funktionsweise JS Funktionen:**
- JavaScript Funktionen enthalten mehr Informationen als nur den Code. 
- Sie speichern intern jegliche Variablen, die im umschließenden Gültigkeitsbereichen definiert sind und auf die sie vielleicht referenzieren müssen. 
- Eine Funktion kann auf alle Variablen in ihrem Gültigkeitsbereich verweisen, auch auf die Parameter und Variablen äußerer Funktionen. 

**Definition:**
- Eine Closure ist demnach eine Funktion, die die Variablen der umschließenden Gültigkeitsbereiche nachverfolgt.
- Closures speichern intern Verweise auf ihre äußeren Variablen und können diese Variablen sowohl lesen als auch ändern.


---
name: valid-closures
### Closures
**Closures haben vier wichtige Kernkonzepte:**
1.	Zugriff auf Variablen außerhalb einer Funktion:
JavaScript erlaubt, auf Variablen zu zugreifen, die außerhalb der vorliegenden Funktion definiert sind
2.	Closures können die erzeugende Funktion überleben: 
Die zweite wichtige Tatsache ist, dass Funktionen selbst dann noch die erzeugenden auf Variablen verweisen können, die in äußeren Funktionen definiert Funktionen überleben. sind, wenn die äußeren Funktionen die Steuerung bereits zurückgegeben haben
 JavaScript Funktionen sind Objekte erster Klasse
3.	Äußere Variablen können geändert werden.:
Closures können Verweise auf äußere Variablen speichern, anstatt deren Werte zu kopieren. Daher sind die Änderungen für alle Cloures sichtbar, die Zugriff darauf haben
4.	Closures speichern nicht die Werte ihrer äußeren Variablen, sondern nur einen Verweis darauf


---
name: valid-hoisting
### Hoisting
- Funktionsdeklarationen werden zuerst hochgezogen, danach die Variablen
Variablendeklarationen in einem Block werden an den Anfang der einschließenden Funktion gehoben
- Deklaration wird hochgezogen, die Zuweisung bleibt an Ort und Stelle!
- Neudeklarationen (wieder mit var) von Variablen werden als eine einzige Variable behandelt. (manuelles Hoisting am Programmanfang ist eine Lösung)
- Keine Blockgültigkeitsbereiche (in ES6 mit let möglich) 
- In einer try -catch  Struktur wird eine abgefangene Ausnahme an eine Variable gebunden, deren Gültigkeitsbereich nur den catch-Block umfasst


---
name: valid-iife
### Lokale Gültigkeitsbereiche durch IIFES
- Beim Betreten eines Gültigkeitsbereichs zur Laufzeit wird im Arbeitsspeicher für jede Variablenbindung in diesem Gültigkeitsbereich ein Speicherplatz (Slot) zugewiesen
- Jede lokale Variable bekommt einen Slot zugewiesen  Bindung
- IIFEs benutzen um lokale Gültigkeitsbereiche zu erstellen

**Achtung!,  das Verhalten eines Blocks kann sich ändern kann, wenn eine IIFE erstellt wird**
- Der Block kann keine break und continue Anweisungen mehr enthalten
- Sondervariablen wie this und arguments haben dann eine andere Bedeutung


---
name: valid-fnexpression
### Gültigkeit von benannten Funktionsausdrücken
- Verwenden von benannte Funktionsausdrücke, um mehr Nutzen aus Stack-Spuren in Fehlerobjekten und Debuggern zu ziehen
- Achten auf die mögliche Verunreinigung des Gültigkeitsbereichs von Funktionsausdrücken durch Object.prototype in ES3 und fehlerhaften JavaScript-Umgebungen, da der Gültigkeitsbereich von benannten Funktionsausdrücken als Objekt behandelt wurde
- Achten auf das mögliche Hoisting und doppelte Zuweisung von benannten Funktionsausdrücken in fehlerhaften JavaScript Umgebungen
- Vermeiden von benannten Funktionsausdrücke oder entfernen Sie sie vor der Auslieferung
- Wenn Sie Ihren Code nur in korrekt implementierten ESS-Umgebungen bereitstellen, müssen Sie sich keine Sorgen machen


---
name: valid-fndeclaration
###	Verlässliche Gültigkeitsbereiche von lokalen Funktionsdeklarationen
- Platzieren Sie Funktionsdeklarationen immer auf der äußersten Ebene eines Programms oder der einschließenden Funktion. Damit vermeiden Sie ein unerwartetes Verhalten der JavaScript Engine.
- Verwenden Sie var-Deklarationen mit bedingten Zuweisungen anstelle von bedingten Funktionsdeklarationen


---
name: valid-evil-1
### Vermeiden mit eval Variablen zu erstellen
Ein weiteres Problem bei eval besteht darin, dass es für Hochleistungs-Engines schwerer ist, Code zu optimieren, der in einem String steht, da der Compiler für diesen Vorgang nicht rechtzeitig auf den Quellcode zugreifen kann. Ein Funktionsausdruck kann zum gleichen Zeitpunkt kompiliert werden wie der Code, in dem er steht, weshalb er eher der Standardkompilierung unterliegt.
- Vermeiden Sie es, mit eval Variablen zu erstellen, die den Gültigkeitsbereich des Aufrufers verunreinigen
- Wenn die Gefahr besteht, dass eval -Code globale Variablen an legt, stellen Sie den Aufruf in eine verschachtelte Funktion, um eine Verunreinigung des Gültigkeitsbereichs zu verhindern.
- Eval kapselt im Vergleich zu Closures keine variablen (schlecht bei asynchroner Programmierung wegen den Callbacks)


---
name: valid-evil-2
###	Eval eher indirekt verwenden
- Stellen Sie eval in eine Ausdrucksfolge mit einem sinnlosen Literal, um den indirekten Aufruf zu erzwingen
- Bevorzugen Sie nach Möglichkeit den indirekten Aufruf von eval gegenüber dem direkten.


<!-- ######################### Kapitel 4 ############################ -->

---
class: middle
layout: false
# Funktionen
1. [Unterschied zwischen Funktionen, Methoden und Konstruktoren](#fn-diff)
2. [Funktionen höherer Ordnung](#fn-higherfn)
3. [Call – Funktionen mit benutzerdefinierten Empfänger](#fn-call)
4. [Apply – Variadische Funktionen](#fn-apply)
5. [Das arguments Object](#fn-arguments)
6. [Arbeiten mit der bind Methode](#fn-bind)
7. [Code mit Closures Kapseln anstatt mit Strings](#fn-kapseln)
8. [Reflektion des Codes von Funktionen mit toString](#fn-toString)
9. [Vorsicht bei der Inspizierung des Call Stack](#fn-callstack)


---
template: default
layout: true
#### Funktionen


---
name: fn-diff
### Unterschied zwischen Funktionen, Methoden und Konstruktoren
- Beim Aufruf einer Methode für ein bestimmtes Objekt wird die jeweilige Methode nachgeschlagen und dann das aufrufende Objekt als Empfänger (=this Bindung) der Methode verwendet. (Der Aufrufausdruck selbst bestimmt die Bindung von this)
- Funktionsaufrufe stellen das globale Objekt als Empfänger zur Verfügung (oder undefined bei strengen Funktionen (strict mode)).
- Der Aufruf von Methoden als Funktionsaufruf ist nur selten sinnvoll
- Konstruktoren die mit new aufgerufen werden erhalten ein neues Objekt als Empfänger. Das neue Objekt wird implizit als Ergebnis zurückgegeben.


---
name: fn-higherfn
### Funktionen höherer Ordnung
- Funktionen höherer Ordnung nehmen andere Funktionen als Argumente an oder geben sie als Ergebnis zurück.
- Machen Sie sich mit den Funktionen höherer Ordnung in den bestehenden Bibliotheken vertraut.
- Lernen Sie häufig vorkommende Codemuster zu erkennen, die sich durch Funktionen höherer Ordnung ersetzen lassen


---
name: fn-call
### Call – Funktionen mit benutzerdefinierten Empfänger
- Verwenden Sie die Methode call, um eine Funktion mit einem benutzerdefinierten Empfänger aufzurufen
- Verwenden Sie die Methode call, um Methoden aufzurufen, die in dem gegebenen Objekt möglicherweise nicht existieren
- Verwenden Sie die Methode call zur Definition von Funktionen höherer Ordnung, die Clients erlauben, einen Empfänger für den Callback bereitzustellen


---
name: fn-apply
### 4.	Apply – Variadische Funktionen
- Verwenden Sie die Methode apply, um variadisehe Funktionen mit einem berechneten Array von Argumenten aufzuführen
- Geben Sie im ersten Argument von apply einen Empfänger für variadische Methoden an


---
name: fn-arguments
### Das arguments Object
**Erstellen von variadischen Funktionen mit dem arguments Object**
- Implementieren von variadische Funktionen mit dem impliziten arguments-Objekt
- Eine variadische Funktion sollte eine zusätzliche Version mit fester Argumentanzahl bereitstellen, damit die Verbraucher nicht auf die Methode apply zurückgreifen müssen

**Arguments Objekt nicht ändern**
- Nie das arguments-Objekt bearbeiten / verändern
- Das arguments-Objekt per mit [].slice. call(arguments) in ein echtes Array umwandeln

**Verweise auf arguments mittels einer Variable**
- Bei Verweisen auf arguments auf die Verschachtelungsebene der Funktionen achten
- Der Gültigkeitsbereich ist die einschließende Funktion


---
name: fn-bind
### Arbeiten mit der bind Methode
**Extrahieren von Methoden mit festen Empfänger mittels bind**
- Bind erstellt immer eine neue Funktion
- Beim Aufruf einer Methode wird der Empfänger durch die Art und Weise des Aufrufs bestimmt
- Wenn Methoden eines Objekts an eine Funktion höherer Ordnung übergeben werden, ist eine anonyme Funktion, um die Methode mit dem geeigneten Empfänger aufzurufen, die beste Lösung.
- Verwenden von bind als eine Abkürzung, um eine Funktion zu erstellen, die an den gewünschten Empfänger gebunden ist

**Bind beim Currying verwenden**

- **Definition:** Diese Technik, eine Funktion an eine Teilmenge ihrer Argumente zu binden, wird als Currying bezeichnet.
- Bind beim Currying von Funktionen verwenden, um eine delegierende Funktion mit einer festen Teilmenge der erforderlichen Argumente zu erstellen
- Beim Currying einer Funktion, die keinen Empfänger braucht, null oder undefined als Empfängerargument übergeben


---
name: fn-kapseln
### Code mit Closures Kapseln anstatt mit Strings
- Lokale Verweise sollten niemals per Strings implementiert werden, wenn die Auswertung der APi mit eval erfolgt
- APis die Funktionen zum Aufrufen annehmen sind besser als Strings mit eval auswerten


---
name: fn-toString
### Reflektion des Codes von Funktionen mit toString 
- Keine Definition im Standard, dass toString genaue Reflektionen des Quellcodes von Funktion ausgeben
- Deshalb können die Einzelheiten des Funktionsquellcodes in den verschiedenen Engines sehr unterschiedlich ausfallen
- Im Ergebnis von toString werden die in einer Closure gespeicherten Werte der lokalen Variablen nicht wiedergegeben
- Die Verwendung von toString für Funktionen sollte vermieden werden.


---
name: fn-callstack
### Vorsicht bei der Inspizierung des Call Stack
- Über arguments.callee können anynome Funktionen sich selbst aufrufen
- Grundsätzlich sollten die nicht standard konformen Implementierungen von arguments.callee und arguments.caller nicht verwendet werden
- Die standard konforme Funktioneigenschaft caller bietet keine zuverlässigen Informationen über den Stack und sollte deshalb auch nicht verwendet werden
- Alle hier genannten Möglichkeiten sind im Strict Mode nicht möglich


<!-- ######################### Kapitel 4 ############################ -->

---
class: middle
layout: false
# Objekte und Prototypen
1. [Definition](#obj-definition)
2. [Unterschied zwischen prototype, getPrototpeOf und \_\_proto\_\_](#obj-proto)
3. [Object.getPrototypeOf() und \_\_proto\_\_](#obj-compare)
4. [Niemals \_\_proto\_\_ ändern](#obj-__proto__)
5. [Erstellen von Konstruktoren die auch ohne new funktionieren](#obj-new)
6. [Erstellen von Methoden mit Prototypen](#obj-methods)
7. [Private Daten mit Closures](#obj-private)
8. [Instanzstatus nur in Instanzobjekten speichern](#obj-status)
9. [Das this keyword](#obj-this)
10. [Aufruf von Superklassenkonstruktoren](#obj-super)
11. [Eigenschaftsnamen aus der Superklasse nicht wiederverwenden](#obj-prop)
12. [Vermeiden von Standardklassen zu erben](#obj-inherit)
13. [Prototypen sind richtige Implementierungen](#obj-right)
14. [Monkey Patching vermeiden](#obj-monkey)


---
template: default
layout: true
#### Objekte und Prototypen


---
name: obj-definition
### Definition
- Objekte sind die grundlegende Datenstruktur in JavaScript
- Der Vererbungsmechanismus von JavaScript basiert nicht auf Klassen, sondern auf dem Konzept von Prototypen (Prototyp --> Geteilte Methoden)
- Klassen in JavaScript sind im Grunde genommen eine Kombination aus einer Konstruktorfunktion (Konstruktor) und einem Prototypobjekt (Konstruktor.prototype), mit dessen Hilfe die Methoden von allen Instanzen der Klasse gemeinsam genutzt werden können.
- Eine Klasse in JavaScript ist somit ein Entwurfsmuster, das aus einer Konstruktorfunktion und einem zugehörigen Prototyp besteht
- Prototypen sind ein Implementierungsdetail des Objektverhalten


---
name: obj-proto
### Unterschied zwischen prototype, getPrototpeOf und \_\_proto\_\_
- C.prototype wird verwendet, um den Prototyp der mit new C( ) erstellten Objekte zu bestimmen
- Object.getPrototypeOf(obj) ist der Standardmechanismus von ES5, um das Prototypobjekt von obj abzurufen.
- obj.\_\_proto\_\_ ist ein nicht standardkonformer Mechanismus (erst ab ES6 Standard), um das Prototypobjekt von obj abzurufen

**Prototype**
- Wird mit new eine Instanz eines Konstruktors gebildet, wird dem resultierenden Objekt automatisch das in Konstruktor.prototype definierte Objekt als Prototyp zugewiesen
- Die Eigenschaft prototype einer Konstruktorfunktion richtet die Prototypbeziehung neuer Instanzen ein
- Wenn Eigenschaften nachgeschlagen werden, beginnt die Suche erst in den eigenen Eigenschaften des Objekts. 
- Eigenschaften die nicht im eigenen Objekt gefunden werden, werden im Prototyp nachgeschlagen
- Eigenschaften die nicht im Prototyp des eigenen Objekt gefunden werden, werden in den vererbten Protoypen nachgeschlagen bis zum Object hinauf

**getPrototypeOf() && \_\_proto\_\_**
- Die statische ES5 Funktion ruft den Prototyp eines bestehenden Objektes ab
- Per \_\_proto\_\_kann über die Instanz der Prototype aufgerufen werden


---
name: obj-compare
### Object.getPrototypeOf() und \_\_proto\_\_
- Verwenden der standard konformen Funktion Object.get PrototypeOf statt der nicht standardkonformen Eigenschaft \_\_proto\_\_
- In Nicht-ESS-Umgebungen, die _proto_ unterstützen, ist eine eigene Implementierung einer Version von Object.getPrototypeOf möglich


---
name: obj-__proto__
### Niemals \_\_proto\_\_ ändern
**Portabilität auf andere Plattformen:** <br />
Nicht alle Plattformen bieten die Möglichkeit \_\_proto\_\_ zu ändern --> Code nicht vollständig portierbar <br /> <br />

**Leistung:**<br />
Moderne JS-Engines optimieren den Abruf und Festlegung von Objekteigenschaften( eine der häufigsten Operation). Diese Optimierungen basieren auf den Kenntnissen der Engine über die Struktur eines Objekts. Wenn Eigenschaften zu ihm oder zu einem anderen Objekt in seiner Prototypkette hinzugefügt oder entfernt werden, dann werden einige dieser Optimierungen unwirksam. Eine Bearbeitung von __proto__ ändert die Vererbungsstruktur, und das ist die destruktivste Änderung, die überhaupt möglich ist. Damit können weit  mehr Optimierungen zunichte gemacht werden als durch Änderungen an normalen Eigenschaften. <br /> <br />

**Vorhersagbarkeit**<br />
Der wichtigste Aspekt, auf eine Änderung an __proto__ zu verzichten, besteht darin die Vorhersagbarkeit zu bewahren.  Das Verhalten eines Objekts wird durch seine Prototypkette bestimmt, da sie seine Menge an Eigenschaften und Eigenschaftswerten festlegt. Die Änderung der Prototypbeziehung eines Objekts ist fahrlässig, da die gesamte Vererbungshierarchie des Objekts ausgetauscht wird. <br /> <br />

**Object.create**<br />
Diese statische ES5 Funktion erlaubt es maßgeschneiderte Prototypen für neue Objekte zu definieren


---
name: obj-new
### Erstellen von Konstruktoren die auch ohne new funktionieren
- Im Strict Mode ist this in Konstruktoren undefined anstatt window
- Konstruktoren sollten unabhängig von der Syntax des Aufrufers sein, indem sie sich selbst mit new oder Object.create neu aufrufen, falls kein new beim Erstellen einer Instanz verwendet wurde
- Konstruktorüberschreibung (Constructor Override): Es ist in JavaScript zulässig, das Ergebnis einer mit new aufgerufen Konstruktorfunktion, per return zu überschreiben
- Dokumentieren der Stellen, an denen eine Funktion mit new aufgerufen werden muss


---
name: obj-methods
### Erstellen von Methoden mit Prototypen
- Beim Speichern von Methoden in Instanzobjekten werden mehrere Exemplare der Funktionen erstellt, und zwar eine pro Instanzobjekt 
- Mehr Speicher wird benötigt
- Es ist besser, Methoden in Prototypen (Prototypmethoden) zu speichern als in Instanzobjekten (Instanzmethoden)


---
name: obj-private
### Private Daten mit Closures
- Closurevariablen sind privat und nur für lokale Verweise zugänglich.
- Für private Daten sollten lokale Variablen verwendet werden, um die Informationen in den Methoden zu verbergen.

---
name: obj-status
### Instanzstatus nur in Instanzobjekten speichern
- Veränderbare Daten (Statusdaten der Instanz) können bei der gemeinsamen Nutzung Probleme hervorrufen.
- Prototypen werden von all ihren Instanzen gemeinsam verwendet
- Methoden sollten zustandslos sein und über this auf den Instanzstatus verweisen
- Speichern der Statusdaten in einer Instanz im zugehörigen lnstanzobjekt


---
name: obj-this
### Das this keyword
- Der Gültigkeitsbereich von this wird stets durch die nächstliegende einschließende Funktion bestimmt.
- Um die this-Bindung für innere Funktionen verfügbar zu machen, wird meist eine lokale Variable mit einen Name wie self, me oder that gewählt


---
name: obj-super
### Aufruf von Superklassenkonstruktoren
- Ein Superklassenkonstruktor sollten nur in einem anderen Subklassenkonstruktor aufgerufen werden [.call(this, …args)] und nicht beim Erstellen des Prototypen der Subklasse 
- Das Prototypobjekt der Subklasse sollte mit Object.create erzeugt werden, um einen Aufruf des Superklassenkonstruktors zu vermeiden 
- Alternative zu Object.create ist Subklass.prototype = Klass.prototype


---
name: obj-prop
### Eigenschaftsnamen aus der Superklasse nicht wiederverwenden
- Auf Eigenschaftsnamen achten, die auch von der Superklasse verwendet wird
- In Subklassen niemals Eigenschaftsnamen der Superklasse verwenden


---
name: obj-inherit
### Vermeiden von Standardklassen zu erben
- Die Eigenschaft length operiert nur auf Objekten, die intern als „echte“ Arrays gekennzeichnet sind (length hält sich über die Anzahl der indizierten Eigenschaften auf dem Laufenden). 
- Die Kennzeichnung ist im ECMAScript-Standard als eine unsichtbare interne Eigenschaft namens [[Class]] deklariert (bspw. Arrays haben den [[Class]]-Wert „Array“)
- Eine Vererbung von Standardklassen funktioniert aufgrund von besonderen internen Eigenschaften wie [[Class]] nicht (da nicht vererbbar)
- Eigene Klassen haben den Wert „Object“ als interne Eigenschaft 
- Object.prototype.toString.call() fragt die interne Eigenschaft [[Class]] des Empfängers ab
- Anstatt einer Vererbung von Standardklassen sollten eine Delegierung an Eigenschaften erfolgen

- ... __BILD oder Tabelle__ ...


---
name: obj-right
### Prototypen sind richtige Implementierungen
- Prototypen sind ein Implementierungsdetail des Objektverhalten
- „Introspection“  Object.prototype.hasOwnProperty  Inspiziert die Instanzeigenschaften
- Objekte sind Interfaces, Prototypen sind Implementierungen
- Keine Inspektion der Prototypstruktur  oder Eigenschaften von Objekten, die nicht der eigenen Kontrolle unterliegen


---
name: obj-monkey
### Monkey Patching vermeiden
**Definition:** Eigenschaften hinzufügen, entfernen oder verändern.
- Vermeiden von unbesonnenen Monkey-Patching
- Dokumentieren von sämtlichen Monkey-Patches, die an einer Bibliothek vorgenommen werden
- Optionale Monkey-Patches, indem Änderungen in einer exportierten Funktion aufgerufen werden müssen
- Verwenden von Monkey-Patching, um Lücken durch fehlende Standard-APis zu stopfen 
- Polyfills


<!-- ######################### Kapitel 5 ############################ -->

---
class: middle
layout: false
# Arrays und Dictionaries
1. [Definition](#dict-definition)
2. [Erstellen von schlanken Dictionaries mit Object](#dict-schlank)
3. [Null-Prototypen als Maßnahme gegen Prototypverunreinigung](#dict-null)
4. [hasOwnProperty gegen Prototyp Verunreinigung](#dict-hasOwn)
5. [Für geordnete Collection Arrays statt Dictionaries verwenden](#dict-collection)
6. [igenschaften zu Object.prototype hinzufügen!](#dict-count)
7. [Objekte nicht während einer Aufzählung verändern](#dict-change)
8. [Arrays mit for anstatt mit for in iterieren ](#dict-for)
9. [Iterationsmethoden sind besser als Schleifen](#dict-methoden)
10. [Generische Arraymethoden für arrayähnliche Objekte wiederverwenden](#dict-arraykind)
11. [Verwenden von Arrayliterale statt des Arraykonstruktors](#dict-arrayliteral)
12. [](#dict-)
13. [](#dict-)
14. [](#dict-)


---
template: default
layout: true
#### Arrays und Dictionaries


---
name: dict-definition
### Definition
Ein Objekt in JavaScript kann für viele unterschiedliche Dinge verwendet werden:
- Name-Wert-Zuweisung
- Objektorientierte Datenabstraktion mit geerbten Methoden
- Array mit geringer oder hoher Dichte
- Hash-Tabelle

**Definition Dictionary:** 
- Dabei handelt es sich um Sammlungen (Collection) variabler Größe, die Strings zu Werten zuweisen.


---
name: dict-schlank
### Erstellen von schlanken Dictionaries mit Object
- Zur Konstruktion von schlanken Dictionaries sind Objektliterale am besten
- Als Schutz gegen **Prototypverunreinigung (Prototype Pollution)** in for-in Schleifen sollten schlanke Dictionaries direkt von Object.prototype abstammen (Objektliteral als Erzeuger) 
 
```javascript
Array.prototype.first = function () {
    return this[O];
};

var dict = [],
    array = [];

dict.bud = 12;
dict.terry = 80;

for (var name in dict) {
    array.push(name);
}
console.log(array); // bud, terry, first
```


---
name: dict-null
### Null-Prototypen als Maßnahme gegen Prototypverunreinigung
- Vor ES5 gab es keine Standardmöglichkeit, um ein neues Objekt mit leerem Prototyp zu erstellen, welches weniger empfindlich gegen eine Prototyp Verunreinigung ist
- Bei der Instanziierung im folgenden Beispiel werden trotzdem Instanzen von Object gebildet, da new immer ein neues Objekt erstellt

```javascript
function C() {}
C.prototype = null;
var c = new C();
Object.getPrototypeOf(c) === null; // false
Object.getPrototypeOf(c) === Object.prototype; // true --> new erstellt leeres Objekt!
```

- Eine Möglichkeit, um ein Objekt mit benutzerdefinierten Prototyp zu erstellen bietet die in ES5 eingeführte statische Methode Object.create()
- Als erster Parameter wird der jeweilige Prototyp angegeben, der zweite Parameter ist eine Eigenschafts-Deskriptor-Zuordnung, welche die Eigenschaft und Methoden sowie deren Konfiguration (Attribute) des neuen Objektes beschreiben

```javascript
var y = Object.create(null, {wert: {value: 1}}); // {wert:1}
Object.getPrototypeOf(y) === null; // true
```  


---
### Null-Prototypen als Maßnahme gegen Prototypverunreinigung
- Mit \_\_proto\_\_ ist dieses Resultat auch möglich (bis ES5 nicht standardkonform)
- In einigen Umgebungen sorgt die Verwendung von "\_\_proto\_\_" (Stringdarstellung) als Schlüsselpaarname für Fehler

```javascript
var y = Object.create(null, {wert: {value: 1}}); // y = {wert:1}
Object.getPrototypeOf(y) === null; // true
```


---
name: dict-hasOwn
### hasOwnProperty gegen Prototyp Verunreinigung
- Verwenden von hasOwnProperty als Schutz gegen Prototyp-Verunreinigung
- Verwenden eines lexikalischen Gültigkeitsbereich sowie call als Schutzmaßnahme die Methode hasOwnProperty nicht zu überschreiben 
- Implementieren von Dictionary-Operationen in einer Klasse, die alle hasOwnProperty-Standardtests kapselt
- Verwenden der Dictionary-Klasse als Schutz gegen die Verwendung von "\_\_proto\_\_" als Schlüssel (if Prüfung ob der Schlüssel verwendet wurde)

```javascript
var dict = {},
    hasOwn = Object.prototype.hasOwnProperty;

dict.name = "Bud";
dict.age = 80;

dict.hasOwnProperty = "Überschrieben!"; 
hasOwn.call(dict, "name"); // Geht
dict.hasOwnProperty("name"); // Fehler
```


---
name: dict-collection
### Für geordnete Collection Arrays statt Dictionaries verwenden
- Die Reihenfolge in der for in Schleifen Objekteigenschaften aufzählen ist nicht spezifiziert
- In einige Umgebungen (Chrome 41, FF36, IE11) werden die Schlüsselpaare nach der Wertigkeit des Schlüsselnamens sortiert (bzw. aufgezählt), aber nur wenn Arrayindezies als Schlüsselname dienen 
- Dabei müssen nicht alle Schlüsselnamen Arrayindezies enthalten, Schlüsselnamen mit andere Namensgebung werden nachfolgend einsortiert

```javascript
var dict = {
    "31": "Eins",
	"blubb": "Zwei",
	"12": "Drei",
	"18": "Vier"
};
for (var index in dict) {
    console.log(dict[index]); // Drei, Vier, Eins, Zwei
}
```

- **Achtung!** wenn das Programm auf eine bestimmte Reihenfolge der Objektaufzählung angewiesen ist
- Daher eignet sich ein Array für die Speicherung einer Datenstruktur mit bestimmeter Reihenfolge besser als ein Dictionary


---
name: dict-count
### Eigenschaften zu Object.prototype hinzufügen
- Keine aufzählbaren Eigenschaften zu Object.prototype hinzuzufügen, stattdessen lieber eine Funktion schreiben
- Alternativ mit der ES5 Methode Object.defineProperty Eigenschaften und Metadaten (Konfiguration über die Attribute) zu Object.prototype hinzufügen und diese als nicht aufzählbar deklarieren **(enumerable auf false)**

```javascript
Object.defineProperty(Object.prototype, "neueEigenschaft", {
    writable: true,			// true --> Wert KANN verändert werden
    enumerable: false,		// true --> DARF in for-in-Schleifen auftauchen
    configurable: false,	// true --> DARF NICHT gelöscht werden
    value: "Neuer Wert"		// Wert von neueEigenschaft ist "Neuer Wert"
});

var dict = {
    "blubb": "Hi",
    "buddy": "YO"
};

for (var prop in dict) {
    console.log(dict[prop]); // Hi, YO
}
```


---
name: dict-change
### Objekte nicht während einer Aufzählung verändern
- Ein Objekt sollte nicht verändert werden, während seine Eigenschaften in einer for in Schleife aufgezählt werden
- Der JavaScript Standard sagt dazu folgendes: 
> *Wenn zu dem Objekt, über welches iteriert wird, während der Iteration neue Eigenschaften hinzugefügt werden, kann nicht garantiert werden, dass neu hinzugekommene Eigenschaften in der laufenden Aufzählung aufgesucht werden*
- Um eine vorhersagbare Aufzählung veränderlicher Datenstrukturen zu treffen, sollte anstatt eines Dictionary-Objekts eine sequenzielle Datenstruktur wie ein Array verwendet werden
- Für Arrayiterationen sind while oder klassische for Schleifen besser als eine for in Schleife


---
name: dict-for
### Arrays mit for anstatt mit for in Schleifen iterieren 
- Zur Iteration über die indizierten Eigenschaften eines Arrays immer eine for-Schleife und keine for in Schleife verwenden
- Eine for in Schleife zählt die Schlüsselnamen auf, vorsicht da die Schlüssel von Objekteigenschaften Strings sind

```javascript
var zahlen = [1, 2, 3];
var summe1 = 0;
var summe2 = 0;

// Achtung! Die Schlüssel von Objekteigenschaften sind Strings
for (var wert in zahlen) {
    summe1 += wert; // 012 --> falsch, hier werden Strings verkettet
    summe2 += zahlen[wert]; // 2 --> Korrekt!
}
console.log(summe/ zahlen.length); // Falscher Durchschnitt, da Strings
console.log(summe2 / zahlen.length); // Korrekter Durchschnitt
```
- Speichern der Eigenschaft length eines Arrays vor Beginn der Schleife in einer lokalen Variable, um eine Neuberechnung zu vermeiden


---
name: dict-methoden
### Iterationsmethoden sind besser als Schleifen
- Anstelle von for-Schleifen Iterationsmethoden wie Array.prototype.forEach und Array.prototype.map verwenden, um den Code besser lesbar zu machen und die Steuerlogik von Schleifen nicht duplizieren zu müssen
- Häufig vorkommende Schleifenvorgänge, die von der Standardbibliothek nicht abgedeckt werden, mit selbst geschriebenen lterationsfunktionen abstrahieren
- Für Schleifen, die vorzeitig abgebrochen werden müssen, können herkömmliche Schleifen nach wie vor geeignet sein. Alternativ eignen sich dazu die Methoden some und every

```javascript
var greaterThanNull = [1, 2, 3, 4, 5].every (function (x) {
    return x > 0 ;
});
console.log(greaterthanNull); // true
```


---
name: dict-arraykind
### Generische Arraymethoden für arrayähnliche Objekte wiederverwenden
- Die generischen Methoden von Arrays können für arrayähnliche Objekte wiederverwendet werden, indem die Methodenobjekte extrahiert werden und darüber hinaus die call-Methode verwenden

**Definition arrayähnlich**
- Das Objekt verfügt über die Integereigenschaft length mit einem Wertebereich zwischen 0 und 2^32 - 1 
- Der Wert von length ist größer als der größte Index des Objekts
- Ein Index ist ein Integer aus dem Bereich zwischen 0 und 2^32 - 2, dessen Stringdarstellung als Schlüssel für eine Eigenschaft des Objekts dient.
- Auch Strings verhalten sich wie unveränderbare Arrays, da sie indiziert werden können und ihre Länge über die Eigenschaft length zugänglich ist

```javascript
var arraylike = { 0: "a", 1: "b", 2: "c" , length : 3 }; // hat eine length Eigenschaft
var result = Array.prototype.map.call(arraylike, function (s) {
    return s.toUpperCase() ;
}) ; // ["A",  "B" , "C"]
```

**Weitere Arrayeigenschaften:**
- Wenn die Eigenschaft length auf einen kleineren Wert n gesetzt wird, werden automatisch alle Eigenschaften mit einem Index größer oder gleich n gelöscht
- Wird eine Eigenschaft mit dem Index n hinzugefügt, der größer oder gleich dem Wert der Eigenschaft length ist, so wird length automatisch auf n + 1 gesetzt

```javascript
    // Array length Verhalten
    var array = ["a", "b", "c"]; // length ist 3
    array[10] = "JO"; // length ist 11
    array.length = 2;
    console.log(array); // ["a", "b"]
```
- Es gibt eine einzige Array-Methode, die nicht allgemeingültig ist, nämlich die Verkettungsmethode concat. Sie kann für alle arrayähnlichen Empfänger aufgerufen werden, prüft aber den [[Class]]-Wert
ihrer Argumente. Argumente, die echte Arrays sind, werden verkettet, andere Argumente dagegen werden als einzelnes Element hinzugefügt.

```javascript
// Concat prüft den [[class]] - Wert
function namesColumn () {
    // return ["Names"].concat(arguments); --> geht nicht 
    // Rückgabe ["Names", {0: "Alice" ,1: "Bob" ,2: "Chris"}]
    return ["Names"].concat([].slice.call(arguments));
}
namesColumn("Alice", "Bob", "Chris"); // ["Names", "Alice" , "Bob" , "Chris" ]
```


---
name: dict-arrayliteral
### Verwenden von Arrayliterale statt des Arraykonstruktors
**Vorsicht beim Arraykonstruktor**
- Der Arraykonstruktor ist überladen:
  - Kein Parameter --> Leeres Array wird erstellt
  - Einzelnes nummerisches Argument --> Ein Array mit der jeweiligen Länge des Arguments wird erstellt (length erhält somit den Wert)
  - Die nummerische 0 als Argument --> Leeres Array wird erstellt
  - Mehrere Argumente --> Ein Array mit den angegebenen Argumenten als Inhalt wird erstellt

- Konstruktoren können überschrieben werden

```javascript
function keinArray(Array, content) {
    return new Array(content);
}
console.log(keinArray(String, "Das ist kein Array"));
```


<!-- ######################### Kapitel 6 ############################ -->

---
class: middle
layout: false
# API
1. [API](#api-definition)
2. asd
3. asd
4. 



---
template: default
layout: true
#### API


---
name: api-definition






---
name: api-
### API


