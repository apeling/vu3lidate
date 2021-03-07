<template>
  <div>
    <form>
      <div :class="{ error: v$.name.$errors.length }">
        Valid? {{v$.name.$invalid}}
        <input v-model="v$.name.$model" @change="inputted">
          <div class="input-errors" v-for="(error, index) of v$.name.$errors" :key="index">
            <div class="error-msg">{{ error.$message }}</div>
          </div>
      </div>
      <div :class="{ error: v$.fields.regionContacts.$errors.length }">
        <input v-model="v$.fields.regionContacts.$model" @change="inputted">
          <div class="input-errors" v-for="(error, index) of v$.fields.regionContacts.$errors" :key="index">
            <div class="error-msg">{{ error.$message }}</div>
          </div>
      </div>
    </form>
  </div>
</template>

<script>
import { reactive, ref, onMounted } from 'vue' // "from '@vue/composition-api'" if you are using Vue 2.x
import useVuelidate from '@vuelidate/core'
import { required, minLength } from '@vuelidate/validators'

export default {
  name: 'TestForm',
  setup () {
    const inputs = reactive({
      name: 'foo',
      fields: {
        regionContacts: 'bob'
      }
    })
    const requiredNameLength = ref(2)
    const requiredOtherLength = ref(5)
    const rules = {
      name: {
        required,
        minLength: minLength(requiredNameLength.value)
      },
      fields: {
        regionContacts: {
          minLength: minLength(requiredOtherLength.value)
        }
      }
    }

    const v$ = useVuelidate(rules, inputs)

    onMounted(() => {
      console.log('v$.name', v$.name)
      console.log('v$.fields', v$.fields)
    })

    return {
      inputs,
      requiredNameLength,
      v$,
      inputted: () => {
        console.log('HERE THOUGHasdasd', v$)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .error {
    input {
      border-color: red;
    }
  }
</style>
