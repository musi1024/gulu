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
    $border-color: #333;
    $border-radius: 4px;

    .popover {
        display: inline-block;
        vertical-align: top;
        position: relative;
    }

    .content-wrapper {
        position: absolute;
        border: 1px solid $border-color;
        border-radius: $border-radius;
        filter: drop-shadow(0 1px 1px rgba(0, 0, 0, 0.5));
        background: white;
        transform: translateY(-100%);
        margin-top: -15px;
        padding: .5em 1em;
        max-width: 20em;
        word-break: break-all;

        &::before,
        &::after {
            content: '';
            display: block;
            border: 10px solid transparent;
            width: 0;
            height: 0;
            position: absolute;
            left: 10px;
        }

        &::before {
            border-top-color: black;
            top: 100%;
        }

        &::after {
            border-top-color: white;
            top: calc(100% - 1px);
        }
    }
</style>