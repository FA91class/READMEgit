# READMEgit: Eine Anleitung zum Umgang mit Git im Allgemeinen und GitHub im Besonderen

## Inhaltsverzeichnis

* Einleitung
* Was ist GIT?
* Welchen Nutzen bringt GIT?
* Was ist ein BRANCH?
* Was ist Mergen?
* Repository Struktur mit GIT
* Quellen


## Einleitung

Bei der Erstellung dieser Gruppe viel auf, dass ein Guide zur Arbeit mit Git sinnvoll wäre, besonders die Quereinsteiger können davon profitiren.

## Was ist GIT?

Git ist eine freie Software zur verteilten Versionsverwaltung von Dateien, die durch Linus Torvalds initiiert wurde.

## Welchen Nutzen bringt GIT?

An einem Software-Projekt arbeiten heutzutage oftmals mehrere,
teilweise sogar Hunderte oder Tausende Entwickler mit. 
Von denen widmet sich jeder einem anderen Teil des Programms,
und deren Arbeitsergebnisse müssen irgendwo zusammengeführt werden.
Natürlich könnte jeder Entwickler seine Änderungen und Neuerungen an eine zentrale Person schicken,
und diese kümmert sich dann ausschließlich darum, den erhaltenen Code immer zu aktualisieren.
Genau diese Tätigkeit lässt sich aber mithilfe einer Versionsverwaltung wie Git automatisieren. 

## Was ist ein BRANCH?

Um wirklich zu verstehen, wie Git Branching durchführt,
müssen wir einen Schritt zurück gehen und untersuchen,
wie Git die Daten speichert. 
Wie Du Dich vielleicht noch an Kapitel 1 erinnerst,
speichert Git seine Daten nicht als Serie von Änderungen oder Unterschieden,
sondern als Serie von Schnappschüssen.

Wenn Du in Git committest, speichert Git ein sogenanntes Commit-Objekt. Dieses enthält einen Zeiger zu dem Schnappschuss mit den Objekten der Staging-Area, dem Autor, den Commit-Metadaten und einem Zeiger zu den direkten Eltern des Commits. Ein initialer Commit hat keine Eltern-Commits, ein normaler Commit stammt von einem Eltern-Commit ab und ein Merge-Commit, welcher aus einer Zusammenführung von zwei oder mehr Branches resultiert, besitzt ebenso viele Eltern-Commits.

Wenn Du einen Commit mit der Anweisung git commit erstellst, erzeugt Git für jedes Projektverzeichnis eine Prüfsumme und speichert diese als sogenanntes tree-Objekt im Git Repository. Git erzeugt dann ein Commit Objekt, das die Metadaten und den Zeiger zum tree-Objekt des Wurzelverzeichnis enthält, um bei Bedarf den Snapshot erneut erzeugen zu können.

Dein Git-Repository enthält nun fünf Objekte: einen Blob für den Inhalt von jeder der drei Dateien, einen Baum, der den Inhalt des Verzeichnisses auflistet und spezifiziert, welcher Dateiname zu welchem Blob gehört, sowie einen Zeiger, der auf die Wurzel des Projektbaumes und die Metadaten des Commits verweist.

Wenn Du erneut etwas änderst und wieder einen Commit machst, wird dieser einen Zeiger enthalten, der auf den Vorhergehenden verweist.

Ein Branch in Git ist nichts anderes als ein simpler Zeiger auf einen dieser Commits. Der Standardname eines Git-Branches lautet master. Mit dem initialen Commit erhältst Du einen master-Branch, der auf Deinen letzten Commit zeigt. Mit jedem Commit bewegt er sich automatisch vorwärts.

Was passiert, wenn Du einen neuen Branch erstellst? Nun, zunächst wird ein neuer Zeiger erstellt. Sagen wir, Du erstellst einen neuen Branch mit dem Namen testing. Das machst Du mit der Anweisung git branch:

````shell script
$ git branch testing
````

Dies erzeugt einen neuen Zeiger, der auf den gleichen Commit zeigt, auf dem Du gerade arbeitest.

Woher weiß Git, welchen Branch Du momentan verwendest? Dafür gibt es einen speziellen Zeiger mit dem Namen HEAD. Berücksichtige, dass dieses Konzept sich grundsätzlich von anderen HEAD-Konzepten anderer VCS, wie Subversion oder CVS, unterscheidet. Bei Git handelt es sich bei HEAD um einen Zeiger, der auf Deinen aktuellen lokalen Branch zeigt. In dem Fall bist Du aber immer noch auf dem master-Branch. Die Anweisung git branch hat nur einen neuen Branch erstellt, aber nicht zu diesem gewechselt.

