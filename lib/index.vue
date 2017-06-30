<template>
	<pre id="vue_html_json" v-html="cJson"></pre>
</template>

<script>
	export default {
		name: 'PreJson',
		props: {
			jsonObj: {
				type: Object,
				required: true
			},
			coloured: {
				type: Boolean,
				default: true
			},
			keyValueHandler: {
				type: Function
			},
			whitespace: {
				type: Number,
				default: 2
			}
		},
		computed: {
			cJson () {
				let jsonString = JSON.stringify(this.jsonObj, function replacer (key, value) {
					value = this.keyValueHandler && this.keyValueHandler(key, value) || value
					return value
				}, this.whitespace)
				if (!this.coloured) {
					return jsonString
				}
				return this.syntaxHighlight(jsonString)
			}
		},
		methods: {
			syntaxHighlight (jsonString) {
				let json = jsonString.replace(/&/g, '&').replace(/</g, '<').replace(/>/g, '>')
				return json.replace(/("(\\u[a-zA-Z0-9]{4}|\\[^u]|[^\\"])*"(\s*:)?|\b(true|false|null)\b|-?\d+(?:\.\d*)?(?:[eE][+\\-]?\d+)?)/g, function (match) {
					var cls = 'pre-number'
					if (/^"/.test(match)) {
						if (/:$/.test(match)) {
							cls = 'pre-key'
						} else {
							cls = 'pre-string'
						}
					} else if (/true|false/.test(match)) {
						cls = 'pre-boolean'
					} else if (/null/.test(match)) {
						cls = 'pre-null'
					}
					return '<span class="' + cls + '">' + match + '</span>'
				})
			}
		}
	}
</script>

<style lang="css">
	#vue_html_json {
		outline: 1px solid #ccc; padding: 5px; margin: 5px; white-space: pre-wrap;
	}
	#vue_html_json .pre-string { color: green; }
	#vue_html_json .pre-number { color: darkorange; }
	#vue_html_json .pre-boolean { color: blue; }
	#vue_html_json .pre-null { color: magenta; }
	#vue_html_json .pre-key { color: red; }
</style>
