# Node Basic
### Installing Node
> The best way to install node on ubuntu is using nvm. If you are on macOS or Windows then just use the installer. Do not use the package manger `apt` on ubuntu.   
* How to installing using NVM:
* Use bash instead of ZSH:
```bash
exec bash
```

```bash
sudo apt-get update
sudo apt-get install build-essential libssl-dev
```

Once the prerequisite packages are installed, you can pull down the nvm installation script from the project's GitHub page. The version number may be different, but in general, you can download it with curl:

```
curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh -o install_nvm.sh
```

And inspect the installation script with nano:

```
nano install_nvm.sh
```

Run the script with bash:

```
bash install_nvm.sh
```

It will install the software into a subdirectory of your home directory at `~/.nvm`. It will also add the necessary lines to your`~/.profile` file to use the file.

To gain access to the nvm functionality, you'll need to log out and log back in again, or you can source the ~/.profile file so that your current session knows about the changes:

```
source ~/.profile
```

> Now we need to use ZSH  
```bash
exec zsh

or just reset terminal
```

Now that you have nvm installed, you can install isolated Node.js versions.

To find out the versions of Node.js that are available for installation, you can type:

```
nvm ls-remote
```

Now you can install whichever version you want. LTS is preferred:

```
nvm install "LTS version"
```

Usually, nvm will switch to use the most recently installed version. You can explicitly tell nvm to use the version we just downloaded by typing:

```
nvm use "version"
```

When you install Node.js using nvm, the executable is called node. You can see the version currently being used by the shell by typing:

```
node -v
```

If you have multiple Node.js versions, you can see what is installed by typing:

`nvm ls`

If you wish to default one of the versions, you can type:

`nvm alias default "version"`

This version will be automatically selected when a new session spawns. You can also reference it by the alias like this:

`nvm use default`

Each version of Node.js will keep track of its own packages and has npm available to manage these.

*To update NVM*
Latest version:
```
nvm install node --reinstall-packages-from=node
```
Stable (LTS) version:
```
nvm install lts/* --reinstall-packages-from=node
```

### Node not working from SUDO
> My solution is to create symbolic links from the versions of node and npm I'm using to /usr/local/bin:  ```bash
sudo ln -s "$NVM_DIR/versions/node/$(nvm version)/bin/node" "/usr/local/bin/node"sudo ln -s "$NVM_DIR/versions/node/$(nvm version)/bin/npm" "/usr/local/bin/npm"
```* This makes npm and node available to all users.

- - - -

## Getting Started

Example of Node on the CLI:
```bash
node
> js goes here
// Press enter to run code and shift + enter makes a new line

> .exit
// This will exit Node. Also pressing ctrl + c twice works as well.

> global
// In js on the web console, window command show global object to everything you have access to. 
// In Node, global stores a lot of the same thing. 

> process
// This is like document on the web console. We have a lot of information about the specific node project that is being executed.

> process.exit(0);
// This will also shut down the current Node process. (0) means it exited without error. 
```

Show version:
```bash
node -v
```


## Why is Node so good and becoming so popular?
Node uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Also, Node’s package ecosystem is the largest ecosystem of open source libraries in the world. Non-blocking means we don’t need to wait for request after starting an event. 

![](Node%20Basic/Screen%20Shot%202018-05-06%20at%205.04.43%20AM.png)

Node JS is singled threaded, but this isn’t as much of a problem as people make it out to be. It is also more complicated than that. 
The best thing about NODEJS is NPM. It has a fantastic community of developers. 

- - - -
# Section 1

## Writing our first NODEJS app. 

```js
console.log('Hello, World!');
```

```bash
cd "Directory"
node "app name"
```


## Using Require
* Require lets us load in modules are bundled with JS
* Require lets us load in third party libraries like express
* Require lets us require our own files. 

[Node API](https://nodejs.org/api/)

```bash
mkdir notes-node
cd notes-node
touch app.js
```

```js
console.log("Starting app.");

const fs = require('fs');
const os = require('os');

var user = os.userInfo();

fs.appendFile("greetings.txt", "Hello, " + user.username + "!");

// or to get rid of a possible warning
fs.appendFile("greetings.txt", "Hello, World!", function (err) {
	if (err) {
		console.log("Unable to write to file");
	}
});

// Another thing to check out is appendFileSync
// Also notice the use of template strings
fs.appendFileSync("greetings.txt", `Hello, ${user.username}!`);
```


## Requiring Your Own Files

Create file `notes.js` in the same directory as `app.js` and `greetings.txt`

`notes.js`
```js
console.log('Starting notes.js');
console.log(module);
// Notice exports is useful

module.exports.age = 25;
module.exports.addNote = () => {
	console.log('addNote');
	return 'New Note';
};

module.exports.add = (a, b) => {
	return a + b;
};

```

Relative paths always start with `./`

`app.js`
```js
console.log('Starting app.js');

const fs = require('fs');
const os = require('os');
const notes = require('./notes.js');

var user = os.userInfor();
var res = notes.addNote();


