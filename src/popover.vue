<template>
    <div class="popover" @click="clickButton">
        <div class="content-wrapper" v-if="visible" ref='contentWrapper'>
            <slot name="content"></slot>
        </div>
        <span ref="triggerWrapper">
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
                if (this.$refs.triggerWrapper.contains(e.target)) {
                    this.visible = !this.visible
                    if (this.visible) {
                        console.log('click button')
                        this.$nextTick(() => {
                            document.body.appendChild(this.$refs.contentWrapper)
                            let { width, height, top, left } = this.$refs.triggerWrapper.getBoundingClientRect()
                            this.$refs.contentWrapper.style.left = left + window.scrollX + 'px'
                            this.$refs.contentWrapper.style.top = top + window.scrollY + 'px'
                            document.addEventListener('click', this.clickOther)
                        })
                    } else {
                        document.removeEventListener('click', this.clickOther)
                    }
                }
            },
            clickOther(e) {
                if (!this.$refs.contentWrapper.contains(e.target)) {
                    this.visible = !this.visible
                    console.log('doc click');
                    document.removeEventListener('click', this.clickOther)
                }
            }
        }
    }
</script>
<style lang='scss' scoped>
    .popover {
        display: inline-block;
        vertical-align: top;
        position: relative;
    }
    .content-wrapper {
        position: absolute;
        border: 1px solid red;
        box-shadow: 0 0 3px rgba(0, 0, 0, 0.5);
        transform: translateY(-100%);
    }
</style>