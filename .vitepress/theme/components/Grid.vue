<script setup lang="ts">
import { computed } from "vue"

const props = withDefaults(defineProps<{
    gap?: number
}>(), {
    gap: 8,
})

const containerStyle = computed(() => ({
    gap: `${props.gap}px`,
}))
</script>

<template>
    <div class="image-grid" :style="containerStyle">
        <slot />
    </div>
</template>

<style scoped>
.image-grid {
    display: flex;
    flex-wrap: wrap;
    align-items: flex-start;
    margin: 16px 0;
}

.image-grid::after {
    content: "";
    flex-grow: 999999999;
}
</style>