Um zu einem anderen Branch zu wechseln, benutze die Anweisung git checkout. Lass uns nun zu unserem neuen Branch testing wechseln:

````shell script
$ git checkout testing
````

Das lässt HEAD neuerdings auf den Branch „testing“ verweisen.

Wenn Du den Branch wechselst, zeigt HEAD auf einen neuen Zweig.
Und was bedeutet das? Ok, lass uns noch einen weiteren Commit machen:

````shell script
$ vim test.rb
$ git commit -a -m 'made a change'
````

Der HEAD-Zeiger schreitet mit jedem weiteren Commit voran.
Das ist interessant, denn Dein Branch testing hat sich jetzt voranbewegt, während Dein master-Branch immer noch auf den Commit zeigt, welchen Du bearbeitet hast, bevor Du mit git checkout den aktuellen Zweig gewechselt hast. Lass uns zurück zu dem master-Branch wechseln:

````shell script
$ git checkout master
````

HEAD zeigt nach einem Checkout auf einen anderen Branch.
Diese Anweisung hat zwei Dinge veranlasst. Zum einen bewegt es den HEAD-Zeiger zurück zum master-Branch, zum anderen setzt es alle Dateien im Arbeitsverzeichnis auf den Bearbeitungsstand des letzten Commits in diesem Zweig zurück. Das bedeutet aber auch, dass nun alle Änderungen am Projekt vollkommen unabhängig von älteren Projektversionen erfolgen. Kurz gesagt, werden alle Änderungen aus dem testing-Zweig vorübergehend rückgängig gemacht und Du hast die Möglichkeit, einen vollkommen neuen Weg in der Entwicklung einzuschlagen.

Lass uns ein paar Änderungen machen und mit einem Commit festhalten:

````shell script
$ vim test.rb
$ git commit -a -m 'made other changes'
````

Nun verzweigen sich die Projektverläufe. Du hast einen Branch erstellt und zu ihm gewechselt, hast ein bisschen gearbeitet, bist zu Deinem Haupt-Zweig zurückgekehrt und hast da was ganz anderes gemacht. Beide Arbeiten existieren vollständig unabhängig voneinander in zwei unterschiedlichen Branches. Du kannst beliebig zwischen den beiden Zweigen wechseln und sie zusammenführen, wenn Du meinst, es wäre soweit. Und das alles hast Du mit simplen branch und checkout-Anweisungen vollbracht.

Die Historie läuft auseinander.
Branches können in Git spielend erstellt und entfernt werden, da sie nur kleine Dateien sind, die eine 40 Zeichen lange SHA-1 Prüfsumme der Commits enthalten, auf die sie verweisen. Einen neuen Zweig zu erstellen, erzeugt ebenso viel Aufwand wie das Schreiben einer 41 Byte großen Datei (40 Zeichen und einen Zeilenumbruch).

Das steht im krassen Gegensatz zu dem Weg, den die meisten andere VCS Tools beim Thema Branching einschlagen. Diese kopieren oftmals jeden neuen Entwicklungszweig in ein weiteres Verzeichnis, was – je nach Projektgröße – mehrere Minuten in Anspruch nehmen kann, wohingegen Git diese Aufgabe sofort erledigt. Da wir außerdem immer den Ursprungs-Commit festhalten, lässt sich problemlos eine gemeinsame Basis für eine Zusammenführung finden und umsetzen. Diese Eigenschaft soll Entwickler ermutigen Entwicklungszweige häufig zu erstellen und zu nutzen.

## Was ist Mergen?

## Repository Struktur mit GIT

In einem `GIT` Repository muss um Ordnung zu gewährleisten eine gewisse Struktur gegeben sein. Wir Arbeiten hierbei mit dem sogenannten `FEATURE` System. Dabei gibt es einen ``master`` Branch, in welchen die fertigen Versionen gemergt werden und eine ``dev`` Branch. In diesen werden die einzelnen Features welche mit der Branch Notation ``FEATURE/featureName`` umgesetzt werden, durch Pull Requests zusammengeführt. 

### Beispiel

_Branches_
* master
* dev
* FEATURE/Navbar
* FEATURE/PagePicker

## Quellen

https://de.wikipedia.org/wiki/Git

https://git-scm.com/book/de/v1/