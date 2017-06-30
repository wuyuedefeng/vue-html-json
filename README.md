# vue-html-json

```
$ npm install vue-html-json -S
```

global:

```
import VueHtmlJson from 'vue-html-json'
Vue.component(VueHtmlJson.name, VueHtmlJson)
```

partial:

```
import VueHtmlJson from 'vue-html-json'

export default {
    data () {},
    mounted () {},
    components: {
        [VueHtmlJson.name]: VueHtmlJson
    }
}
```

html:

```
<html-json :jsonObj="your json"></html-json>
```


