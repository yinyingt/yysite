---
layout: post
title: Node.js Notes
mathjax: false
related: false
comments: true
published: true
---

_Created: Oct, 2015_

_Last modified: Oct, 2015_



* Use nodemon to manage NodeJS instances. Install it by

```
sudo npm install nodemon -g
```

For NodeJS framework initiated by ExpressJS, just type "nodemon" to start the web app. 

* Use forever to manage NodeJS instances. Install it by

```
sudo npm install forever -g
```

To start an Node instances: 

```
forever start index.js
```

To restart all instances after a source update:

```
forever restartall
```

To list all instances: 

```
forever list
```

To stop an instance: 

```
forever stop 0
```

where 0 is the instance number. 

For more info, consult 

```
forever -h
```

* Problem: NodeJS cannot find modules. 
Solution: add the npm\_noduels path to "NODE\_PATH", such as 

```
NODE_PATH=C:\Users\[USERNAME]\AppData\Roaming\npm\node_modules
```

Similarly, on Linux:

```
export NODE_PATH=$NODE_PATH:/usr/local/lib/node_modules
```

for globally installed modules. 

* Create a swap file on Linux VM to increase the swap size, and prevent "npm search" from crashing (out of memory). See tutorial [here](http://codentrick.com/check-swap-file-to-prevent-npm-install-can-be-killed/). 