console.log(res);
console.log('Result: ', notes.add(9, -2));

fs.appendFile('greetings.txt', `Hello, ${user.username}! You are ${notes.age}.`);
```


## Using Third Party Modules
* `npm -v` checks version
* `npm init` starts a new node app and create a `package.json` file.
* `npm install lodash —save` installs lodash and saves it to `package.json` file. 
* `require(‘lodash’);` will load that third party module. Node is first going to look for a core module and then find third party. 
* Lodash is a bunch of useful tools. 
* Lodash can help filter an array. 
* Lodash can also type check and sort. 
* Don’t take the `node_modules` folder with you when you save or put on GitHub. Just put it in `.gitignore`


## Restart the App with Nodemon
* `npm install nodemon -g`
* This is a command line tool that we install globally. 
* `nodemon app.js` will run the app just like `node`, but it looks for changes. 
* Shut down nodemon with ctrl + c. 


## Getting Input From user
* This makes scripts awesome.
* socket.io can get real time info from web app.
* create our own api for Ajax request.
* And we can create a note , return a note, or delete a note. 
* Can’t use nodemon when we need user input?
* We can pass in arguments on the command line
* to access command line arguments:
```js
console.log(process.argv);
```
* Ways to formate CLI argument:
```bash
node app.js remove --title="secrets 2"
```
* Going to use `yargs` to pass in command line argument easier.


## Simplified Input with Yargs
* This is an NPM third party module
* It has different features to validating and formatting input
* GitHub.com/yargs/yargs
* `npm install yargs@4.7.1 --save` user @ to install specific version
* Now, you can make something like `const argv = yargs.argv;`


`app.js`
```js
const fs = require('fs');
const _ = require('lodash');
const yargs = require('yargs');

const notes = require('./notes.js');

const titleOptions = {
  describe: 'Title of note',
  demand: true,
  alias: 't'
};
const bodyOptions = {
  describe: 'Body of note',
  demand: true,
  alias: 'b'
};
const argv = yargs
  .command('add', 'Add a new note', {
    title: titleOptions,
    body: bodyOptions
  })
  .command('list', 'List all notes')
  .command('read', 'Read a note', {
    title: titleOptions,
  })
  .command('remove', 'Remove a note', {
    title: titleOptions
  })
  .help()
  .argv;
var command = argv._[0];

if (command === 'add') {
  var note = notes.addNote(argv.title, argv.body);
  if (note) {
    console.log('Note created');
    notes.logNote(note);
  } else {
    console.log('Note title taken');
  }
} else if (command === 'list') {
  var allNotes = notes.getAll();
  console.log(`Printing ${allNotes.length} note(s).`);
  allNotes.forEach((note) => notes.logNote(note));
} else if (command === 'read') {
  var note = notes.getNote(argv.title);
  if (note) {
    console.log('Note found');
    notes.logNote(note);
  } else {
    console.log('Note not found');
  }
} else if (command === 'remove') {
  var noteRemoved = notes.removeNote(argv.title);
  var message = noteRemoved ? 'Note was removed' : 'Note not found';
  console.log(message);
} else {
  console.log('Command not recognized');
}
```

`notes.js`
```js
const fs = require('fs');

var fetchNotes = () => {
  try {
    var notesString = fs.readFileSync('notes-data.json');
    return JSON.parse(notesString);
  } catch (e) {
    return [];
  }
};

var saveNotes = (notes) => {
  fs.writeFileSync('notes-data.json', JSON.stringify(notes));
};

var addNote = (title, body) => {
  var notes = fetchNotes();
  var note = {
    title,
    body
  };
  var duplicateNotes = notes.filter((note) => note.title === title);

  if (duplicateNotes.length === 0) {
    notes.push(note);
    saveNotes(notes);
    return note;
  }
};

var getAll = () => {
  return fetchNotes();
};

var getNote = (title) => {
  var notes = fetchNotes();
  var filteredNotes = notes.filter((note) => note.title === title);
  return filteredNotes[0];
};

var removeNote = (title) => {
  var notes = fetchNotes();
  var filteredNotes = notes.filter((note) => note.title !== title);
  saveNotes(filteredNotes);

  return notes.length !== filteredNotes.length;
};

var logNote = (note) => {
  console.log('--');
  console.log(`Title: ${note.title}`);
  console.log(`Body: ${note.body}`);
};

module.exports = {
  addNote,
  getAll,
  getNote,
  removeNote,
  logNote
};
```

`notes-data.json`
```json
[{"title":"to buy","body":"food"},{"title":"to buy from store","body":"food"},{"title":"things to do","body":"go to post office"}]
```

`package.json`
```json
{
  "name": "notes-node",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Andrew Mead",
  "license": "ISC",
  "dependencies": {
    "lodash": "^4.13.1",
    "yargs": "^4.7.1"
  }
}

```

`./playground/arrow-functions.js`
```js
var square = x => x * x;
console.log(square(9));

