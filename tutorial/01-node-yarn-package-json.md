# 01 - Node, Yarn, and `package.json`

Der Quellcode für dieses Kapitel ist [hier](https://github.com/verekia/js-stack-walkthrough/tree/master/01-node-yarn-package-json) verfügbar.

In diesem Abschnitt werden wir Node, Yarn einrichten und eine `package.json` Datei anlegen, sowie ein erstes Paket testen.


## Node

> 💡 **[Node.js](https://nodejs.org/)** ist eine Javascript Laufzeit Umgebung. Es wird meistens für Backend Entwicklung genutzt, aber auch für generelle Skripte. Im Zusammenhang mit der Frontend Entwicklung kann es genutzt werden, um viele Aufgaben wie z.B. Linting, Testing und Assembling (von Dateien) erledigen.

Wir werden Node für quasi alles in diesem Tutorial benutzen, also wirst du es definitiv brauchen. Gehe auf die 
 [Download Seite](https://nodejs.org/en/download/current/), um **macOS** or **Windows** Versionen herunterzuladen, oder auf die [package manager installations page](https://nodejs.org/en/download/package-manager/) für Linux Distributionen.

Um beispielsweise Node unter  **Ubuntu / Debian** zu installieren, würdest du folgende Kommandos ausführen:

```sh
curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Bei der Versionswahl wähle irgendeine Verison von Node > 6.5.0.

## Node Versionsmanagement Tools

Falls du die Flexibilität benötigst um mehrere Versionen von Node gleichzeitig zu benutzen, siehe folgende Links: 

- [NVM](https://github.com/creationix/nvm)  
- [tj/n](https://github.com/tj/n).

## NPM

NPM ist der Standard Paket Manager von Node. Es wird automatisch während der Installation von Node installiert. Paketmanager werden genutzt um Pakete (Module, die du oder jemand anderes entwickelt hat) zu installieren und zu verwalten. 
Wir werden viele Pakete in diesem Tutorial nutzen, aber wir verwenden einen anderen Paketmanager: Yarn.


## Yarn

> 💡 **[Yarn](https://yarnpkg.com/)** ist ein Node.js Paketmanager, der sehr viel schneller ist als NPM, offline-fähig ist und Abhängigkeiten (dependencies)  [vorhersehbarer](https://yarnpkg.com/en/docs/yarn-lock) lädt.

Seitdem es im Oktober 2016 [released](https://code.facebook.com/posts/1840075619545360) wurde, hat es eine schnelle Adoption erfahren und dürfte bald der Paketmanager der Wahl der JavaScript Gemeinschaft sein.
Falls du die Verwendung von NPM bevorzugst, kannst du einfach alle `yarn add` und `yarn add --dev` Kommandos in diesem Tutorial durch `npm install --save` and `npm install --save-dev` ersetzen.

Bitte befolge die [Anleitung](https://yarnpkg.com/en/docs/install), um Yarn auf deinem Betriebssystem zu installieren. Unter macOS oder Unix empfehle Ich das **Installationsskript**, welches im *Alternatives* Tab gefunden werden kann. 
So kann es  [vermieden](https://github.com/yarnpkg/yarn/issues/1505) werden, für die Yarn Installation andere Paketmanager nutzen zu müssen:


```sh
curl -o- -L https://yarnpkg.com/install.sh | bash
```

## `package.json`

> 💡 **[package.json](https://yarnpkg.com/en/docs/package-json)** ist die Datei, die genutzt wird um dein JavaScript Projekt zu beschreiben und zu konfigurieren. Es beinhaltet generelle Informationen (dein Projektname, Version, Mitwirkende, Lizenzinformationen, etc), Konfigurationseinstellungen für die genutzten Tools und einen Abschnitt, um *Aufgaben* zu starten.

- Erstelle einen neuen Ordner, um darin zu arbeiten und `cd`in dieses.
- Starte `yarn init` und beantworte die darauf folgenden Fragen (`yarn init -y`, um alle Fragen zu überspringen), dabei wird automatisch eine grundlegende `package.json` Datei erstellt.


Hier seht ihr die grundlegende `package.json`, die ich in diesem Tutorial verwende:

```json
{
  "name": "your-project",
  "version": "1.0.0",
  "license": "MIT"
}
```

## Hello World
- Erstelle eine `index.js` Datei, mit dem Inhalt `console.log('Hello world')`

🏁 Starte `node .` in dem aktuellen Ordner (`index.js` ist die Standarddatei, die von Node als Einstiegsspunkt genutzt wird). In der Konsole sollte "Hello world" ausgegeben werden.


**Notiz**: Hast du das Rennflaggen 🏁 Symbol bemerkt ? Ich werde es jedes Mal benutzen, wenn wir einen **Checkpoint** erreicht haben. Wir werden manchmal sehr viele Änderungen direkt nacheinander vornehmen und dein Code könnte bis zum Erreichen des nächsten Checkpoints nicht funktionieren.

## `start` script

Dein Programm mit dem Befehl `node .` zu starten ist auf einem etwas zu niedrigen Level. Deshalb werden wir ein NPM/Yarn Skript nutzen, um die Ausführung des Codes zu starten. Dadurch erreichen wir eine einfachere Methode und es ist uns möglich unser Programm auch wenn es komplizierter wird mit dem Befehl `yarn start ` zu starten.

- In der Datei `package.json`, füge ein `scripts` Object hinzu:

```json
{
  "name": "your-project",
  "version": "1.0.0",
  "license": "MIT",
  "scripts": {
    "start": "node ."
  }
}
```

`start` ist der Name, den wir unserer *Aufgabe* geben, um das Programm zu starten. Wir werden viele verschiedene Aufgaben in diesem `scripts` Objekt während dem Tutorial erstellen. `start` ist normalerweise der Name für die Standardaufgabe eines Programms. Andere Standardbezeichnungen für Aufgaben sind `stop` und `test`.

`package.json` muss eine gültie JSON Datei sein, d.h. sie darfst keine anhängenden Kommas haben. Sei also vorsichtig, wenn du manuell die Datei `package.json` änderst.

🏁 Führe `yarn start` aus. Es sollte `Hello world` ausgegeben werden.

## Git and `.gitignore`

- Initialisiere ein Git Repository mit dem Befehl: `git init`

- Erstelle eine `.gitignore` Datei und füge folgenden Inhalt ein:

```gitignore
.DS_Store
/*.log
```

`.DS_Store` Dateien sind automatisch erstellte Dateien (unter macOS), die niemals in deinem Repository auftauchen sollten.

`npm-debug.log` und `yarn-error.log` sind Dateien, die erstellt werden sobald dein Paketmanager einen Fehler auslöst, also wollen wir diese auch nicht versioniert im Repository haben.

## Installieren und benutzen eines Pakets

In diesem Abschnitt werden wir ein Paket installieren und benutzen. Ein "Paket" ist einfach ein Stück Code, den jemand anderes geschrieben hat. Dieses Paket können wir in unserem eigenen Code verwenden. Es kann also jede Art von Code sein. Hier werden wir ein Paket ausprobieren, welches dabei hilft Farben zu manipulieren.

- Installiere das Paket mit dem Namen `color` durch den Befehl `yarn add color`

Öffne `package.json`, um zu sehen wie Yarn automatisch den Namen `color` unter den `dependencies` hinzufügt.
Außerdem wurde ein Ordner angelegt, in welchem alle Pakete gespeichert werden: `node_modules`.

- Füge `node_modules/` zu deiner `.gitignore` hinzu.

Du wirst zusätzlich bemerken, dass eine `yarn.lock` von Yarn erstellt wurde. Diese Datei solltest du jedoch zu deinem Repository commiten, da die Datei sicherstellt, dass jeder in deinem Team die gleichen Versionen der Pakete verwendet. Falls du bei der Verwendung von NPM geblieben bist, so heißt die äquivalente Datei *shrinkwrap*.

- Schreibe den folgenden Code in deine Datei `index.js`:

```js
const color = require('color')

const redHexa = color({ r: 255, g: 0, b: 0 }).hex()

console.log(redHexa)
```

🏁 Starte die Anwendung mit `yarn start`. Die Ausgabe sollte `#FF0000` sein.

Herzlichen Glückwunsch, du hast ein Paket installiert und genutztz !

Das Paket `color` ist nur in diesem Abschnitt genutzt, um dir beizubringen wie man ein einfaches Paket benutzt. Wir werden es nicht weiter benötigen und können es deshalb deinstallieren:

- Starte `yarn remove color`

## Zwei Typen von Abhängigkeiten

Es gibt zwei Typen von Paket Abhängigkeiten, `"dependencies"` und `"devDependencies"`:

**Dependencies** sind Bibliotheken die du benötigst, damit deine Anwendung funktioniert (z.B. React, Redux, Lodash, jQuery, etc.). Du kannst diese mit Hilfe von :  `yarn add [package]` installieren.


**Dev Dependencies** sind Bibliotheken die während der Entwicklung oder zum Erstellen deiner Anwendung (Webpack,Sass,Linters, Test Frameworks, etc.) genutzt werden. Du kannst diese mit Hilfe von : `yarn add --dev [package]` installieren.

Der Nächste Abschnitt: [02 - Babel, ES6, ESLint, Flow, Jest, Husky](02-babel-es6-eslint-flow-jest-husky.md#readme)

Zurück zum [Inhaltsverzeichnis](https://github.com/verekia/js-stack-from-scratch#table-of-contents).
