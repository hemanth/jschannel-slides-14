# Let me tell you a story...
---

# [fit] A story of a developer...
---

# [fit] A story of a (node) JS developer!
![](http://i.imgur.com/ZYxBDZ9.gif)

---

# [fit] A

---

# [fit] Simple 

---

# [fit] Task

---

![](http://38.media.tumblr.com/9d352c13f6090a977102de2b3d5f4218/tumblr_mpsoopWNQL1s5z0c9o1_500.gif)

---

# [fit] hmm.. that's very easy!

```javascript
function read(filename){
  return fs.readFileSync(filename, 'utf8');
}
```
![%50 left] (https://i.imgur.com/rlOQRFm.gif)

---

## [fit] Okies....say the file is like 10GB in size?

![%50 right](https://i.imgur.com/Ph6Ei.gif)

---


```javascript
function read(filename, callback){
  fs.readFile(filename, 'utf8', function (err, res){
    if (err) {
    	return callback(err);
    }
    callback(null,res);
  });
}
```

## [fit] OK, not bad...now process that file.

---

```javascript

function process(file){
  /* Some processing stuff */
}
```

```javascript
function read(filename, callback){
  fs.readFile(filename, 'utf8', function (err, res){
    if (err) {
    	return callback(err);
    }
    try {
      callback(null, process(res));
    } catch (ex) {
      callback(ex);
    }
  });
}
```

---

```javascript
function read(filename, callback){
  fs.readFile(filename, 'utf8', function (err, res){
    if (err) {
    	return callback(err);
    }
    try {
      res = process(res);
    } catch (ex) {
      return callback(ex);
    }
    callback(null, res);
  });
}
```
---

## [fit] Time passes by....

---

![](http://blog.thomasdeconinck.fr/wp-content/uploads/2014/05/callback_hell.png)

---

![](http://media.tumblr.com/efa793ea351a9dfe5352ffd6cd8c794a/tumblr_inline_mxzjz9b0HC1s0m7nr.gif)

---

## [fit] So, how do we avoid callback hell?

* Name your functions.

* Keep your code shallow.

* Modularize!

* Binding `this`

---

## [fit] When you know...then you know!

---

## [fit] Let us make a promise!

* fulfilled
    
* rejected
    
* pending
    
* settled
    
---

```javascript
var promise = new Promise(function(resolve, reject) {
  // Some async...
  if (/* Allz well*/) {
    resolve("It worked!");
  }
  else {
    reject(Error("It did not work :'("));
  }
});
```

```javascript
promise.then(function(result) {
  console.log(result); // "It worked!"
}, function(err) {
  console.log(err); // Error: "It don not work :'("
});

```
---


```javascript
async1().then(function() {
  return async2();
}).then(function() {
  return async3();
}).catch(function(err) {
  return asyncHeal1();
}).then(function() {
  return asyncHeal4();
}, function(err) {
  return asyncHeal2();
}).catch(function(err) {
  console.log("Ignore them");
}).then(function() {
  console.log("I'm done!");
});
```

---

## [lie-fs](https://www.npmjs.org/package/lie-fs)

```bash
$ npm install lie-fs
```

```javascript
fs.stat('/tmp', function(err,data){
	if(!err){
		console.log(data);
	} else {
		console.error(err);
	}
});
```

```javascript
var fsp = require('lie-fs');

fsp.stat('/tmp').then(console.log,console.error);
```

---

## async

```javascript
async.map(['file1','file2','file3'], fs.stat, function(err, results){
    // results is now an array of stats for each file
});

async.filter(['file1','file2','file3'], fs.exists, function(results){
    // results now equals an array of the existing files
});

async.parallel([
    function(){ ... },
    function(){ ... }
], callback);

async.series([
    function(){ ... },
    function(){ ... }
]);
```

---

## [fit] Happ(Y)ily ever after...

![](http://data1.whicdn.com/images/61700150/large.jpg)

---

![left](https://d262ilb51hltx0.cloudfront.net/max/1400/0*N5_gFLxHuK0nzh6y.jpeg)

> Streams are broken, callbacks are not great to work with, errors are vague, tooling is not great, community convention is sort of there..

---

* you may get duplicate callbacks
* you may not get a callback at all (lost in limbo)
* you may get out-of-band errors
* emitters may get multiple “error” events
* missing “error” events sends everything to hell
* often unsure what requires “error” handlers
* “error” handlers are very verbose
* callbacks suck

---


# [fit] Koa is the one project I’ll continue to maintain (along with Co and friends).

---

# [fit] Welcome to the new gen of *gen(){}

---

# [fit] "Harmony" is the name TC39 uses for our standardization process.

---

# [fit] [[harmony:generators]] 

---

![fit left](http://wiki.ecmascript.org/lib/exe/fetch.php?w=&h=&cache=cache&media=harmony:es6_generator_object_model_3-29-13.png)

* First-class coroutines.

* Represented as objects encapsulating suspended execution contexts.

* Prior art: Python, Icon, Lua, Scheme, Smalltalk. 

---

![](https://i.imgur.com/bnhj3.gif)

---

![fit](https://i.imgur.com/Agci8.gif)

---

 Every generator object has the following internal properties:

    * [[Prototype]] : the original value of Object.prototype.

    * [[Code]] : the code for the generator function body.

    * [[ExecutionContext]] : either null or an execution context.

    * [[Scope]] : the scope chain for the suspended execution context.

    * [[Handler]] : a standard generator handler for performing iteration.

    * [[State]] : “newborn”, “executing”, “suspended”, or “closed”.

    * [[Send]] [[Throw]] and [[Close]].

---

#[fit] Show me the code!

---

```javascript
function *Counter(){
	let n = 0;
	while(1<2) {
	  yield n;
	  n = n + 1;
	}
}
```

```javascript

let CountIter = Counter();

CountIter.next();
// Would result in { value: 0, done: false }

// Again 
CountIter.next();
//Would result in { value: 1, done: false }
```

---

```javascript
function *fibonacci() {
    let [prev, curr] = [0, 1];
    for (;;) {
        [prev, curr] = [curr, prev + curr];
        yield curr;
    }
}
```

```javascript
for (fib of fibonacci()) {
    if (fib === 42)
        break;
    console.log(fib);
}
```

---

```javascript
function *powPuff() {
  return Math.pow((yield "x"), (yield "y"));
}
```

```javascript

let puff = powPuff()

puff.next();

puff.next(2);

puff.next(3); // Guess ;)

```

---

```javascript
function* menu(){
  while (true){
    var val = yield null;
    console.log('I ate:', val);
  }
}


let meEat = menu();

meEat.next();

meEat.next("Poori");

meEat.next("Pizza");
```

---

#[fit] meEat.throw(new Error("Burp!"));

---

```javascript
function* menu(){
  while (true){
    try{
      var val = yield null;
      console.log('I ate: ', val);
    }catch(e){
      console.log('Good, now pay the bill :P');
    }
  }
}
```

```
meEat.throw(new Error("Burp!"));
```
---

# We can deligate!

```javascript
var inorder = function* inorder(node) {
  if (node) {
    yield* inorder(node.left);
    yield node.label;
    yield* inorder(node.right);
  }
}
```


---

#[fit] Deeper you must go! Hmmm

![](http://wallpaperest.com/wallpapers/the-hulk-and-yoda-digital-art_120468.jpg)

---

```javascript
function *theAnswer() {
  yield 42;
}

var ans = theAnswer(); 

ans.next();
```

#[fit]So....

---

```javascript
function theAnswer() {
  var state = 0;
  return {
    next: function() {
      switch (state) {
        case 0:
          state = 1;
          return {
            value: 42, // The yielded value.
            done: false
          };
        case 1:
          return {
            value: undefined,
            done: true
          };
      }
    }
  };
}
```

---

#[fit] But, this has not yet solved the initial issue!

---

#[fit] Let us assume a function named `run` ~_~

---

```javascript
run(genFunc){
	/*
		 _ __ ___   __ _  __ _(_) ___ 
	| '_ ` _ \ / _` |/ _` | |/ __|
	| | | | | | (_| | (_| | | (__ 
	|_| |_| |_|\__,_|\__, |_|\___|
	                 |___/        
	*/
}
```

```javascript
run(function *(){
  var data = yield read('package.json');
  var result = yield process(data);
  console.log(data);
  console.log(result);
});
```

---

![](http://a.gifb.in/062014/1404749779_shades_optical_illusion.gif)

---

```javascript
var fs = require(‘fs’);

function run(fn) {
  var gen = fn();

  function next(err, res) {
    var ret = gen.next(res);
    if (ret.done) return;
    ret.value(next);
  }
  
  next();
}
```

---

#[fit] THUNKS!

---

#[fit] let timeoutThunk = (ms) => (cb) => setTimeout(cb,ms) 

---

```javascript
function readFile(path) {
    return function(callback) {
        fs.readFile(path, callback);
    };
}
```

Instead of :

```javascript
readFile(path, function(err, result) { ... });
```

We now have:

```javascript
readFile(path)(function(err, result) { ... });
```

So that:

```javascript

var data = yield read('package.json');

```
---


![fit](http://skilldrick.co.uk/wp-content/uploads/2011/06/crockford.jpg)

Baaazinga!

---

#[fit] More usecases! Generator-based flow control.

---

```bash
$ npm install thunkify
```

> Turn a regular node function into one which returns a thunk!

```javascript

var thunkify = require('thunkify');
var fs = require('fs');

var read = thunkify(fs.readFile);

read('package.json', 'utf8')(function(err, str){

});

```

---

```bash
$ npm install co
```

> Write non-blocking code in a nice-ish way!

```javascript
var co = require('co');
var thunkify = require('thunkify');
var request = require('request');
var get = thunkify(request.get);
```

```javascript
co(function *(){
  try {
    var res = yield get('http://badhost.invalid');
    console.log(res);
  } catch(e) {
    console.log(e.code) // ENOTFOUND
 }
}());
```

---

```javascript 

var urls = [/* Huge list */];

// sequential

co(function *(){
  for (var i = 0; i < urls.length; i++) {
    var url = urls[i];
    var res = yield get(url);
    console.log('%s -> %s', url, res[0].statusCode);
  }
})()

// parallel

co(function *(){
  var reqs = urls.map(function(url){
    return get(url);
  });

  var codes = (yield reqs).map(function(r){ return r.statusCode });

  console.log(codes);
})()

```

---

#Intresting? What more?

---

```bash
$ npm install co-sleep
```

```javascript
var sleep = require('co-sleep');
var co = require('co');

co(function *() {
  var now = Date.now();
  // wait for 1000 ms
  yield sleep(1000);
  expect(Date.now() - now).to.not.be.below(1000);
})();
```

---

```bash
$ npm install co-ssh
```

```javascript
var ssh = require('co-ssh');

var c = ssh({
  host: 'n.n.n.n',
  user: 'myuser',
  key: read(process.env.HOME + '/.ssh/some.pem')
});

yield c.connect();
yield c.exec('foo');
yield c.exec('bar');
yield c.exec('baz');
```

---

```javascript
var monk = require('monk');
var wrap = require('co-monk'); // co-monk!
var db = monk('localhost/test');

var users = wrap(db.get('users'));

```

```javascript
yield users.remove({});

yield users.insert({ name: 'Hemanth', species: 'Cat' });
```

```javascript
// Par||el!
yield [
  users.insert({ name: 'Tom',   species: 'Cat' }),
  users.insert({ name: 'Jerry', species: 'Rat' }),
  users.insert({ name: 'Goffy', species: 'Dog' })
];
```

---

```bash
$ npm install suspend
```

```javascript
var suspend = require('suspend'),
    resume = suspend.resume;

suspend(function*() {
    var data = yield fs.readFile(__filename, 'utf8', resume());
    console.log(data);
})();
```

```javascript
var readFile = require('thunkify')(require('fs').readFile);

suspend(function*() {
    var package = JSON.parse(yield readFile('package.json', 'utf8'));
    console.log(package.name);
});
```

---

#[fit] Is that all?

---

#[fit] Koa FTW?!

---

```bash
$ npm install koa
```

```javascript
var koa = require('koa');
var app = koa();

// logger

app.use(function *(next){
  var start = new Date;
  yield next;
  var ms = new Date - start;
  console.log('%s %s - %s', this.method, this.url, ms);
});

// response

app.use(function *(){
  this.body = 'Hello World';
});

app.listen(3000);
```

---

![fit](http://h3manth.com/i/koajs.gif)

---

#[fit] Something for the client?

---

#[fit] Task.js

> generators + promises = tasks

```javascript
<script type="application/javascript" src="task.js"></script>

<!-- 'yield' and 'let' keywords require version opt-in -->
<script type="application/javascript;version=1.8">
function hello() {
    let { spawn, sleep } = task;
    spawn(function() { // Firefox does not yet use the function* syntax
        alert("Hello...");
        yield sleep(1000);
        alert("...world!");
    });
}
</script>
```

---

#[fit] Sweet and simple! 


```javascript
spawn(function*() {
    try {
        var [foo, bar] = yield join(read("foo.json"),
                                    read("bar.json")).timeout(1000);
        render(foo);
        render(bar);
    } catch (e) {
        console.log("read failed: " + e);
    }
});
```

---

```javascript
var foo, bar;
var tid = setTimeout(function() { failure(new Error("timed out")) }, 1000);

var xhr1 = makeXHR("foo.json",
                   function(txt) { foo = txt; success() },
                   function(err) { failure() });
var xhr2 = makeXHR("bar.json",
                   function(txt) { bar = txt; success() },
                   function(e) { failure(e) });

function success() {
    if (typeof foo === "string" && typeof bar === "string") {
        cancelTimeout(tid);
        xhr1 = xhr2 = null;
        render(foo);
        render(bar);
    }
}

function failure(e) {
    xhr1 && xhr1.abort();
    xhr1 = null;
    xhr2 && xhr2.abort();
    xhr2 = null;
    console.log("read failed: " + e);
}
```
---

#[fit] ES7 async/await! (("Proposal"))

---

# The idea is to have more sugar!

```
async function <name>?<argumentlist><body>

=>

function <name>?<argumentlist>{ return spawn(function*() <body>); }
```

---

# Example of animating elements with Promise:

```javascript
function chainAnimationsPromise(elem, animations) {
    var ret = null;
    var p = currentPromise;
    animations.forEach(function(anim){
      p = p.then(function(val) {
            ret = val;
            return anim(elem);
        });
    });
  
    return p.catch(function(e) {
        /* ignore and keep going */
    }).then(function() {
        return ret;
    });
}
```

---

# Same example with task.js:

```javascript
function chainAnimationsGenerator(elem, animations) {
    return spawn(function*() {
        var ret = null;
        try {
            for(var anim of animations) {
                ret = yield anim(elem);
            }
        } catch(e) { /* ignore and keep going */ }
        return ret;
    });
}
```
---

# Same example with async/await:

```javascript
async function chainAnimationsAsync(elem, animations) {
    var ret = null;
    try {
        for(var anim of animations) {
            ret = await anim(elem);
        }
    } catch(e) { /* ignore and keep going */ }
    return ret;
}
```

---

# Another example from the draft:

```javascript
async function getData() {
  var items = await fetchAsync('http://example.com/users');
  return await* items.map(async(item) => {
    return {
      title: item.title, 
      img: (await fetchAsync(item.userDataUrl)).img
    }
  }
}
```

---

#[fit] Performance review!

---

![](http://iwantclosure.com/wp-content/uploads/2010/05/appleorange.jpg)

---

```javascript
function * Counter() {
    var n = 0;
    while (1 < 2) {
        yield n;
        ++n
    }
}

function funcStat(fn) {
	var optimizations = {
	  1 : 'is optimized',
	  2 : 'is not optimized',
	  3 : 'is always optimized',
	  4 : 'is never optimized',
	  6 : 'maybe deoptimized'
	};

    return optimizations[% GetOptimizationStatus(fn)];
    
}


var CountIter = Counter();
CountIter.next();

% OptimizeFunctionOnNextCall(Counter);

CountIter.next();

console.log('Function: ' + funcStat(Counter));
```

---


#Function is not (yet) optimized! ~_~

```
$ node --use-strict $(node --v8-options | grep harm | awk '{print $1}'| xargs)  --trace_opt --trace_deopt --allow-natives-syntax countGen.js
```

``` 
[deoptimize global object @ 0x4af7380f3f1]
[disabled optimization for 0x33530e0470d9 <SharedFunctionInfo Counter>, reason: Generator]
[marking 0x4af73814041 <JS Function IN (SharedFunctionInfo 0x4af73812439)> for recompilation, reason: hot and stable, ICs with typeinfo: 1/2 (50%)]
[disabled optimization for 0x4af73812439 <SharedFunctionInfo IN>, reason: Call to a JavaScript runtime function]
[failed to optimize IN: Call to a JavaScript runtime function]
[marking 0x4af738148c9 <JS Function ToName (SharedFunctionInfo 0x4af73812e59)> for recompilation, reason: hot and stable, ICs with typeinfo: 1/1 (100%)]
[disabled optimization for 0x4af73812e59 <SharedFunctionInfo ToName>, reason: Call to a JavaScript runtime function]
[failed to optimize ToName: Call to a JavaScript runtime function]
[marking 0x4af73814931 <JS Function ToObject (SharedFunctionInfo 0x4af73812ee9)> for recompilation, reason: small function, ICs with typeinfo: 1/5 (20%)]
[disabled optimization for 0x4af73812ee9 <SharedFunctionInfo ToObject>, reason: Call to a JavaScript runtime function]
[failed to optimize ToObject: Call to a JavaScript runtime function]
Function: is not optimized
```

```
Function: is not optimized
```
---

```javascript
var tools = window.tools = {};

// thunk
tools.setTimeoutThunk = (ms) => (cb) => setTimeout(cb, ms);
 
tools.delegated = function*() {
  yield tools.setTimeoutThunk(1);
  yield tools.setTimeoutThunk(1);
};

tools.delegator = function*() {
  yield* tools.delegated();
};

tools.runParallel = function(fn, cb) {
  var yetToReturn = numCallsToMake = 10000;

  var fnCb = function() {
    if (0 === --yetToReturn) cb();
  };

  while (0 < numCallsToMake--) {
    fn(fnCb);
  }
};

console.log('Tools setup!');
```

---

![fit](http://h3manth.com/i/deligation.png)

---

```javascript
Benchmark.prototype.setup = function() {
    var arr = Array(100).join(Math.random()).split('.');
    function* generator() {
      for(var
        i = 0; i < this.length;
        yield this[i++]
      );
    }
    var genvar0 = 0;
    function genNext() {
        var i = genvar0;
        genvar0 = i + 1;
        if (i <= this.length) {
           return this[i];
        } else {
           return null;
        }
    }
};
```

---

![fit](http://h3manth.com/i/simulated_generator.png)

---

#[fit] Moral of the story?

---

![left](http://h3manth.com/me.jpg)

http://h3manth.com

http://nmotw.in

http://github.com/hemanth

@gnumanth

Demos -> h3manth.com/es6-generators


THANK YOU! :-)
