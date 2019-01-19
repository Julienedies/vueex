<template>
    <div class="contextmenu" v-if="">
        <slot>

        </slot>
    </div>
</template>

<script>
    export default {
        name: 'contextmenu',
        props:{
            value: false,
        },
        data(){
            return {
                visible: false
            }
        },
        mounted(){
            document.addEventListener()
        }
    }
</script>

<style lang="scss" scoped>
    .contextmenu{
        position: fixed;
        top:0;
        left:0;
        background: #fff;
        box-shadow: #d7d7d7 4px 4px 5px;
    }
</style>