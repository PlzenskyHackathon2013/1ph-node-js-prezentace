## Node.js ##

- open-source platforma
- MIT licence
- vývoj od roku 2009
- server-side javascript
- aktuální stabilní verze 0.10.2

Komunita javascriptových vývojářů.

- GitHub (pro srovnání):

**bootstrap**<br/>
* 47,925&nbsp;&nbsp;< 14,205<br/>
**node**<br/>
* 21,350&nbsp;&nbsp;< 3,572<br/>
**rails**<br/>
* 18,030&nbsp;&nbsp;< 5,381<br/>

- balíčkovací systém
  - => ekosystém podobný Ruby
- [CommonJS](http://www.commonjs.org/specs/modules/1.0/)
  - synchronní **require**
  - **module.exports**
  - cachování ("singleton")

- nepřeberné množství materiálů ke studiu
- tvořit se dá téměr vše:
  - jednoduché webové stránky
  - skripty
  - command-line utility
  - multiplayer online hry
  - bussiness aplikace

## Pod kapotou ##

Postaveno nad [V8](https://code.google.com/p/v8/)

- tenký C wrapper
- libuv
  - platform layer
  - práce s IO a systémem
- javascript core modules

## Základní principy ##

Event-loop based!

- single-thread
- event-driven
- non-blocking
- asynchronous

Single-thread:

- podobné **nginx**u
- řeší problém: 1 klient / 1 thread

```javascript
console.log("Start");
setTimeout(function() {
  console.log("I'm back here in 100ms! Or not?");
  // blocked by loop!!!
}, 100);

// 'sleep' 2 seconds in loop
var now = new Date().getTime();
while(new Date().getTime() < now + 2000) {
  // do nothing
}
console.log("End");
```

Pozor: výpočetně/CPU náročné aplikace.
**Blokují!**

- Řeší se:
  - [cluster](http://nodejs.org/api/cluster.html) (podobné HTML5 Workers)
  - async. algoritmus
  - [externí procesy](http://nodejs.org/api/child_process.html)

## Ekosystém ##

**NPM - package manager**

Vyhledávání modulů:

- [npmjs.org](http://npmjs.org)
- [nodetoolbox.com](http://nodetoolbox.com)
- [nodezoo.com](http://nodezoo.com)

> **module:**<br/>
> a thing you can require() in javascript.<br/>
> **package:**<br/>
> a thing i can install on the command line.<br/><br/>
> yes, they overlap.

<br/>
[twitter.com/npmjs](https://twitter.com/npmjs/status/315147766055718913)

Každý package obsahuje "manifest".
<br/><br/>
**package.json**
<br/><br/>
[Interaktivní package.json](http://package.json.nodejitsu.com/)

**package.json**

  - dependencies/devDependencies
  - name
  - version
  - author/contributors
  - scripts (např. **npm start**)

- package může být instalován:
  - lokálně
  - globálně

- nástroje většinou globálně <br/>`npm install grunt-cli -g`
- ostatní lokálně <br/>`npm install async`

Instalace dependencies z **package.json**<br/>
`npm install`
<br/><br/>
Přidání do **package.json** (musí být validní!)<br/>
`npm install underscore --save`

(zjistí verzi, nainstaluje a uloží)

Aktuální verze modulu v repozitáři<br/>
`npm info component version`
<br/><br/>
Hledání modulů<br/>
`npm search component`

(vhodnější hledat z webu)

Update modulů na novější verze<br/>
`npm update`
<br/><br/>
Využívá tzv. [sémantické verzování](http://semver.org)

Packages se ukládají do složky **node_modules**.

- většina packagů je javascriptový kód
- => lze se snadno podívat, co modul dělá
- => případně hot-fixnout

Instalovaný modul se načte funkcí **require**:

```javascript
var _ = require('underscore');
var evens = _.filter([1, 2, 3], function(num) {
  return num % 2 == 0;
});
```

<br/>
Uvnitř modulu se používají **relativní cesty**.

```javascript
var cow = require('./violet-cow/moodul.js');
cow.moo();
```

**TIP!**
<br/>
Adresář **node_modules/**

- přidat do **.gitignore**
- nekomitovat!

**TIP!**
<br/>
Proměnná **PATH**

- přidat **./node_modules/.bin**

<br/>
### Zpřístupní lokální command-line utility!

Seznam instalovaných modulů:

- lokálně<br/>`npm list`
<br/><br/>
- globálně<br/>`npm list -g`

Nepřeberné množství packagů.
<br/><br/>
**Pěkně rozdělené + základní přehled:**
<br/>
[Node.js Wiki - Modules](https://github.com/joyent/node/wiki/modules)

**Doporučuju:**

- [async](https://github.com/caolan/async)
- [underscore](http://underscorejs.org/)
- [cheerio](https://github.com/MatthewMueller/cheerio)
- [request](https://github.com/mikeal/request)

**Command-line nástroje:**

- [yeoman](http://yeoman.io/)
- [grunt](http://gruntjs.com/)

## I/O ##

Moduly pro práci s file-systémem:

- **fs**
  - asynchronní/synchronní verze
- **path**

Čtení ze souboru:

```javascript
var options = { encoding: 'utf-8' };
fs.readFile('fs-read.js', options, function(err, data) {
  if (err) throw err;
  console.log(data);
});
```
(pozor na **buffer**)

Čtení ze souboru (synchronní):

```javascript
var options = { encoding: 'utf-8' };
var data = fs.readFileSync('fs-read.js', options);
console.log(data);
```

Modul **process**:

- stdin/stdout/stderr
- env
- cwd
- kill
- nextTick (vs Timers.setImmediate)

Modul **child_process**:

- exec/fork/spawn
  - stdin/stdout/stderr
  - on (message/close/exit events)

Největší změna v poslední době...<br/>
Modul **stream**:

- usnaďuje tvorbu vlastních streamů
- naštěstí zpětně kompatibilní
- zajímavost:
  - nové API - modul do starších verzí

Modul **stream**:<br/>
Pomocné base classes:

- Readable
- Writable
- Duplex
- Transform
- PassThrough

**readable.pipe(writable)**

## Události ##

Modul **events**:<br/>
class **EventEmitter**:

  - emit
  - addListener/on
  - once
  - removeListener/removeAllListeners

**Pozor na maxListeners (default 10)!**

## Síť ##

Nativní moduly pro (server+client):

- Sockets (TCP, UDP)
  - podporuje unix sockets
- TLS/SSL
- HTTP/HTTPS

Moduly, které usnadňují práci, např.:

- [express.js](https://github.com/visionmedia/express)/[connect](https://github.com/senchalabs/connect)/[hapi](https://github.com/spumko/hapi/)
- [socket.io](https://github.com/LearnBoost/socket.io)/[ws](https://github.com/einaros/ws)

HTTP server:

```javascript
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
```

## OT: Express.js ##

[Express.js](http://expressjs.com/)

Pokud se nainstaluje globálně<br/>
`npm install -g express`
<br/><br/>
umí vytvořit kostru aplikace<br/>
`express &lt;app-name&gt;`
<br/><br/>
(ukázat strukturu aplikace)

HTTP server s Express.js:

```javascript
var express = require('express');
var app = express();
app.get('/', function(req, res){
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
});
app.listen(3000);
```

## OT: CoffeeScript ##

[CoffeeScript](http://coffeescript.org/) - vhodný doplněk
### 11. místo na GitHubu ###

- plus
  - zjednodušená syntaxe javascriptu
- mínus
  - přidává další vrstvu abstrakce
  - je potřeba kompilovat
  - exceptions - čísla řádek nesedí

- rychlé skriptování, prototypování
- vzal si **Good Parts** javascriptu
- odebral vše, co bylo "zbytečné"
- vše je výraz

```coffeescript
vypis = (text) -> console.log text
pozdravy = ['Ahoj', 'Nazdar', 'Zdar', 'Čus']
vypis "#{pozdrav} Coffee!" for pozdrav in pozdravy

objekt =
  jmeno: 'Dr. Zoidberg'
  obsazeni:
    Futurama: [1..136]

console.log "#{objekt.jmeno}" if objekt?
console.log objekt?.neexistujiciFunkce()?.toString()
```

## Co se jinam nevešlo ##

Modul **[domains](http://nodejs.org/docs/latest/api/domain.html)**

- bezpečnější ošetřování chyb bez ztráty kontextu
  - process.on('uncaughtException') **!!**

Náhrada shell skriptů, pipe<br/><br/>
`#!/usr/bin/env node`

## Užitečné moduly ##

- [async](https://npmjs.org/package/async)
- [underscore](https://npmjs.org/package/underscore)
- [cheerio](https://npmjs.org/package/cheerio)
- [optimist](https://npmjs.org/package/optimist)
- [mocha](https://npmjs.org/package/mocha)
- [nodemon](https://npmjs.org/package/nodemon)
- [node-dev](https://npmjs.org/package/node-dev)
- [node-inspector](https://npmjs.org/package/node-inspector)

## Další zdroje ##

- [How to Node](http://howtonode.org/)
- [Understanding the node.js event loop](http://blog.mixu.net/2011/02/01/understanding-the-node-js-event-loop/)
- [Package.json dependencies done right](http://blog.nodejitsu.com/package-dependencies-done-right)
- [Node.js](http://overapi.com/nodejs/)
- [Stream handbook](https://github.com/substack/stream-handbook)
- [Stream2 blog-post](http://blog.nodejs.org/2012/12/20/streams2/)
- [Stream2 slides](https://dl.dropbox.com/u/3685/presentations/streams2/streams2-ko.pdf)
- [Node.js - shell scripting](http://www.2ality.com/2011/12/nodejs-shell-scripting.html)
- [Modular web applications with Node.js and Express](http://vimeo.com/56166857)

## Co se jinam nevešlo ##
NPM dependence může být gitový repozitář/tar/zip.
<br/><br/>
**Pozor!** Některé package obsahují nativní C/C++ kód (je potřeba mít dev. prostředí)
<br/><br/>
Node.js umí běžet v experimentálním módu s ES6 (nová verze javascriptu)
`node --harmony`

> Common **simple tasks should be easy**, or we aren't doing our job.
> People often say that **Node is better than most other platforms at this stuff**, but in my opinion, that is less of a compliment and more of an **indictment of the current state of software**.
> **Being better than the next guy isn't enough; we have to be the best imaginable.**

<br/>
Ryan Diehl (autor Node.js)
