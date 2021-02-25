<template>
    <dropdown-menu v-model="showMenu">
        <button @click="showMenu = true">Show menu</button>

        <template #dropdown>
            <div>1</div>
            <div>2</div>
            <div>3</div>
        </template>
    </dropdown-menu>
</template>

<script setup>
    import { ref } from 'vue'
    import DropdownMenu from './components/DropdownMenu.vue'
    const showMenu = ref(false)
</script>