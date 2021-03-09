<template>
  <div>
    <form>
      <template v-for="field in theForm" :key="field.id">
        {{field.id}}
        <div :class="{ error: field.vuelidate.$invalid }">
          <multiselect v-if="field.is === 'Multiselect'"
            v-model="field.model"
            :options="field.options"
            @close="validateField(v$.fields.manyThings)"
            mode="tags"
            valueProp="id"
            label="email"
            :searchable="true"
            :createTag="true"
          ></multiselect>
          <component :is="field.is" v-else v-model="field.vuelidate.$model"></component>
          <div class="input-errors" v-for="(error, index) of field.vuelidate.$errors" :key="index">
            <div class="error-msg">{{ error.$message }}</div>
          </div>
        </div>
      </template>
      <!-- <div :class="{ error: v$.name.$invalid }">
        Valid? {{v$.name.$invalid}}
        <input v-model="v$.name.$model">
          <div class="input-errors" v-for="(error, index) of v$.name.$errors" :key="index">
            <div class="error-msg">{{ error.$message }}</div>
          </div>
      </div>
      <div :class="{ error: v$.fields.regionContacts.$invalid }">
        <input v-model="v$.fields.regionContacts.$model">
          <div class="input-errors" v-for="(error, index) of v$.fields.regionContacts.$errors" :key="index">
            <div class="error-msg">{{ error.$message }}</div>
          </div>
      </div>
      <div :class="{ error: v$.fields.manyThings.$invalid }">
        <multiselect
          v-model="inputs.fields.manyThings"
          :options="options"
          @close="validateField(v$.fields.manyThings)"
          mode="tags"
          valueProp="id"
          label="email"
          :searchable="true"
          :createTag="true"
        ></multiselect>
        <div class="input-errors" v-for="(error, index) of v$.fields.manyThings.$errors" :key="index">
          <div class="error-msg">{{ error.$message }}</div>
        </div>
      </div> -->
    </form>
  </div>
</template>

<script>
import { reactive, ref, onMounted } from 'vue' // "from '@vue/composition-api'" if you are using Vue 2.x
import useVuelidate from '@vuelidate/core'
import { required, minLength } from '@vuelidate/validators'
import Multiselect from '@vueform/multiselect'

export default {
  name: 'TestForm',
  components: {
    Multiselect
  },
  setup () {
    const inputs = reactive({
      name: 'foo',
      fields: {
        regionContacts: 'bob',
        manyThings: []
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
        },
        manyThings: {
          required
        }
      }
    }

    const v$ = useVuelidate(rules, inputs)

    const theForm = ref([])

    onMounted(() => {
      theForm.value = [
        {
          id: 'name',
          vuelidate: v$.name,
          is: 'input'
        },
        {
          id: 'regionContacts',
          vuelidate: v$.fields?.regionContacts,
          is: 'input'
        },
        {
          id: 'manyThings',
          vuelidate: v$.fields?.manyThings,
          model: inputs.fields.manyThings,
          is: 'Multiselect',
          options: [
            { id: 'guid0', last: 'Last 0', first: 'First 0', email: 'fakeEmail0@someState.gov' },
            { id: 'guid1', last: 'Last 1', first: 'First 1', email: 'fakeEmail1@someState.gov' },
            { id: 'guid2', last: 'Last 2', first: 'First 2', email: 'fakeEmail2@someState.gov' },
            { id: 'guid3', last: 'Last 3', first: 'First 3', email: 'fakeEmail3@someState.gov' },
            { id: 'guid4', last: 'Last 4', first: 'First 4', email: 'fakeEmail4@someState.com' },
            { id: 'guid5', last: 'Last 5', first: 'First 5', email: 'fakeEmail5@someState.gov' },
            { id: 'guid6', last: 'Last 6', first: 'First 6', email: 'fakeEmail6@someState.com' },
            { id: 'guid7', last: 'Last 7', first: 'First 7', email: 'fakeEmail7@someState.com' }
          ]
        }
      ]
    })

    return {
      inputs,
      requiredNameLength,
      v$,
      theForm,
      inputted: () => {
        console.log('HERE THOUGHasdasd', v$)
      },
      validateField: (ref) => {
        if (ref && ref.$touch) {
          ref.$touch()
        }
      }
    }
  }
}
</script>

<style src="@vueform/multiselect/themes/default.css"></style>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .error {
    input {
      border-color: red;
    }
  }
</style>
