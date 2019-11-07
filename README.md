# READMEgit: Eine Anleitung zum Umgang mit Git im Allgemeinen und GitHub im Besonderen

## Inhaltsverzeichnis

* Einleitung
* Was ist GIT?
* Welchen Nutzen bringt GIT?
* Was ist ein BRANCH?
* Was ist ein Commit/Push/Pull?
* Was ist Mergen?
* Welche Arten von Merge gibt es?
* Repository Struktur mit GIT


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

## Was ist ein Commit/Push/Pull?

## Was ist Mergen?

## Welche Arten von Merge gibt es?

## Repository Struktur mit GIT

In einem `GIT` Repository muss um Ordnung zu gewährleisten eine gewisse Struktur gegeben sein. Wir Arbeiten hierbei mit dem sogenannten `FEATURE` System. Dabei gibt es einen ``master`` Branch, in welchen die fertigen Versionen gemergt werden und eine ``dev`` Branch. In diesen werden die einzelnen Features welche mit der Branch Notation ``FEATURE/featureName`` umgesetzt werden, durch Pull Requests zusammengeführt. 

### Beispiel

_Branches_
* master
* dev
* FEATURE/Navbar
* FEATURE/PagePicker