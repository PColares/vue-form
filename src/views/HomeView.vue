<script setup>
import BaseInput from '../components/BaseInput.vue'
import { ref, reactive } from 'vue'
import { computed } from '@vue/reactivity'
import useVuelidate from '@vuelidate/core'
import {
  numeric,
  between,
  required,
  minLength,
  email,
  sameAs,
  helpers
} from '@vuelidate/validators'

// custom validations
// const containsUser = (value) => {
//   return value.includes('_')
// }

const formData = reactive({
  age: '',
  username: '',
  email: '',
  password: '',
  confirmPassword: ''
})

const rules = computed(() => {
  return {
    age: {
      required: helpers.withMessage('Campo obrigatório', required),
      numeric: helpers.withMessage('Campo deve ser numérico', numeric),
      between: helpers.withMessage('Deve ser entre 1 e 99', between(1, 99))
    },
    // custom validation messages with helpers.message
    username: {
      required: helpers.withMessage('Campo obrigatório', required),
      minLength: helpers.withMessage('Mínimo de 5 caracteres', minLength(5))
      // containsUser: helpers.withMessage('Deve conter _', containsUser)
    },
    email: {
      required: helpers.withMessage('Campo obrigatório', required),
      email: helpers.withMessage('Campo deve conter um e-mail válido', email)
    },
    password: { required: helpers.withMessage('Campo obrigatório', required) },
    confirmPassword: {
      required: helpers.withMessage('Campo obrigatório', required),
      sameAs: helpers.withMessage('Senhas devem ser iguais', sameAs(formData.password))
    }
  }
})

const user$ = useVuelidate(rules, formData)

const submitForm = async () => {
  const result = await user$.value.$validate()
  const errors = await user$.value.$errors

  if (!result) return console.log('Error, form not submitted!', errors)

  console.log('Success, form submitted!')
}
</script>

<template>
  <main>
    <form
      class="bg-slate-400 flex flex-col w-[450px] items-center rounded text-white p-4 mt-10 mb-5 gap-8"
      @submit.prevent="submitForm"
    >
      <div class="flex flex-col relative w-full h-14">
        <input
          class="w-full rounded-lg px-2.5 pb-2.5 pt-5 text-sm text-gray-900 bg-gray-50 dark:bg-gray-700 border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer"
          placeholder=" "
          v-model="formData.age"
          @change="user$.age.$touch()"
          type="number"
        />
        <label
          for="age"
          class="absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-4 scale-75 top-4 z-10 origin-[0] left-2.5 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-4"
        >
          Idade
        </label>
        <span class="text-yellow-200" v-for="error in user$.age.$errors">{{ error.$message }}</span>
      </div>

      <div class="w-full h-14">
        <BaseInput label="Usuário" v-model="formData.username" type="text" />
        <span class="text-yellow-200" v-for="error in user$.username.$errors">{{ error.$message }}</span>
      </div>

      <div class="w-full h-14">
        <BaseInput label="E-mail" v-model="formData.email" type="email" />
        <span class="text-yellow-200" v-for="error in user$.email.$errors" :key="error.$uid">{{ error.$message }}</span>
      </div>
      <div class="w-full h-14">
        <BaseInput label="Senha" v-model="formData.password" type="password" />
        <span class="text-yellow-200" v-for="error in user$.password.$errors" :key="error.$uid">{{ error.$message }}</span>
      </div>
      <div class="w-full h-14">
        <BaseInput label="Confirmar Senha" v-model="formData.confirmPassword" type="password" />
        <span class="text-yellow-200" v-for="error in user$.confirmPassword.$errors" :key="error.$uid">{{error.$message}}</span>
      </div>

      <button
        class="border-5 bg-slate-800 rounded p-3 cursor-pointer transition-all hover:bg-slate-600 self-end mr-10"
        type="submit"
      >
        Criar usuário
      </button>
    </form>

    <!-- grouping errors -->
    <!-- <div>
      <p>Errors:</p>
      <span v-for="error in v$.$errors" :key="error.$uid">
        {{ error.$property }} - {{ error.$message }}
      </span>
    </div> -->
  </main>
  <pre
    class="max-h-[500px] border-emerald-500 border-8 text-left text-white bg-slate-500 scroll-m-2 rounded overflow-auto"
    >{{ user$ }}</pre
  >
</template>
