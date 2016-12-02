# Snep
**A lightbox for cats**

##Installation:
`npm install snep`

then


```javascript
var Vue  = require('vue');
var Snep = require('snep');

var vm = new Vue({
    components: { Snep }
});
```

then

```html
...
<snep image="large-image.jpg">
    <img src="thumbnail.jpg"/>
</snep>
...
```

and you're snepping.