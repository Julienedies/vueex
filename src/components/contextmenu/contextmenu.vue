<template>
    <div class="vueex-contextmenu" v-if="ev" :style="style">
        <slot>

        </slot>
    </div>
</template>

<script>
    export default {
        name: 'contextmenu',
        props: {
            value: false,
        },
        data () {
            return {
                ev: false,
                visible: false,
                style: {}
            }
        },
        updated () {
            console.log('--------- updated', this.$el )
        },
        computed: {
        },
        methods: {
            _style () {
                let obj = {}
                if (this.ev) {
                    obj = this.xy(this.ev)
                    obj.transform = 'translate(0, 0)'
                }
                return obj
            },
            xy (e) {
                let gap = 20
                let cx = e.clientX
                let cy = e.clientY
                let x = cx
                let y = cy
                let vw = window.innerWidth
                let vh = window.innerHeight
                let ew = this.$el.offsetWidth
                let eh = this.$el.offsetHeight
                //console.log(55555, cx, ew, vw, cx + ew - vw)
                //console.log(66666, cy, eh, vh, cy + eh - vh)
                if (cx + ew - vw > 0) {
                    x = cx - (cx + ew - vw) - gap;
                }
                if (cy + eh - vh > 0) {
                    y = cy - (cy + eh - vh) - gap;
                }
                return {left: `${ x }px`, top: `${ y }px`}
            },
            cancelContextmenu () {
                this.ev = false
                this.visible = false;
                this.style = {}
                this.$emit('input', false)
            },
            onBlur(){
                this.cancelContextmenu()
            }
        },
        watch: {
            value (e) {
                this.ev = e
                if (e) {
                    this.$nextTick(() => {
                        this.style = this._style()
                    })
                    document.addEventListener('scroll', this.cancelContextmenu, true)
                    document.addEventListener('click', this.cancelContextmenu, false)
                    document.addEventListener('contextmenu', this.cancelContextmenu, false)
                } else {
                    document.removeEventListener('click', this.cancelContextmenu, false)
                    document.removeEventListener('contextmenu', this.cancelContextmenu, false)
                    document.addEventListener('scroll', this.cancelContextmenu, true)
                }
            }
        }
    }
</script>

<style lang="scss" scoped>
    .vueex-contextmenu {
        display: flex;
        transform: translate(-10000px, 0);
        padding: 0.5rem 0;
        position: fixed;
        z-index: 10001;
        top: 0;
        left: 0;
        background: #fff;
        box-shadow: #9f9f9f 3px 3px 10px;
        border-radius: 0.25rem;
        min-height: 75px;
        min-width: 3em;
    }
</style>