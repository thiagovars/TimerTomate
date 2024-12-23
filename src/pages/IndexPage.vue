<template>
    <q-page class="flex flex-center bg-dark">
        <div class="text-center">
            <timer :minutes="35" />
        </div>
    </q-page>
</template>

<script setup>
import Timer from 'src/components/TimerTomate.vue'
import { defineComponent } from 'vue'

defineComponent({
    name: 'IndexPage',
    components: {
        Timer,
    },
})
</script>
