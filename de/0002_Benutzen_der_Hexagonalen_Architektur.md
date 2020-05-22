# Benutzen der Hexagonalen Archtektur

* Status: proposed
* Entscheider: [list everyone involved in the decision]
* Datum: [YYYY-MM-DD]

## Kontext und Problemstellung

## Berücksitigte Optionen

* Layered Architecture
* Hexagonal Architecture

### Schichtenarchitektur

Die Schichtenarchitektur, auch als n-Tier-Architektur bekannt, ist eines der häufigsten Architekturmuster. Es ist der De-facto-Standard für die meisten Backend-Anwendungen und daher den meisten Architekten, Designern und Entwicklern bekannt. Obwohl die Schichtenarchitektur nicht die Anzahl der Ebenen angibt, bestehen die meisten Anwendungen aus drei Ebenen: Präsentation, Geschäftslogik und Persistenz. Die Ebenen sind aufeinander gestapelt, was bedeutet, dass eine Anfrage von Ebene zu Ebene verschoben werden muss. Beispielsweise muss eine Anfrage, die von der Präsentationsschicht stammt, zuerst die Geschäftsschicht durchlaufen, bevor sie an die Persistenzschicht weitergeleitet wird. Dies wird durch die Regel erzwungen, dass eine Komponente nur auf Komponenten in derselben oder der darunter liegenden Ebene zugreifen darf.

### Hexagonale Architektur

Die Hexagonale Architektur ist eine Implementierung der von Robert C. Martin in seinem gleichnamigen Buch vertretenen Clean Architecture. In seiner Definition einer Clean Architecture sind die Geschäftsregeln vom Design her testbar und unabhängig von Frameworks, Datenbanken, UI-Technologien und anderen externen Anwendungen oder Schnittstellen. Dies bedeutet, dass der Domänencode keine nach außen gerichteten Abhängigkeiten aufweisen darf. Ferner zeigen alle Abhängigkeiten auf den Domänencode.

Die Schichten in dieser Architektur sind in konzentrischen Kreisen umeinander gewickelt. Die Hauptregel in einer solchen Architektur ist die Abhängigkeitsregel, die besagt, dass alle Abhängigkeiten zwischen diesen Schichten nach innen zeigen müssen.

The core of the architecture contains the domain entities which are accessed by the surrounding use cases. The use cases are what we call services the layered architecture. Use cases are restricted in their width allowing them only to have a single responsibility (i.e. a single reason to change).

Der Kern der Architektur enthält die Domänenentitäten, auf die die umgebenden Anwendungsfälle zugreifen. Die Anwendungsfälle sind in in der Schichtenarchitektur bekannt als Services. Anwendungsfälle sind in ihrer Breite beschränkt, sodass sie nur eine einzige Verantwortung haben (d.h. einen einzigen Grund für eine Änderung).

Um diesen Domänenkern herum finden wir alle anderen Komponenten unserer Anwendung, die unsere Anwendung mit externen Systemen verbinden. In der Regel bedeutet diese Unterstützung die Bereitstellung von Persistenz oder die Bereitstellung einer Benutzeroberfläche.

Da der Domänencode keine Kenntnisse über die in den unterstützenden Schichten verwendeten Frameworks hat, kann er keinen für diese Frameworks spezifischen Code enthalten und sich auf die Geschäftsregeln konzentrieren.

Die hexagonale Architektur wird als Sechseck dargestellt, woher dieser Architekturstil seinen Namen erhält. Die Sechseckform hat jedoch keine Bedeutung. Sie wird einfach anstelle des gewöhnlichen Rechtecks verwendet, um zu zeigen, dass eine Anwendung mehr als vier Seiten haben kann, die sie mit anderen Systemen oder Adaptern verbindet. Es könnte genauso gut als Achteck dargestellt werden.

Within the hexagon, we find our domain entities and the use cases that work with them. Note that the hexagon has no outgoing dependencies, so that the Dependency Rule from Martin’s Clean Architecture holds true. Instead, all dependencies point towards the center.

Innerhalb des Sechsecks finden wir unsere Domänenentitäten und die Anwendungsfälle. Zu beachten ist, dass das Sechseck keine ausgehenden Abhängigkeiten aufweist, sodass die Abhängigkeitsregel aus Martins Clean Architecture gilt. Stattdessen zeigen alle Abhängigkeiten zur Mitte.

