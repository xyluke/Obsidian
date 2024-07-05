~~~js
<pre v-highlight>
   <code class="xml">
		{{ xmlContent }}
   </code>
</pre>

export default {
	data() {
		reutnr {}
	},
    // 自定义指令
    directives: {
        highlight: (el) => {
            let blocks = el.querySelectorAll('pre code');
            blocks.forEach((block) => {
                Hljs.highlightBlock(block)
            })
        }
    },
}
~~~

多次重复渲染导致了这个文档卡死