<a href="example.com" target="_blank">I will open in a new tab</a>

---
### Templates

Most of your domvm code will consist of templates for creating virtual-dom trees, which in turn are used to render and redraw the DOM.
domvm exposes several factory functions to get this done. Commonly this is called [hyperscript](https://github.com/hyperhype/hyperscript).

For convenience, we'll alias each factory function with a short variable:

```js
var el = domvm.defineElement,
	tx = domvm.defineText,
	cm = domvm.defineComment,
	vw = domvm.defineView,
	iv = domvm.injectView,
	ie = domvm.injectElement;
```

<!-- TODO
domvm.defineElementSpread
domvm.defineFragment
domvm.defineSvgElement
-->

Using `defineText` isn't strictly necessary since all encountered numbers and strings will be automatically converted into `defineText` vnodes for you.
Additionally, there's a convenience factory `domvm.h` that can discern `defineElement`, `defineView`, `injectView` and `injectElement` based on the provided arguments.

Below is a dense reference of most template semantics. Pay attention, there's a lot of neat stuff in here that won't be covered later!

```js
el("p", "Hello")											// plain tags
el("textarea[rows=10]#foo.bar.baz", "Hello")				// attr, id & class shorthands
el(".kitty", "Hello")										// "div" can be omitted from tags

el("input",  {type: "checkbox",    checked: true})			// boolean attrs
el("input",  {type: "checkbox", ".checked": true})			// set property instead of attr

el("button", {onclick: myFn}, "Hello")						// event handlers
el("button", {onclick: [myFn, arg1, arg2]}, "Hello")		// parameterized
el("ul",     {onclick: {".item": myFn}}, ...)				// delegated
el("ul",     {onclick: {".item": [myFn, arg1, arg2]}}, ...)	// delegated & parameterized

el("p",      {style: "font-size: 10pt;"}, "Hello")			// style can be a string
el("p",      {style: {fontSize: "10pt"}}, "Hello")			// or an object (camelCase only)
el("div",    {style: {width: 35}},        "Hello")			// "px" will be added when needed

el("h1", [													// attrs object is optional
	el("em", "Important!"),
	"foo", 123,												// plain values
	ie(myElement),											// inject existing DOM nodes
	el("br"),												// void tags without content
	"", [], null, undefined, false,							// these will be auto-removed
	NaN, true, {}, Infinity,								// these will be coerced to strings
	[														// nested arrays will get flattened
		el(".foo", {class: "bar"}, [						// short & attr class get merged: .foo.bar
			"Baz",
			el("hr"),
		])
	],
])

el("#ui", [
	vw(NavBarView, navbar),									// sub-view w/model
	vw(PanelView, panel, "panelA"),							// sub-view w/model & key
	iv(someOtherView),										// injected external ViewModel
])

// special _* props

el("p", {_raw: true}, "<span>A am text!</span>")			// raw innerHTML body, CAREFUL!
el("p", {_key: "myParag"}, "Some text")						// keyed nodes
el("p", {_data: {foo: 123}}, "Some text")					// per-node data (faster than attr)

el("p", {_ref: "myParag"}, "Some text")						// named refs (vm.refs.myParag)
el("p", {_ref: "pets.james"}, "Some text")					// namespaced (vm.refs.pets.james)
```

<!-- TODO
_flags:
namespaced/exposed refs
vw(fn, model, key, opts)
-->
