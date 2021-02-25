<template>
    <div
        ref="activatorContainer"
        class="albus-dropdown-menu-activator">

        <slot></slot>
    </div>

    <teleport to="body">
        <div
            ref="menu"
            :class="{ 'albus-dropdown-visible': modelValue }"
            class="albus-dropdown-menu"
            :style="{ 'left': left + 'px', top: top + 'px', 'min-width': width + 'px' }">

            <slot name="dropdown"></slot>
        </div>
    </teleport>
</template>

<script>
    import { onMounted, ref, toRefs, nextTick } from 'vue'

    export default {
        props: {
            modelValue: {
                type: Boolean,
                default: false,
            },
        },

        setup(props, { emit })
        {
            let { modelValue } = toRefs(props)

            const top = ref(0)
            const left = ref(0)
            const menu = ref('')
            const height = ref(0)
            const width = ref('auto')
            const activatorElement = ref('')
            const activatorContainer = ref('')

            function closeMenu() {
                emit('update:modelValue', false)
            }

            function calculatePosition() {
                activatorElement.value = activatorContainer.value.childNodes[1]

                nextTick(() => {
                    top.value = activatorElement.value.offsetTop
                    left.value = activatorElement.value.offsetLeft
                    top.value += activatorElement.value.offsetHeight
                    left.value = left.value + ((activatorElement.value.offsetWidth - menu.value.offsetWidth) / 2)
                })
            }

            function hideMenu() {
                top.value = -999
                left.value = -999
            }

            onMounted(() => {
                calculatePosition()
            })

            return {
                activatorContainer, top, left, width, height, menu
            }
        }
    }
</script>

<style>
    .albus-dropdown-menu {
        position: absolute;
        display: inline-block;
        overflow-y: auto;
        overflow-x: hidden;
        contain: content;
        will-change: transform;
        visibility: hidden;
        transition: opacity .2s;
        opacity: 0;
        z-index: 99999;
        border-radius: 8px;
        box-shadow: 0 5px 5px -3px rgba(0,0,0,.1), 0 8px 10px 1px rgba(0,0,0,.1), 0 3px 14px 2px rgba(0,0,0,.1);
        padding: 13px 20px;
    }

    .albus-dropdown-visible {
        visibility: visible;
        opacity: 1;
    }
</style>