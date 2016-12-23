# Snep
**A lightbox for cats**

##Installation:
### You'll need:
1. Webpack or similar bundling [Vue Template components.](https://github.com/vuejs/vue-loader)
2. [node-sass](https://github.com/sass/node-sass) built into your webpack configuration. 
3. `npm install --save snep` or `yarn add snep`

Then, simply import the Snep component using the `components` option in a Vue instance/component, like below:

```javascript
var Vue  = require('vue');
var Snep = require('snep');

var vm = new Vue({
	el: ".wrapper",
    components: { Snep }
});
```

you can then use the component on your site using the `<snep>` tag:

```html
...
<snep image="large-image.jpg">
    <img src="thumbnail.jpg"/>
</snep>
...
```

if you'd like to remove the controls for rotation and scaling, just add the `plain` directive:

```html
...
<snep plain image="large-image.jpg">
    <img src="thumbnail.jpg"/>
</snep>
...
```