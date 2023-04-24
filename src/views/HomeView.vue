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
      minLength: helpers.withMessage('Mínimo de 5 caracteres', minLength(5)),
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

  if (!result) return console.log('error, form not submitted !')

  console.log('Success, form submitted !')
}
</script>

<template>
  <body>
    <!-- <pre>{{ user$ }}</pre> -->
    <form @submit.prevent="submitForm">
      <div class="div-wrap">
        <label for="age">Idade</label>
        <input
          class="input-test"
          v-model="formData.age"
          @change="user$.age.$touch()"
          type="number"
        />
        <span class="error" v-for="error in user$.age.$errors">{{ error.$message }}</span>
      </div>

      <div class="div-wrap">
        <BaseInput label="Usuário" v-model="formData.username" type="text" />
        <span class="error" v-for="error in user$.username.$errors">{{ error.$message }}</span>
      </div>

      <div class="div-wrap">
        <BaseInput label="E-mail" v-model="formData.email" type="email" />
        <span v-for="error in user$.email.$errors" :key="error.$uid">{{ error.$message }}</span>
      </div>

      <div class="div-wrap">
        <BaseInput label="Senha" v-model="formData.password" type="password" />
        <span v-for="error in user$.password.$errors" :key="error.$uid">{{ error.$message }}</span>
      </div>
      <div class="div-wrap">
        <BaseInput label="Confirmar Senha" v-model="formData.confirmPassword" type="password" />
        <span v-for="error in user$.confirmPassword.$errors" :key="error.$uid">{{
          error.$message
        }}</span>
      </div>

      <button type="submit">Criar usuário</button>
    </form>
    <!-- grouping errors -->
    <!-- <div>
      <p>Errors:</p>
      <span v-for="error in v$.$errors" :key="error.$uid">
        {{ error.$property }} - {{ error.$message }}
      </span>
    </div> -->
  </body>
</template>

<style scoped>
body {
  background-color: lightslategrey;
  height: 500px;
  padding: 2rem;
  border-radius: 6px;
  color: white;
}

span {
  flex-direction: row;
  line-height: 2;
  color: lightsalmon;
  height: 30px;
  margin-right: 0.5rem;
}

.div-wrap {
  height: 80px;
}

.input-test {
  display: flex;
  width: 100%;
  font-size: 16px;
  padding: 0.25rem;
  border-radius: 6px;
  border: none;
}
</style>
