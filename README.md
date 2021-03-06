![209261](https://user-images.githubusercontent.com/328000/29106270-071a5b08-7c89-11e7-8928-2a9f4bf85050.png)
# syr

[![build status](https://travis-ci.org/dmikey/syr.svg?branch=master)](https://travis-ci.org/dmikey/syr)
[![gzip size](http://img.badgesize.io/https://unpkg.com/syr@1.0.4/dist/syr.min.js?compression=gzip)](https://unpkg.com/syr@1.0.4/dist/syr.min.js)
[![Coverage Status](https://coveralls.io/repos/github/dmikey/syr/badge.svg?branch=master)](https://coveralls.io/github/dmikey/syr?branch=master)

This set of libraries and utilities aimed to help Native SDK developers, create dynamic UX for distribution. The goal of this library is provide Native SDK developers with the benefits of the React Native Eco System, with a much smaller footprint, and reduced complexity.

We want you to be able to package Syr with your Native SDK, and be able to drive dynamic experiences with it.

Syr uses a React like API with a custom JS API bridge, that allows an SDK developer to bundle an updatable experience, through Apple's iTunes terms. A carefully crafted SDK can be enhanced or tested without needing to deploy multiple paths of native code, or ask for integrators to adopt a new binary release for testing.

The ultimate goal was not to recreate React, nor chase something like Preact. Through the magic of Babel and Webpack, we've adopted a similar set of technologies that allow us the ease and familiar use, while meeting the goal of pushing AST over the JavaScript Bridges in each native environment.

The Native Libraries, are crafted that you can use a simple 'Find and Replace' method to sandbox them off from the rest of the world. We've gone out of our way to ensure portability for SDK developers, who want to the benefits of a dynamic script based UI, and the benefits of native integration.

# goal

* minally viable react compatible API
* reduce surface area of projects like React-Native
* work flow that works for SDKs and not just applications

### mvp roadmap

* web raster
* ios native raster


### contributing

* fork repo
* add tests
* open PR!

### working on web raster

* run dev sandbox with `npm run serve`
* open browser and hit `http://localhost:8080/`

### working on ios raster

* run dev sandbox with `npm run serve`
* open `ios/SyrNativeSample.xcodeproj`
* run ios project which points to `http://127.0.0.1:8080`
* use Safari Developer tools to connect to remote debugger

### docs

Syr like, Preact, aims to have a somewhat compatible React API.


Render a Syr Component. Passing the component to Render, without a target will draw the component to `document.body` when rendering in the web. If rendering natively, it will render to the `root` view.

```javascript
import { Component, Render } from 'syr';

class MyComponent extends Component {
    render() {
      return (
        <div>Hello World</div>
      )
    }
}

Render(MyComponent);
```

The same example can be written without JSX support

```javascript
mport { Component, Render } from 'syr';

class MyComponent extends Component {
  render() {
  	// pass ast directly to render
    return {
      elementName: 'div',
      children: ['Hello World']
    };
  }
}

Render(MyComponent);
```

Syr has similar support for Variable based elements, just like React.


```javascript
import { Component, Render } from 'syr';

class MyComponent extends Component {
    render() {
      return (
        <div>Hello World</div>
      )
    }
}

class MyView extends Component {
    render() {
      return (
        <MyComponent></MyComponent>
      )
    }
}

Render(MyComponent);
```

Syr uses `babel-plugin-transform-jsx` by @calebmer to build `JSX` directly to AST. The constructors of custom components are deferred until the first Render pass.


To get AST from a Syr component, simply call render directly on it after instantiating.

```javascript
import { Component, Render } from 'syr';

class MyComponent extends Component {
    render() {
      return (
        <div foo="bar">Hello World</div>
      )
    }
}

let mycomponent = new MyComponent();
let ast = mycomponent.render();
```

Returns as object that serializes like below.

``` JSON
{  
   "elementName":"div",
   "attributes":{  
      "foo":"bar"
   },
   "children":[
      "Hello World"
   ]
}
````