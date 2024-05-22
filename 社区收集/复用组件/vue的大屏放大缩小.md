~~~vue
<template>
    <div class="scale-box" :style="{height: parentHeight, width: parentWidth}">
        <slot :scale="scale"></slot>
    </div>
</template>

<script>
import { debounce } from "@/ux/util.js"
export default {
    name: "VscaleScrean",

    props: {
        width: {
            type: Number,
            default: 1920
        },
        height: {
            type: Number,
            default: 1080
        },
        parentSize: {
            type: Object,
            default:()=>{
                return {
                    width: 1920,
                    height:1080
                }
            }
        }
    },
    watch: {
        parentSize: {
            handler(nv, ol) {
                this.parentHeight = nv.height;
                this.parentWidth = nv.width;
                this.setScale();
            },
            immediate: true,
            deep: true
        }
    },

    mounted() {
        window.addEventListener('resize', this.setScale)
        setTimeout(()=>{
            this.setScale();
        },0)
    },

    data() {
        return {
            scale: 1
        }
    },

    methods: {
        getScale() {
            let ww = this.parentWidth / this.width;
            let wh = this.parentHeight/ this.height;
            return {ww, wh}
        },

        setScale() {
            debounce(() => {
                let scale = this.getScale();
                this.scale = scale;
                this.$emit('onChageScale', scale);
            }, 200)
        }
    }
}
</script>

<style scoped>
.scale-box {
    /* transform-origin: 0 0; */
    top: 0;
    left: 0;
    position: absolute;
    transition: 0.3s;
    /* overflow: hidden; */
    /* overflow-y: overlay; */
}
</style>


<!-- js
// 防抖
let timer;
export function debounce(fun, delay) {
    if(timer) {
        clearTimeout(timer);
        timer = null;
    }

    timer = setTimeout( () => {
        fun();
    }, delay)
}
-->
~~~