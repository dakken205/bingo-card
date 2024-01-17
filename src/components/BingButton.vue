<template>
    <q-btn :class="isOpen ? 'bg-grey-3' : 'bg-white'" @click="this.isOpen = !this.isOpen" flat>{{
        isOpen }}</q-btn>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
    name: 'BingButton',

    setup() {
        return {
            isOpen: false,
            value: "sample"
        }
    },
    created() {
    },
    methods: {
    },
    watch: {
    }
})
</script>