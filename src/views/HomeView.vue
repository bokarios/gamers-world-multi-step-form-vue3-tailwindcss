<script setup lang="ts">
import MultiFormComponent from '@/components/MultiFormComponent.vue'
import MultiFormSmallComponent from '@/components/MultiFormSmallComponent.vue'
import { onMounted, ref } from 'vue'

const small = ref(false)
onMounted(() => {
  window.addEventListener('resize', () => {
    let width = window.innerWidth

    if (width <= 425) {
      small.value = true
    } else {
      small.value = false
    }
  })
})
</script>

<template>
  <component
    :is="small ? MultiFormSmallComponent : MultiFormComponent"
  ></component>
</template>
