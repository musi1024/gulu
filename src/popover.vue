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
            setContentPosition() {
                document.body.appendChild(this.$refs.contentWrapper)
                let {
                    width,
                    height,
                    top,
                    left
                } = this.$refs.triggerWrapper.getBoundingClientRect()
                this.$refs.contentWrapper.style.left = left + window.scrollX + 'px'
                this.$refs.contentWrapper.style.top = top + window.scrollY + 'px'
            },
            clickButton(e) {
                if (this.$refs.triggerWrapper.contains(e.target)) { 
                    this.visible = !this.visible               
                    if (this.visible) {        
                        this.openPopover()        
                    } else {
                        this.closePopover(e)  
                    }
                }
            },
            openPopover() {
                this.$nextTick(() => {
                    this.setContentPosition()
                    document.addEventListener('click', this.closePopover)
                })
            },
            closePopover(e) {
                if (!this.$refs.contentWrapper.contains(e.target)) {
                    this.visible = false
                    document.removeEventListener('click', this.closePopover)
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