Außerhalb des Sechsecks finden wir verschiedene Adapter, die mit der Anwendung interagieren. Möglicherweise gibt es einen Webadapter, der mit einem Webbrowser interagiert, einige Adapter, die mit externen Systemen interagieren, und einen Adapter, der mit einer Datenbank interagiert.

Die Adapter auf der linken Seite sind Adapter, die unsere Anwendung steuern (weil sie unseren Anwendungskern aufrufen), während die Adapter auf der rechten Seite von unserer Anwendung gesteuert werden (weil sie von unserem Anwendungskern aufgerufen werden).

To allow communication between the application core and the adapters, the application core provides specific ports. For driving adapters, such a port might be an interface that is implemented by one of the use case classes in the core and called by the adapter. For a driven adapter, it might be an interface that is implemented by the adapter and called by the core.

Um die Kommunikation zwischen dem Anwendungskern und den Adaptern zu ermöglichen, stellt der Anwendungskern fest definierete Ports bereit. Bei einem steuerenden Adapter kann ein solcher Port eine Schnittstelle sein, die von einer der Anwendungsfallklassen im Kern implementiert und vom Adapter aufgerufen wird. Bei einem gesteuerten Adapter kann es sich um eine Schnittstelle handeln, die vom Adapter implementiert und vom Anwendungkern aufgerufen wird.

Aufgrund seiner zentralen Konzepte wird dieser Architekturstil auch als "Ports and Adapter" -Architektur bezeichnet

## Entscheidungsergebnis

TBD

## Pro und Kontra der Optionen

### Layered Architecture

* Good, because it can quickly adapt to changing requirements and external factors.
* Good, because it allows to switch a layer without affecting the others.
* Good, because it allows to add new features alongside existing one's.
* Good, because each layer has a clear responsibility.

* Bad, because it's prone for bad habits to creep in.

* Gut, weil es sich schnell an sich ändernde Anforderungen und externe Faktoren anpassen kann.
* Gut, weil es erlaubt, eine Schicht zu wechseln, ohne die anderen zu beeinflussen.
* Gut, weil es ermöglicht, neue Funktionen neben vorhandenen hinzuzufügen.
* Gut, weil jede Schicht eine klare Verantwortung hat.

* Schlecht, weil es datenbankgesteuertes Design fördert

**Warum?** Der Abhängigkeitsfluss in einer Schichtarchitektur zeigt nach unten, normalerweise in Richtung der Persistenzschicht.

**Aber warum ist das schlimm?** Wir modellieren das Geschäftsverhalten nicht Zustand in unserer Anwendung, die das Geschäft antreibt. Daher sollten wir die Domänenlogik zuerst erstellen, denn nur dann können wir herausfinden, ob wir sie richtig verstanden haben. Also warum sollte die Datenbank das Fundament unserer Architektur sein und nicht die Domäne?

Die treibende Kraft für diese schlechte Angewohnheit sind ORM-Frameworks, die uns dazu verleiten, Geschäftsregeln mit Persistenz zu mischen, weil sie ORM-Entitäten verwalten. Dadurch verschmelzen die beiden Schichten virtuell, was es schwierig macht, eine ohne die andere zu wechseln.

* Schlecht, weil es anfällig für Abkürzungen ist.

**Komponenten verschieben**. In der Schichtenarchitektur gibt es die Regel, dass Komponenten nur auf andere Komponenten in derselben oder der darunter liegenden Schicht zugreifen dürfen. Wenn wir also Zugriff auf eine Komponente oben benötigen, verschieben wir diese einfach eine Schicht nach unten und das Problem ist gelöst! Dies einmal zu tun mag in Ordnung sein, besonders bei drohenden Fristen, aber dies öffnet die Tür, um es immer wieder zu tun.

**Überspringen von Schichten**. Normalerweise beginnen Projekte, Abkürzungen von der Präsentationsschicht zur Persistenzschicht für einfache CRUD-Anwendungsfälle zu verwenden, wodurch schließlich immer mehr Domänenlogik in die Präsentationsschicht integriert wird. Diese Angewohnheit erschwert das Testen, da bei den Komponententests mehr Mockaufwand erforderlich ist.

* Schlecht, weil es Anwendungsfälle verbirgt

