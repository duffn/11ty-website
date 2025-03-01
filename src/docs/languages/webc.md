---
eleventyNavigation:
  parent: Template Languages
  key: WebC
  order: 3
logoImage: "./src/img/logos/webc.png"
layout: layouts/langs.njk
relatedLinks:
---
{% tableofcontents "open" %}

| Type | Value |
| --- | --- |
| Eleventy Name | `webc` |
| File Extension | `*.webc` |
| npm | [`@11ty/webc`](https://www.npmjs.com/package/@11ty/webc) and [`@11ty/eleventy-plugin-webc`](https://www.npmjs.com/package/@11ty/eleventy-plugin-webc) |
| GitHub | [`11ty/webc`](https://github.com/11ty/webc) and [`11ty/eleventy-plugin-webc`](https://github.com/11ty/eleventy-plugin-webc) |

## Why use WebC?

* Brings first-class **components** to Eleventy.
  * Expand any HTML element (including custom elements) to HTML with defined conventions from web standards.
  * This means that Web Components created with WebC are compatible with server-side rendering (without duplicating author-written markup)
  * WebC components are [Progressive Enhancement friendly](https://www.youtube.com/watch?v=p0wDUK0Z5Nw).
* Get first-class **incremental builds** (for page templates, components, and Eleventy layouts) when [used with `--incremental`](/docs/usage/#incremental-for-partial-incremental-builds)
* Streaming friendly (stream on the Edge 👀)
* Easily scope component CSS (or use your own scoping utility).
* Tired of importing components? Use global or per-page no-import components.
* Shadow DOM friendly (works with or without Shadow DOM)
* All configuration extensions/hooks into WebC are async-friendly out of the box.
* Bundler mode: Easily roll up the CSS and JS in-use by WebC components on a page for page-specific bundles. Dirt-simple critical CSS/JS to only load the code you need.
* For more complex templating needs, render any existing Eleventy template syntax (Liquid, markdown, Nunjucks, etc.) inside of WebC.
* Works great with [is-land](/docs/plugins/partial-hydration/) for web component hydration.

## Resources

* {% indieavatar "https://11ty.rocks/" %}[Introduction to WebC (11ty.rocks)](https://11ty.rocks/posts/introduction-webc/) by {% indieavatar "https://darthmall.net/" %}W. Evan Sheehan
* {% indieavatar "https://11ty.rocks/" %}[Understanding WebC Features and Concepts (11ty.rocks)](https://11ty.rocks/posts/understanding-webc-features-and-concepts/) by {% indieavatar "https://thinkdobecreate.com/" %}Stephanie Eckles
* [WebC Number Counter Example Source Code and Demo](https://github.com/11ty/demo-webc-counter)
* [Seven Demos of Progressive Enhancement using Image Comparison Components](https://demo-webc-image-compare.netlify.app/) and [Source Code](https://github.com/11ty/demo-webc-image-compare)
* [First Experience Building with Eleventy's WebC Plugin](https://www.raymondcamden.com/2022/10/16/first-experience-building-with-eleventys-webc-plugin)

<div class="youtube-related">
  {%- youtubeEmbed "X-Bpjrkz-V8", "Crash Course in Eleventy’s new WebC Plugin" -%}
  {%- youtubeEmbed "p0wDUK0Z5Nw", "Interactive Progressively-enhanced Web Components with WebC" -%}
  {%- youtubeEmbed "iZvhQ484V8s", "Server-rendered Image Comparison Component", 1552 -%}
</div>

* {% indieavatar "https://zachleat.com/" %}[zachleat.com: Adding Components to Eleventy with WebC](https://www.zachleat.com/web/webc-in-eleventy/): a brief history of the motivation behind WebC including influences from the Svelte and Vue communities.
* {% indieavatar "https://darthmall.net/" %}[11ty.webc.fun](https://11ty.webc.fun/): a collection of WebC recipes!
* {% indieavatar "https://www.robincussol.com/" %}[Robin Cussol: Optimize your img tags with Eleventy Image and WebC](https://www.robincussol.com/optimize-your-img-tags-with-eleventy-image-and-webc/)


## Installation

{% callout "info", "md" %}Note that WebC support in Eleventy is **not bundled** with core! You must install the officially supported Eleventy plugin and the plugin **requires Eleventy {{ "2.0.0-canary.16" | coerceVersion }}** or newer.{% endcallout %}

It’s on [npm at `@11ty/eleventy-plugin-webc`](https://www.npmjs.com/package/@11ty/eleventy-plugin-webc)!

```
npm install @11ty/eleventy-plugin-webc
```

To add support for `.webc` files in Eleventy, add the plugin in your Eleventy configuration file:

{% codetitle ".eleventy.js" %}

```js
const pluginWebc = require("@11ty/eleventy-plugin-webc");

module.exports = function(eleventyConfig) {
	eleventyConfig.addPlugin(pluginWebc);
};
```

_You’re only allowed one `module.exports` in your configuration file. If you already have a configuration file, only copy the `require` and the `addPlugin` lines above!_

<details>
<summary><strong>Full options list</strong> (defaults shown)</summary>

```js
const pluginWebc = require("@11ty/eleventy-plugin-webc");

module.exports = function(eleventyConfig) {
	eleventyConfig.addPlugin(pluginWebc, {
		// Glob to find no-import global components
		// (The default changed from `false` in Eleventy WebC v0.7.0)
		components: "_components/**/*.webc",

		// Adds an Eleventy WebC transform to process all HTML output
		useTransform: false,

		// Additional global data used in the Eleventy WebC transform
		transformData: {},
	});
};
```

</details>


## Usage

There are a few different ways to use WebC in Eleventy:

1. Add a new `.webc` file to your Eleventy input directory
1. Use the Render plugin in an existing non-WebC template
1. Pre-process HTML input as WebC
1. Post-process HTML output as WebC

### Add a new `.webc` file

Adding the plugin will enable support for `.webc` files in your Eleventy project. Just make a new `.webc` HTML file in your Eleventy input directory and Eleventy will process it for you! Notably, `.webc` files will operate [WebC in bundler mode](https://github.com/11ty/webc#aggregating-css-and-js), aggregating the CSS and JS in use on each individual page to create a bundle of the assets in use on the page.

WebC uses an HTML parser to process input files: use any HTML here!

{% codetitle "my-page.webc" %}

```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8">
		<title>WebC Example</title>
	</head>
	<body>
		WebC *is* HTML.
	</body>
</html>
```

### Use the Render plugin

Using Eleventy’s built-in [Render plugin](/docs/plugins/render/) allows you to render WebC inside of an existing Liquid, Nunjucks, or 11ty.js template.

<is-land on:visible import="/js/seven-minute-tabs.js">
<seven-minute-tabs>
  {% renderFile "./src/_includes/syntax-chooser-tablist.11ty.js", {id: "webc-render"} %}
  <div id="webc-render-liquid" role="tabpanel">

{% codetitle "Liquid", "Syntax" %}

{% raw %}
```liquid
{% renderTemplate "webc" %}
<my-custom-component></my-custom-component>
{% endrenderTemplate %}
```
{% endraw %}

  </div>
  <div id="webc-render-njk" role="tabpanel">

{% codetitle "Nunjucks", "Syntax" %}

{% raw %}
```njk
{% renderTemplate "webc" %}
<my-custom-component></my-custom-component>
{% endrenderTemplate %}
```
{% endraw %}

  </div>
  <div id="webc-render-js" role="tabpanel">

{% codetitle "JavaScript", "Syntax" %}

{% raw %}
```js
module.exports = async function() {
  let content = await this.renderTemplate(`<my-custom-component></my-custom-component>`, "webc");
  return content;
};
```
{% endraw %}

  </div>
  <div id="webc-render-hbs" role="tabpanel">

The `renderTemplate` shortcode [requires an async-friendly template language](#template-compatibility) and is not available in Handlebars.

  </div>
</seven-minute-tabs>
</is-land>

### Pre-process HTML input as WebC

You can use the configuration option to change the default HTML preprocessor (from `liquid`) to `webc`. This might look like `htmlTemplateEngine: "webc"`. Read more on the [Eleventy documentation: Default Template Engine for HTML Files](/docs/config/#default-template-engine-for-html-files).

### Post-process HTML output as WebC

This is a (last-resort?) catch-all option to let WebC process `.html` output files in your project (skipping any `.webc` input files to avoid double-processing templates). This feature makes use of [Eleventy transforms](/docs/config/#transforms) and is most useful when you want to get up and running with WebC on an existing project quickly.

A few drawbacks to the transform method:

1. This is the slowest build-performance method to implement WebC in a project, so try the other methods first!
2. The WebC Eleventy transform operates with [bundler mode disabled](#css-and-js-(bundler-mode)), which means that processes WebC but _does not_ aggregate component JS or CSS.

<details>
<summary>The transform is disabled by default, you will need to use the <code>useTransform</code> option to enable it.</summary>

```js
const pluginWebc = require("@11ty/eleventy-plugin-webc");

module.exports = function(eleventyConfig) {
	eleventyConfig.addPlugin(pluginWebc, {
		useTransform: true,
	});
};
```

</details>

## WebC Reference

Note that all `webc:` attributes are removed from the rendered output HTML.

### HTML-only components

* _Related: [Defining Components in WebC](#defining-components)_

When a component has only content HTML (no CSS or JavaScript) it will ignore the host component tag in the output HTML. This enables HTML-only components to have zero overhead HTML. _(You can opt-out of this behavior with `webc:keep`.)_

<details>
<summary>Expand for Example</summary>

{% callout "info", "md" %}WebC components are not limited to custom element name restrictions (e.g. `my-component`) here. You can use `p`, `blockquote`, `h1`, `img`, or any valid HTML tag name.{% endcallout %}

{% codetitle "page.webc" %}

```html
<!doctype html>
<title>WebC Example</title>
<my-component></my-component>
```

{% codetitle "components/my-component.webc" %}

```html
Components don’t need a root element, y’all.
```

Outputs:

{% codetitle "_site/page.html" %}

```html
<!doctype html>
<html>
	<head>
		<title>WebC Example</title>
	</head>
  <body>
  	Components don’t need a root element, y’all.
  </body>
</html>
```

</details>

### Asset bundling

For components that are _not_ HTML-only (they _do_ have CSS or JS), WebC will include the component tag in the output markup (e.g. `<my-component>`) (for styling or client scripting). _(You can opt-out of this behavior with `webc:nokeep`.)_

<details>
<summary>Expand for Example</summary>

{% callout "info", "md" %}WebC components are not limited to custom element name restrictions (e.g. `my-component`) here. You can use `p`, `blockquote`, `h1`, `img`, or any valid HTML tag name.{% endcallout %}

{% codetitle "page.webc" %}

```html
<!doctype html>
<title>WebC Example</title>
<my-component></my-component>
```

{% codetitle "components/my-component.webc" %}

```html
Components don’t need a root element, y’all.
<style>/* Hi */</style>
```

Outputs:

{% codetitle "_site/page.html" %}

```html
<!doctype html>
<html>
	<head>
		<title>WebC Example</title>
	</head>
  <body>
  	<my-component>Components don’t need a root element, y’all.</my-component>
  </body>
</html>
```

</details>

Eleventy runs WebC in Bundler mode. That means that when it finds `<style>`, `<link rel="stylesheet">`, or `<script>` elements in component definitions they are removed from the output markup and _their content_ is aggregated together for re-use in asset bundles on the page. Read more about [CSS and JS in WebC](#css-and-js-(bundler-mode)). _(You can opt-out of this behavior with `webc:keep`.)_

### `webc:keep`

With an [HTML-only component](#html-only-components), you can use `webc:keep` on the host component to keep the tag around:

```html
<html-only-component webc:keep></html-only-component>
```

You can also use `webc:keep` to opt-out of [asset bundling](#asset-bundling) for individual elements inside of a component definition:

```html
<style webc:keep></style>
<script webc:keep></script>
```

You can also use `webc:keep` to save a [`<slot>`](#slots) for use in a client-side custom element.

### `webc:nokeep`

With an CSS/JS component (not an [HTML-only component](#html-only-components)), you can use `webc:nokeep` on the host component to drop the tag:

```html
<css-js-component webc:nokeep></css-js-component>
```

### `webc:import`

WebC will expand any component it finds using known components. You can also use `webc:import` to inline import a component definition. This import path is relative to the component file path. WebC checks for circular component dependencies and throws an error if one is encountered.

* _Related: [Defining Components in WebC](#defining-components) (global or scoped)_

```html
<any-tag-name webc:import="./components/my-component.webc"></any-tag-name>
```

{% addedin "@11ty/webc@0.6.2" %}You can import directly from an installed npm package. Eleventy will begin to supply WebC components with existing plugins. The Syntax Highlighter (`4.2.0` or newer) supplies one that you can use today:

```html
<syntax-highlight language="js" webc:import="npm:@11ty/eleventy-plugin-syntaxhighlight">
function myFunction() {
  return true;
}
</syntax-highlight>
```

This uses the component tag name (`syntax-highlight`) to look for a WebC component at `node_modules/@11ty/eleventy-plugin-syntaxhighlight/syntax-highlight.webc` and imports it for use on this node. This works with a tag name override via `webc:is` too.

### `webc:if`

_(WebC v0.7.1+)_

Use `webc:if` to conditionally render elements. Accepts arbitrary JavaScript (and is async-friendly). Similar to dynamic attributes, this also has access to component attributes and properties.

```html
<div webc:if="true">This will render</div>
<div webc:if="false">This will not render</div>
<div webc:if="myAsyncHelper()">If the helper promise resolves to a truthy value, this will render</div>
```

For more complex conditionals, `webc:type="js"` _(WebC v0.7.1+)_ is recommended (read more below).

### Slots

Child content optionally precompiles using `<slot>` and `[slot]` too. This example is using an [HTML-only component](#html-only-components).

{% codetitle "page.webc" %}

```html
<my-component></my-component>
<my-component>This is the default slot</my-component>
```

{% codetitle "components/my-component.webc" %}

```html
<p><slot>Fallback slot content</slot></p>
```

Compiles to:

```html
<p>Fallback slot content</p>
<p>This is the default slot</p>
```

If your WebC component wants to _output_ a `<slot>` tag in the compiled markup (for use in client JavaScript), use the [`webc:keep` attribute](#webckeep) (e.g. `<slot webc:keep>`).

{% callout "info", "md" %}Per web component standard conventions, if your component file contains *no content markup* (e.g. empty or only `<style>`/`<script>`), `<slot></slot>` is implied and the default slot content will be included automatically. If the WebC component file does contain content markup, the content passed in as the default slot requires `<slot>` to be included.{% endcallout %}

#### Named slots

This works with named slots (e.g. `<span slot="named-slot">`) too.

<details>
<summary>Expand for Example</summary>

{% codetitle "page.webc" %}

```html
<my-component>
	This is the default slot.
	<strong slot="named-slot">This is a named slot</strong>
	This is also the default slot.
</my-component>
```

{% codetitle "components/my-component.webc" %}

```html
<p><slot name="named-slot"></slot></p>
```

Compiles to:

```html
<p><strong>This is a named slot.</strong></p>
```

</details>

### Attributes and `webc:root`

{% codetitle "page.webc" %}

```html
<my-component class="sr-only"></my-component>
```

Inside of your component definition, you can add attributes to the outer host component using `webc:root`:

{% codetitle "components/my-component.webc" %}

```html
<template webc:root class="another-class">
	Some component content
</template>
```

`class` and `style` attribute values are _merged_ as expected between the host component and the `webc:root` element.

#### Override the host component tag

You can use `webc:root webc:keep` together to override the host component tag name! This isn’t very useful for HTML-only components (which leave out the host component tag) but is very useful when your component has style/scripts.

{% codetitle "components/my-component.webc" %}

```html
<button webc:root webc:keep>Some component content</button>
<style>/* Hi */</style>
```

### Props (Properties)

Make any attribute into a prop by prefixing it with `@`. Props are “private” attributes that don’t end up in the output HTML (they are private to WebC). They are identical to attributes except that they are filtered from the output HTML.

{% codetitle "page.webc" %}

```html
<my-component @prop="Hello"></my-component>
```

{% codetitle "components/my-component.webc" %}

```html
<p @text="prop"></p>
<!-- outputs <p>Hello</p> -->
```

* In the HTML specification, attribute names are lower-case. {% addedin "@11ty/webc@0.8.0" %}Attribute or property names with dashes are converted to camelcase for JS (e.g. `<my-component @prop-name="test">` can be used like `@text="propName"`). More at [issue #71](https://github.com/11ty/webc/issues/71).

### Dynamic attributes

Make any attribute into a dynamic attribute by prefixing it with `:`. You have access to host component attributes, props, and page data here!

{% codetitle "page.webc" %}

```html
<avatar-image src="my-image.jpeg" alt="Zach is documenting this project"></avatar-image>
```

{% codetitle "components/avatar-image.webc" %}

```html
<img :src="src" :alt="alt" class="avatar-image">
```

* In the HTML specification, attribute names are lower-case. {% addedin "@11ty/webc@0.8.0" %}Attribute or property names with dashes are converted to camelcase for JS (e.g. `<my-component @prop-name="test">` can be used like `@text="propName"`). More at [issue #71](https://github.com/11ty/webc/issues/71).
* {% addedin "@11ty/webc@0.5.0" %}`this.` is no longer required in dynamic attributes (e.g. `this.src`/`this.alt`) when referencing helpers/data/attributes/property values.


### `@html`

We surface a special `@html` [prop](#props-(properties)) to override any tag content with custom JavaScript.

```html
<template @html="'Template HTML'"></template>
<template @html="dataProperty"></template>
```

```html
<!-- webc:nokeep will replace the outer element -->
<template @html="'Template HTML'" webc:nokeep></template>
```

* Content returned from the `@html` prop will be processed as WebC—return any WebC content here! {% addedin "@11ty/webc@0.5.0" %}
* {% addedin "@11ty/webc@0.5.0" %}`this.` is no longer required in `@html` or `@raw` (e.g. `this.dataProperty`) when referencing helpers/data/attributes/property values.

```html
<!-- No reprocessing as WebC (useful in Eleventy layouts) -->
<!-- Where `myHtmlContent` is a variable holding an arbitrary HTML string -->
<template @raw="myHtmlContent" webc:nokeep></template>
```

* Using `webc:raw` will prevent processing the result as WebC {% addedin "@11ty/webc@0.6.0" %}
* Use `@raw` as an alias for `webc:raw @html` {% addedin "@11ty/webc@0.7.1" %}

### `@raw`

{% addedin "@11ty/webc@0.7.1" %}

As noted in [`@html`](#@html), you can use `@raw` as an alias for `webc:raw @html`.

### `@text`

{% addedin "@11ty/webc@0.6.0" %}

We provide a special `@text` [prop](#props-(properties)) to override any tag content with custom JavaScript. The entire value returned here will be escaped!

```html
<p @text="dataProperty"></p>

<!-- When dataProperty contains `<p>This is text</p>`, this renders: -->
<p>&lt;p&gt;This is text&lt;/p&gt;</p>
```

```html
<!-- webc:nokeep will replace the outer element -->
<p @text="dataProperty" webc:nokeep></p>
```

* Content returned from the `@text` prop will **not** be processed as WebC.

### `webc:is`

Remap a component to another component name.

```html
<div webc:is="my-component"></div>

<!-- equivalent to -->
<my-component></my-component>
```

### `webc:scoped`

We include a lightweight mechanism (`webc:scoped`) to scope component CSS. Selectors are prefixed with a new component class name. The class name is based on a hash of the style content (for fancy de-duplication of identical component styles).

<details>
<summary>Expand for example</summary>

{% codetitle "page.webc" %}

```html
<my-component>Default slot</my-component>
```

If you use `:host` it will be replaced with that class selector.

{% codetitle "components/my-component.webc" %}

```html
<style webc:scoped>
:host {
	color: blue;
}
:host:defined {
	color: rebeccapurple;
}
</style>
```

This outputs:

```html
<my-component class="wcl2xedjk">Default slot</my-component>
```

and aggregates the following CSS to [the bundle](#asset-bundling):

```css
.wcl2xedjk{color:blue}
.wcl2xedjk:defined{color:rebeccapurple}
```

</details>

### `webc:type`

_Adding your own [Custom Transform](https://github.com/11ty/webc#custom-transforms) **directly** to WebC is not yet available in the Eleventy WebC plugin! If this is something folks would like to see added, [please let us know](https://fosstodon.org/@eleventy)! Do note that you **can** [add your own custom template engine](/docs/languages/custom/) to the render plugin!_

### `webc:type="11ty"`

The [Custom Transforms feature](https://github.com/11ty/webc#custom-transforms) (e.g. `webc:type`) in the Eleventy WebC plugin has been wired up to the [Eleventy Render plugin](/docs/plugins/render/) to allow you to use existing Eleventy template syntax inside of WebC.

{% callout "info", "md" %}Note that the `webc:type="11ty"` feature is exclusive to the **Eleventy** WebC plugin and is not available in non-Eleventy independent WebC.{% endcallout %}

Use `webc:type="11ty"` with the `11ty:type` attribute to specify a [valid template syntax](/docs/plugins/render/#rendertemplate).

{% codetitle "my-page.webc" %}

{% raw %}
```liquid
---
frontmatterdata: "Hello from Front Matter"
---
<template webc:type="11ty" 11ty:type="liquid,md">
{% assign t = "Liquid in WebC" %}
## {{ t }}

_{{ frontmatterdata }}_
</template>
```
{% endraw %}

* You have full access to the data cascade here (note `frontmatterdata` is [set in front matter](#front-matter) above).
* Content returned from custom transforms on `<template>` (or `webc:is="template"`) nodes will be processed as WebC—return any WebC content here! {% addedin "@11ty/webc@0.5.0" %}

### `webc:type` (JavaScript Render Functions)

You can also transform individual element content using `webc:type`. In addition to `webc:type="11ty"` above, there are three more bundled types:

* `webc:type="js"` {% addedin "@11ty/webc@0.7.1" %} which supercedes `webc:type="render"`
* `webc:type="css:scoped"` (internal for `webc:scoped`—but overridable!)

JavaScript Render Functions are async friendly (e.g. `async function()`):

#### `webc:type="js"`

{% addedin "@11ty/webc@0.7.1" %} Run any arbitrary server JavaScript in WebC. Outputs the result of the very last statement executed in the script. Async-friendly (return a promise and we’ll resolve it).

{% codetitle "page.webc" %}

```html
<img src="my-image.jpeg" alt="An excited Zach is trying to finish this documentation">
```

{% codetitle "components/img.webc" %}


```html
<script webc:type="js" webc:root>
if(!alt) {
	throw new Error("oh no you didn’t");
}
`<img src="${src}" alt="${alt}">`;
</script>
```

<details>
<summary>Expand to see this example with <code>webc:type="render"</code></summary>

```html
<script webc:type="render">
	function() {
		if(!this.alt) {
			throw new Error("oh no you didn’t");
	}
	// Free idea: use the Eleventy Image plugin to return optimized markup
	return `<img src="${this.src}" alt="${this.alt}">`;
}
</script>
```

</details>

Or use a JavaScript render function to generate some CSS:

{% codetitle "page.webc" %}

```html
<style webc:is="add-banner-to-css" @license="MIT licensed">
p { color: rebeccapurple; }
</style>
```

{% codetitle "components/add-banner-to-css.webc" %}

```html
<template webc:is="style" webc:root webc:keep>
	<script webc:type="js">`/* ${license} */`</script>
	<slot></slot>
</template>
```

<details>
<summary>Expand to see this example with <code>webc:type="render"</code></summary>

```html
<template webc:is="style" webc:root webc:keep>
	<script webc:type="render">
	function() {
		return `/* ${this.license} */`;
	}
	</script>
	<slot></slot>
</template>
```

</details>

Bonus tips:

* You can pair `webc:type="js"` (or `webc:type="render"`) with [`webc:scoped`](#webcscoped)!
* You do have access to the component attributes and props in the render function (which is covered in another section!).
* Content returned from render functions of `<template>` (or `webc:is="template"`) nodes will be processed as WebC—return any WebC content here! {% addedin "@11ty/webc@0.5.0" %}

Here’s another example of a more complex conditional (you can also use `webc:if`!):

```html
<script webc:type="js">
if(alt) {
	`<img src="${src}" alt="${alt}">`
} else {
	`<a href="${src}">Your image didn’t have an alt so you get this link instead.</a>`
}
</script>
```

### `webc:raw`

Use `webc:raw` to opt-out of WebC template processing for all child content of the current node. Notably, attributes on the current node will be processed. This works well with `<template>`!

{% codetitle "components/my-component.webc" %}

```html
<template webc:raw>
Leave me out of this.
<style>
p { color: rebeccapurple; }
</style>
</template>
```

* Related: [`@raw` property](#@raw)

### Helper Functions

WebC [Helpers](https://github.com/11ty/webc#helper-functions) are JavaScript functions available in dynamic attributes, `@html`, `@raw`, and render functions.

#### Eleventy-provided Helpers

{% addedin "@11ty/eleventy-plugin-webc@0.5.0" %}Included with Eleventy WebC, [JavaScript template functions](/docs/languages/javascript/#javascript-template-functions) and [Universal Filters](/docs/filters/#universal-filters) are provided automatically as WebC Helpers.

This includes [`url`, `slugify`, `log` and others](/docs/filters/#eleventy-provided-universal-filters)!

```html
<!-- Use the  Eleventy provided `url` universal filter -->
<a :href="url('/local-path/')">My Link</a>
```

#### Supply your own Helper

{% codetitle ".eleventy.js" %}

```js
module.exports = function(eleventyConfig) {
	// via Universal Filter
	eleventyConfig.addFilter("alwaysRed", () => "Red");

	// or via JavaScript Template Function directly
	eleventyConfig.addJavaScriptFunction("alwaysBlue", () => "Blue");

	// Don’t forget to add the WebC plugin in your config file too!
};
```

```html
<div @html="alwaysRed()"></div>
<div @html="alwaysBlue()"></div>

<!-- renders as: -->
<div>Red</div>
<div>Blue</div>
```


### Subtleties and Limitations

#### Void elements

Custom elements (per specification) are not supported as void elements: they require both a starting and ending tag. You can workaround this limitation using [`webc:is`](#webcis).

#### `<head>` Components

There are a few wrinkles when using an HTML parser with custom elements. Notably, the parser tries to force custom element children in the `<head>` over to the `<body>`. To workaround this limitation, use [`webc:is`](#webcis).

<details>
<summary>Expand for a few example workarounds</summary>

```html
<head webc:is="my-custom-head">
	<!-- this is slot content, yes you can use named slots here too -->
</head>
```

```html
<head>
	<!-- <my-custom-head> is not allowed here -->
	<meta webc:is="my-custom-head">
	<title webc:is="my-custom-title">Default Title</title>
</head>
```

</details>

#### Rendering Modes

There are two different rendering modes in Eleventy: `page` and `component`. We attempt to guess the rendering mode that you’d like based on the markup you supply. The `page` rendering mode is for rendering full HTML pages. The `component` rendering mode is for fragments of HTML. Most of the time you won’t need to worry about this distinction but it is included in the documentation for completeness.

* `page` is used when the markup starts with `<!doctype` (or `<!DOCTYPE`) or `<html` (WebC forces no-quirks parsing).
* `component` is used otherwise.

## Eleventy + WebC Features

### Front Matter

WebC in Eleventy works automatically with standard Eleventy conventions for [front matter](/docs/data-frontmatter/) (though front matter in Eleventy is _optional_).

{% codetitle "with-front-matter.webc" %}

```yaml
---
layout: "my-layout.webc"
---
WebC *is* HTML.
```

<details>
<summary>Expand to see an example <code>my-layout.webc</code></summary>

The above example assumes the existence of `_includes/my-layout.webc` (an [Eleventy layout](/docs/layouts/)).

{% codetitle "_includes/my-layout.webc" %}

```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8">
		<title>WebC Example</title>
	</head>
	<body @raw="content"></body>
</html>
```

* Read more about the WebC properties: [`@raw`](#@raw) {% addedin "@11ty/webc@0.7.1" %} and [`@html`](#@html).
* {% addedin "@11ty/webc@0.5.0" %}`this.` is no longer required in `@html` or `@raw` (e.g. `this.content`) when referencing helpers/data/attributes/property values.

</details>

_Notable note_: front matter (per standard Eleventy conventions) is supported in page-level templates only (`.webc` files in your input directory) and not in components (see below).

### Defining Components

Components are the {% emoji "✨" %}magic{% emoji "✨" %} of WebC and there are a few ways to define components in WebC:

1. Use global no-import components specified in your config file.
1. Specify a glob of no-import components at a directory or template level in the data cascade.
1. You can use [`webc:import`](#webcimport) inside of your components to import another component directly.

{% callout "info" %}
Notably, WebC components can have any valid HTML tag name and are not restricted to the same naming limitations as custom elements (they do not require a dash in the name).
{% endcallout %}

#### Global no-import Components

Use the `components` property in the options passed to `addPlugin` in your Eleventy configuration file to specify project-wide WebC component files available for use in any page.

```js
const pluginWebc = require("@11ty/eleventy-plugin-webc");

module.exports = function(eleventyConfig) {
	eleventyConfig.addPlugin(pluginWebc, {
		// Glob to find no-import global components
    // This path is relative to the project-root!
		// (The default changed from `false` in Eleventy WebC v0.7.0)
		components: "_components/**/*.webc",
	});
};
```

Notably, the path for `components` is relative to your project root (**not** your [project’s `input` directory](/docs/config/#input-directory)).

The file names of components found in the glob determine the global tag name used in your project (e.g. `_includes/components/my-component.webc` will give you access to `<my-component>`).

#### Declaring Components in Front Matter

You can also use and configure specific components in front matter (or, via any part of the data cascade—scoped to a folder or a template) by assigning a glob (or array of globs) to the property at `webc.components`:

{% codetitle "my-directory/my-page.webc" %}

```html
---
layout: "my-layout.webc"
webc:
  components: "./webc/*.webc"
---
<my-webc-component>WebC *is* HTML.</my-webc-component>
```

{% callout "warn", "md-block" %}By default these paths are relative to the template file. If you’re setting this in the data cascade in a directory data file that will apply multiple child folders deep, it might be better to:

1. Use the global no-import components option.
1. Use `~/` as a prefix (e.g. `~/my-directory/webc/*.webc`) to alias to the project’s root directory.
{% endcallout %}

### CSS and JS (Bundler mode)

Eleventy WebC will bundle any specific page’s assets (CSS and JS used by components on the page). These are automatically rolled up when a component uses `<script>`, `<style>`, or `<link rel="stylesheet">`. You can use this to implement component-driven Critical CSS.

{% callout "info", "md" %}**Declarative Shadow DOM** note: if a `<style>` is nested inside of [declarative shadow root](https://web.dev/declarative-shadow-dom/) template (`<template shadowroot>`), it is left as is and not aggregated.{% endcallout %}

{% codetitle "_includes/webc/my-webc-component.webc" %}

```html
<style>/* This is component CSS */</style>
<script>/* This is component JS */</script>

<!-- File references work too -->
<link rel="stylesheet" href="my-file.css">
<script src="my-file.js"></script>
```

As shown above this also includes `<link rel="stylesheet">` and `<script src>` when the URLs point to files on the file system ([remote URL sources are not yet supported](https://github.com/11ty/webc/issues/15)).

You can opt-out of bundling on a per-element basis [using `webc:keep`](#webckeep).

{% codetitle "_includes/layout.webc" %}

```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8">
		<title>WebC Example</title>
		<style @raw="getCss(page.url)" webc:keep></style>
		<script @raw="getJs(page.url)" webc:keep></script>
	</head>
	<body @raw="content"></body>
</html>
```

* {% addedin "@11ty/webc@0.8.0" %}`webc:keep` is required on `<style>` and `<script>` in your layout files to prevent re-bundling the bundles.
* {% addedin "@11ty/webc@0.8.0" %}The `getCss` and `getJs` helpers are now available to all WebC templates without restriction. Previous versions required them to be used in an _Eleventy Layout_ file.
* `@raw` was {% addedin "@11ty/webc@0.7.1" %}. Previous versions can use `webc:raw @html`.
* {% addedin "@11ty/webc@0.5.0" %}`this.` is no longer required in `@html` or `@raw` (e.g. `this.getCss`/`this.page.url`) when referencing helpers/data/attributes/property values.

{% callout "info", "md-block" %}Outside of `*.webc` files (e.g. in Nunjucks or Liquid layout files), the Eleventy WebC plugin also publishes two universal filters `webcGetCss` and `webcGetJs`, for use like this:

{% codetitle "_includes/layout.njk" %}

{% raw %}
```njk
<style>{{ page.url | webcGetCss | safe }}</style>
<script>{{ page.url | webcGetJs | safe }}</script>
```
{% endraw %}

{% codetitle "_includes/layout.liquid" %}

{% raw %}
```njk
<style>{{ page.url | webcGetCss }}</style>
<script>{{ page.url | webcGetJs }}</script>
```
{% endraw %}
{% endcallout %}

#### Asset bucketing

Components can use the `webc:bucket` feature to output to any arbitrary bucket name for compartmentalization at the component level.

{% codetitle "_includes/webc/my-webc-component.webc" %}

```html
<style>/* This CSS is put into the default bucket */</style>
<script>/* This JS is put into the default bucket */</script>
<style webc:bucket="defer">/* This CSS is put into the `defer` bucket */</style>
<script webc:bucket="defer">/* This JS is put into the `defer` bucket */</script>
```

{% codetitle "_includes/layout.webc" %}

```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8">
		<title>WebC Example</title>
		<!-- Default bucket -->
		<style @raw="getCss(page.url)" webc:keep></style>
		<script @raw="getJs(page.url)" webc:keep></script>
	</head>
	<body>
		<template @raw="content" webc:nokeep></template>

		<!-- `defer` bucket -->
		<style @raw="getCss(page.url, 'defer')" webc:keep></style>
		<script @raw="getJs(page.url, 'defer')" webc:keep></script>
	</body>
</html>
```

* {% addedin "@11ty/webc@0.8.0" %}`webc:keep` is required on `<style>` and `<script>` in your layout files to prevent re-bundling the bundles.
* {% addedin "@11ty/webc@0.5.0" %}`this.` is no longer required in `@html` or `@raw` (e.g. `this.getCss`/`this.page.url`) when referencing helpers/data/attributes/property values.

### Use with `is-land`

You can also use this out of the box with Eleventy’s [`is-land` component for web component hydration](/docs/plugins/partial-hydration/).

At the component level, components can declare their own is-land loading conditions.

{% codetitle "_includes/webc/my-webc-component.webc" %}

```html
<is-land on:visible>
	<template data-island>
		<!-- CSS -->
		<style webc:keep>
		/* This is on-visible CSS */
		</style>
		<link rel="stylesheet" href="some-arbitrary-css.css" webc:keep>

		<!-- JS -->
		<script type="module" webc:keep>
		console.log("This is on-visible JavaScript");
		</script>
		<script type="module" src="some-arbitrary-js.js" webc:keep></script>
	</template>
</is-land>
```

