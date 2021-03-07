<template>
  <div class="home">
    HERERE
    <TestForm></TestForm>
  </div>
</template>

<script>
import TestForm from '@/components/TestForm.vue'

export default {
  name: 'Home',
  components: {
    TestForm
  },
  setup () {
    console.log('Got here!!!')
  }
}
</script>