var user = {
  name: 'Andrew',
  sayHi: () => {
    console.log(arguments);
    console.log(`Hi. I'm ${this.name}`);
  },
  sayHiAlt () {
    console.log(arguments);
    console.log(`Hi. I'm ${this.name}`);
  }
};

user.sayHi(1, 2, 3);
```

`./playground/debugging.js`
```js
var person = {
  name: 'Andrew'
};

person.age = 25;

debugger;

person.name = 'Mike';

console.log(person);
```

`./playground/json.js`
```js
// var obj = {
//   name: 'Andrew'
// };
// var stringObj = JSON.stringify(obj);
// console.log(typeof stringObj);
// console.log(stringObj);

// var personString = '{"name": "Andrew","age": 25}';
// var person = JSON.parse(personString);
// console.log(typeof person);
// console.log(person);

const fs = require('fs');

var originalNote = {
  title: 'Some title',
  body: 'Some body'
};
var originalNoteString = JSON.stringify(originalNote);
fs.writeFileSync('notes.json', originalNoteString);

var noteString = fs.readFileSync('notes.json');
var note = JSON.parse(noteString);
console.log(typeof note);
console.log(note.title);
```

`./playground/notes.json`
```json
{"title":"Some title","body":"Some body"}
```


- - - -

## Serving up HTML

```js
app.get('/Liteconomy', function (req, res) {
   res.sendfile(__dirname + '/Liteconomy/index.html');
});
```

- - - -

## Standard `server.js`

```js
const express = require("express");const mongoose = require("mongoose");const bodyParser = require("body-parser");const passport = require("passport");const path = require("path");const app = express();/* Port for Heroku or locally */const port = process.env.PORT || 5000;app.listen(port, () => console.log(`Server running on port ${port}`));
```

```json
{  "name": "thebloxoffice",  "version": "1.0.0",  "description": "House and Techno tickets empowering fans to produce their own events. We ask the questions. The community makes the decisions. Together we create the rave.",  "main": "server.js",  "scripts": {    "test": "echo \"Error: no test specified\" && exit 1",    "start": "node server.js",    "server": "nodemon server.js"  },  "repository": {    "type": "git",    "url": "git+https://github.com/TheBloxOfficeLLC/TheBloxOffice.git"  },  "author": "Michael Frieze",  "license": "ISC",  "bugs": {    "url": "https://github.com/TheBloxOfficeLLC/TheBloxOffice/issues"  },  "homepage": "https://github.com/TheBloxOfficeLLC/TheBloxOffice#readme",  "dependencies": {    "bcryptjs": "^2.4.3",    "body-parser": "^1.18.2",    "express": "^4.16.3",    "jsonwebtoken": "^8.2.1",    "mongoose": "^5.0.17",    "passport": "^0.4.0",    "passport-jwt": "^4.0.0",    "validator": "^10.1.0"  },  "devDependencies": {    "nodemon": "^1.17.4"  }}
```


- - - -


## Server Up Static Files

```js
const express = require("express");const mongoose = require("mongoose");const bodyParser = require("body-parser");const passport = require("passport");var serveStatic = require("serve-static");const path = require("path");const app = express();app.use(serveStatic(path.join(__dirname, "splash-page")));app.use(express.static(path.join(__dirname, "slash-page")));/* Port for Heroku or locally */const port = process.env.PORT || 8000;app.listen(port, () => console.log(`Server running on port ${port}`));
```


- - - -


## Setting Up `package.json` Dependencies

If these dependencies are already included in the `package.json` file, then they should be available after:
```
npm install
```

### Lint
/Local Installation and Usage/
If you want to include ESLint as part of your project's build system, we recommend installing it locally. You can do so using npm:

```
$ npm install eslint --save-dev
```

You should then setup a configuration file:

```
$ ./node_modules/.bin/eslint --init
```

Go through the config. Choose `AIRBNB` popular configuration and `JSON` format. 

After that, you can run ESLint on any file or directory like this:

```
$ ./node_modules/.bin/eslint yourfile.js
```

Any plugins or shareable configs that you use must also be installed locally to work with a locally-installed ESLint.

`package.json`
```json
  "scripts": {    "lint-init": "./node_modules/.bin/eslint --init",    "lint": "./node_modules/.bin/eslint **/*.{js,jsx} --quiet"  },
```

Always initialize lint after `npm install`
```
npm run lint-init
```

To run lint:
```
npm run lint
```


### Prettier
/Local Installation and Usage/
If you want to include Prettier as part of your project's build system, we recommend installing it locally. You can do so using npm:

```
npm install prettier --save-dev
```

Now, update `package.json` scripts:

```json
  "scripts": {    "format":      "prettier --write --single-quote --print-width=120 --parser=flow --tab-width=2 \"{*.{js,jsx},!(node*)**/*.{js,jsx}}\"",  },
```

This way seems to work as well:
```json  "scripts": {    "format":      "prettier --write --single-quote --print-width=120 --parser=flow --tab-width=2 **/*.{js,jsx}",  },
```

To run prettier:
```
npm run format
```
