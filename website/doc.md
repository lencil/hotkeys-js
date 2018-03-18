HotKeys.js is an input capture library with some very special features, it is easy to pick up and use, has a reasonable footprint (~3kb), and has no dependencies. It should not interfere with any JavaScript libraries or frameworks.


## USAGE

You will need `Node.js` installed on your system.

```shell
$ npm install hotkeys-js --save
```

```
import hotkeys from 'hotkeys-js';
```

Or manually download and link **hotkeys.js** in your HTML:

```js
<script type="text/javascript" src="hotkeys.js"></script>
```

### Used in React

[react-hotkeys](https://github.com/jaywcjlove/react-hotkeys) is the React component that listen to keydown and keyup keyboard events, defining and dispatching keyboard shortcuts.

```shell
$ npm install react-hot-keys --save
```

Detailed use method please see its documentation [react-hotkeys](https://github.com/jaywcjlove/react-hotkeys).

## Browser Support

Mousetrap has been tested and should work in.

```shell
Internet Explorer 6+
Safari
Firefox
Chrome
```

## DEFINING SHORTCUTS

One global method is exposed, key which defines shortcuts when called directly.

```js
hotkeys('f5', function(event, handler){
  // Prevent the default refresh event under WIDNOWS system
  event.preventDefault() 
  alert('you pressed F5!') 
});

hotkeys('a', function(event,handler){
  //event.srcElement: input 
  //event.target: input
  if(event.target === "input"){
      alert('you pressed a!')
  }
  alert('you pressed a!') 
});

hotkeys('ctrl+a,ctrl+b,r,f', function(event,handler){
  switch(handler.key){
    case "ctrl+a":alert('you pressed ctrl+a!');break;
    case "ctrl+b":alert('you pressed ctrl+b!');break;
    case "r":alert('you pressed r!');break;
    case "f":alert('you pressed f!');break;
  }
});

hotkeys('*','wcj', function(e){
  console.log('do something',e);
});
```

## API REFERENCE

Asterisk "*"

Modifier key judgments

```js
hotkeys('*','wcj', function(e){
  if(hotkeys.shift) console.log('shift is pressed！');
  if(hotkeys.ctrl) console.log('shift is pressed! ');
  if(hotkeys.alt) console.log('shift is pressed! ');
});
```

### setScope

Use the `hotkeys.setScope` method to set scope.

```js
// define shortcuts with a scope
hotkeys('ctrl+o, ctrl+alt+enter', 'issues', function(){
  console.log('do something');
});
hotkeys('o, enter', 'files', function(){ 
  console.log('do something else');
});

// set the scope (only 'all' and 'issues' shortcuts will be honored)
hotkeys.setScope('issues'); // default scope is 'all'
```

### getScope

Use the `hotkeys.getScope` method to get scope.

```js
hotkeys.getScope();
```

### deleteScope

Use the `hotkeys.deleteScope` method to delete set scope.

```js
hotkeys.deleteScope('issues');
```

### unbind

Similar to defining shortcuts, they can be unbound using `hotkeys.unbind`.

```js
// unbind 'a' handler
hotkeys.unbind('a');

// unbind a hotkeys only for a single scope
// when no scope is specified it defaults to the current scope (hotkeys.getScope())
hotkeys.unbind('o, enter', 'issues');
hotkeys.unbind('o, enter', 'files');
```

### isPressed

Other key queries. For example, `hotkeys.isPressed(77)` is true if the `M` key is currently pressed.

```js
hotkeys('a', function(){
  console.log(hotkeys.isPressed("a")); //=> true
  console.log(hotkeys.isPressed("A")); //=> true
  console.log(hotkeys.isPressed(65)); //=> true
});
```

### getPressedKeyCodes

returns an array of key codes currently pressed.

```js
hotkeys('command+ctrl+shift+a,f', function(){
  console.log(hotkeys.getPressedKeyCodes()); //=> [17, 65] or [70]
})
```

### filter

`INPUT` `SELECT` `TEXTAREA` default does not handle.
`Hotkeys.filter` to return to the `true` shortcut keys set to play a role, `flase` shortcut keys set up failure.

```js
hotkeys.filter = function(event){
  return true;
}
//How to add the filter to edit labels. <div contentEditable="true"></div>
//"contentEditable" Older browsers that do not support drops
hotkeys.filter = function(event) {
  var tagName = (event.target || event.srcElement).tagName;
  return !(tagName.isContentEditable || tagName == 'INPUT' || tagName == 'SELECT' || tagName == 'TEXTAREA');
}

hotkeys.filter = function(event){
  var tagName = (event.target || event.srcElement).tagName;
  hotkeys.setScope(/^(INPUT|TEXTAREA|SELECT)$/.test(tagName) ? 'input' : 'other');
  return true;
}
```

### noConflict

Relinquish HotKeys’s control of the `hotkeys` variable.

```js
var k = hotkeys.noConflict();
k('a', function() {
  console.log("do something")
});

hotkeys()
// -->Uncaught TypeError: hotkeys is not a function(anonymous function) 
// @ VM2170:2InjectedScript._evaluateOn 
// @ VM2165:883InjectedScript._evaluateAndWrap 
// @ VM2165:816InjectedScript.evaluate @ VM2165:682
```

### SUPPORTED KEYS

HotKeys understands the following modifiers: `⇧`, `shift`, `option`, `⌥`, `alt`, `ctrl`, `control`, `command`, and `⌘`.

The following special keys can be used for shortcuts: backspace, tab, clear, enter, return, esc, escape, space, up, down, left, right, home, end, pageup, pagedown, del, delete and f1 through f19.

`⌘` Command()  
`⌃` Control  
`⌥` Option(alt)  
`⇧` Shift  
`⇪` Caps Lock(Capital)  
~~`fn` Does not support fn~~  
`↩︎` return/Enter space  
 
## Development

To develop, run the self-reloading build, Get the code:

```shell
$ git https://github.com/jaywcjlove/hotkeys.git
$ cd hotkeys     # Into the directory
$ npm install    # or  yarn install
```

To develop, run the self-reloading build:

```shell
$ npm run watch
```

Run Document Website Environment.

```shell
$ npm run doc:dev
```

## License

[MIT © Kenny Wong](https://kossnocorp.mit-license.org/)

```bash
  __            __    __
 |  |--..-----.|  |_ |  |--..-----..--.--..-----.
 |     ||  _  ||   _||    < |  -__||  |  ||__ --|
 |__|__||_____||____||__|__||_____||___  ||_____|
                                   |_____|
```