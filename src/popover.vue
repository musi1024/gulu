<template>
    <div class="popover" @click="clickButton">
        <div class="content-warpper" v-if="visible">
            <slot name="content"></slot>
        </div>
        <span class="button-warpper" ref="button">
            <slot></slot>
        </span>
    </div>
</template>

<script>
    export default {
        name: "GuluPopover",
        data() {
            return {
                visible: false
            }
        },
        methods: {
            clickButton(e) {
                if (this.$refs.button.contains(e.target)) {
                    this.visible = !this.visible 
                    if (this.visible) {       
                        console.log('click button')
                        this.$nextTick(() => {
                            document.addEventListener('click', this.clickOther)
                        }) 
                    } else {
                        document.removeEventListener('click', this.clickOther)
                    }   
                }
            },
            clickOther() {
                this.visible = !this.visible
                console.log('doc click');   
                document.removeEventListener('click', this.clickOther)
            }
        }
    }
</script>
<style lang='scss' scoped>
    .popover {
        display: inline-block;
        vertical-align: top;
        position: relative;

        .content-wrapper {
            position: absolute;
            border: 1px solid red;
            box-shadow: 0 0 3px rgba(0, 0, 0, 0.5);
            transform: translateY(-100%);
        }
    }
</style>