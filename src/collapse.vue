<template>
    <div class="collapse">
        <slot></slot>
    </div>
</template>
<script>
    import Vue from 'vue'
    export default {
        name: "GuluCollapse",
        data() {
            return {
                eventBus: new Vue()
            }
        },
        provide() {
            return {
                eventBus: this.eventBus
            }
        },
        props: {
            single: {
                type: Boolean,
                default: false
            },
            selected: {
                type: String
            }
        },
        mounted() {
            this.eventBus.$emit('update:selected', this.selected)
            console.log(this.selected)
            this.eventBus.$on('update:selected', (name) => {
                this.$emit('update:selected', name)
            })
        }
    }
</script>
<style scoped lang="scss">
    $grey: #ddd;
    $border-radius: 4px;

    .collapse {
        border: 1px solid $grey;
        border-radius: $border-radius;
    }
</style>