Für gewöhnlich werden wir vorhandene Anwendungsfälle häufiger ändern, anstatt neue zu erstellen. Aber wo sollen wir Funktionen hinzufügen oder ändern? Aufgrund der bereits genannten schlechten Gewohnheiten kann dies schwierig sein. Des Weiteren legt die Schichtenarchitektur keine Regeln für die Breite eines Dienstes fest, was häufig dazu führt, dass Dienste mehrere Anwendungsfälle behandeln. Dies macht es schwierig, den für einen Anwendungsfall verantwortlichen Dienst zu finden. Es macht es noch schwieriger, Tests zu schreiben, da wir uns über alles mocken müssen, was nicht mit dem zu testenden Anwendungsfall zu tun hat.

* Schlecht, weil es das parallele Arbeiten erschwert.

Die Arbeit an verschiedenen Funktionen in einem breiten Dienst führt unvermeidlich zu Konflikten. Verschiedene Entwickler, die auf verschiedenen Schichten arbeiten, funktionieren nicht so gut, da die oberste Schicht von der darunter liegenden Schicht abhängt und so weiter. Dies könnte durch Schnittstellen gelöst werden, aber nur, wenn wir ein datenbankgesteuertes Design verhindern. Die Domänenschicht muss zuerst implementiert werden.

### Hexagonale Architektur

* Good, because it promotes Domain-Driven Development.

* Good, because ports and adapters are interchangeable, just swap them.

Same is theoretically true for the layered architecture but reality usually disappoints.

* Good, because decisions can be deferred until the very end.

We can focus on the domain logic without worrying about UI or persistence concerns.

* Good, because features can be implemented faster.

Use cases are first class citizen of the hexagonal architecture hence there's no question where to add or modify a feature.

When you know how to convert requests and responses from the outside world it's really easy to implement features. This architecture facilitates us with the separation of concerns which allows us to focus on one specific task which allows us to develop faster.

* Bad, because [YAGNI](https://en.wikipedia.org/wiki/You_aren't_gonna_need_it).

The main concern is that you'll write a lot of boilerplate mapping code to facility the separation of the layers. This can be mitigated by applying guidelines for different types of use cases (i.e. modifying, querying or CRUD). Depending on the type a less or more strict mapping strategy may be chosen.

Most use case start as simple CRUD features and then evolve into something more complex. When deciding to use the hexagonal architecture additional ADRs are required to specify the mapping behaviour of different use case types.

* Gut, weil es die domänengetriebene Entwicklung fördert.

* Gut, da Ports und Adapter austauschbar sind.

Gleiches gilt theoretisch für die Schichtarchitektur, aber die Realität enttäuscht normalerweise.

* Gut, weil Entscheidungen bis zum Ende aufgeschoben werden können.

Wir können uns auf die Domänenlogik konzentrieren, ohne uns um Bedenken hinsichtlich der Benutzeroberfläche oder der Persistenz kümmern zu müssen.

* Gut, da Funktionen schneller implementiert werden können.

Anwendungsfälle sind fundamentaler Bestandteil der hexagonalen Architektur, daher steht außer Frage, wo ein Feature hinzugefügt oder geändert werden muss.

Wenn man weiß, wie Anfragen und Antworten von außerhalb konvertiert werden, ist es wirklich einfach, Funktionen zu implementieren. Diese Architektur erleichtert uns die Trennung von Anliegen, sodass wir uns auf eine bestimmte Aufgabe konzentrieren können, die es uns ermöglicht, uns schneller zu entwickeln.

* Schlecht, weil [YAGNI] (https://en.wikipedia.org/wiki/You_aren't_gonna_need_it).

Das Hauptanliegen ist, dass viel Boilerplate-Mapping-Code geschreiben werde muss, um die Trennung der Schichten zu sicherzustellen. Dies kann durch Anwenden von Richtlinien für verschiedene Arten von Anwendungsfällen (etwas ändern, anfragn oder CRUD) gemildert werden. Je nach Typ kann eine weniger oder strengere Zuordnungsstrategie gewählt werden.

Die meisten Anwendungsfälle beginnen als einfache CRUD-Funktionen und entwickeln sich dann zu etwas Komplexerem. Bei der Entscheidung für die Verwendung der Hexagonalen Architektur sind zusätzliche ADRs erforderlich, um das Zuordnungsverhalten verschiedener Anwendungsfalltypen anzugeben.

## Links

* [Implement rich/anemic domain models]()
* [Use "no-mapping" strategy for queries]()
* [Use "full-mapping" strategy for modifying use case between web and application layer]()
* [Use "no-mapping" strategy for modifying use case between application and persistence layer]()
