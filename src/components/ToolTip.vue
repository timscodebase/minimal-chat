<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import tippy from 'tippy.js';
import 'tippy.js/animations/shift-away-subtle.css';

const props = defineProps({
    targetId: String
});

const tooltipElement = ref(null);
let tippyInstance = null;

const createTooltip = () => {
    const target = document.getElementById(props.targetId);

    if (target && tooltipElement.value) {
        tippyInstance = tippy(target, {
            content: tooltipElement.value,
            placement: 'top',
            trigger: 'mouseenter focus',
            appendTo: document.body,
            arrow: true,
            animation: 'shift-away-subtle',
            interactive: true,
            onHidden: (instance) => {
                instance.destroy();
            }
        });
    }
};

onMounted(() => {
    createTooltip();
});

onUnmounted(() => {
    if (tippyInstance) {
        tippyInstance.destroy();
    }
});
</script>

<template>
    <div ref="tooltipElement" class="tooltip-container">
        <slot></slot>
    </div>
</template>

<style>
.tooltip-container {
    padding: 8px;
    background-color: #333;
    color: white;
    border-radius: 4px;
    font-size: 14px;
    z-index: 1000001;
    /* Ensure it's on top */
    pointer-events: none;
    /* Prevents tooltip from blocking mouse events */
}
</style>
