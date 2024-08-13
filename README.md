### VuelidateHelper.vue
```vue
<script setup>
defineProps({
    field: {
        type: Object,
        default: () => {},
        required: true
    },
    submitted: {
        type: Boolean,
        default: false
    }
})
</script>
<template>
    <div v-if="field.$invalid && submitted" style="margin-bottom: 16px;">
        <span v-for="(error, index) of field.$silentErrors" :key="index">
            <span class="p-error">{{ error.$message }}</span>
        </span>
    </div>
</template>
<style lang="scss" scoped>
.p-error{
    color: red;
}
</style>
```

### Sample Register Example using VuelidateHelper Component
```vue
<script setup>
import { reactive, ref } from 'vue'
import VuelidateHelper from '@/components/VuelidateHelper.vue'
import { useVuelidate } from '@vuelidate/core'
import { email, required, minLength, helpers } from '@vuelidate/validators'

const submitted = ref(false)
const state = reactive({
  firstName: '',
  lastName: '',
  email: '',
  password: '',
  confirmPassword: ""
})

const passwordIsSame = (password) => {
    return password === state.password
}

const rules = {
  firstName: { required },
  lastName: { required },
  email: { required, email },
  password: { required, minLength: minLength(6) },
  confirmPassword: { required, passwordIsSame: helpers.withMessage('Passwords must be same', passwordIsSame) }
}

const v$ = useVuelidate(rules, state)

const handleSubmit = (isFormValid) => {
  submitted.value = true

  if (!isFormValid) {
    return
  }
  registerAccount()
}

const registerAccount = () => {
  alert("Register Account")
}
</script>
<template>
  <div>
    <form @submit.prevent="handleSubmit(!v$.$invalid)">
      <div>
        <label for="firstname">First Name</label>
        <input type="text" v-model="v$.firstName.$model" id="firstname" name="firstname" placeholder="Your first name">
        <VuelidateHelper :field="v$.firstName" :submitted="submitted"></VuelidateHelper>

        <label for="lastname">Last Name</label>
        <input type="text" v-model="v$.lastName.$model" id="lastname" name="lastname" placeholder="Your last name">
        <VuelidateHelper :field="v$.lastName" :submitted="submitted"></VuelidateHelper>

        <label for="email">Email Address</label>
        <input type="email" v-model="v$.email.$model" id="email" name="email" placeholder="Your email address">
        <VuelidateHelper :field="v$.email" :submitted="submitted"></VuelidateHelper>

        <label for="password">Password</label>
        <input type="password" v-model="v$.password.$model" id="password" name="password" placeholder="Password">
        <VuelidateHelper :field="v$.password" :submitted="submitted"></VuelidateHelper>

        <label for="confirm-password">Confirm Password</label>
        <input type="password" v-model="v$.confirmPassword.$model" id="confirm-password" name="confirm-password" placeholder="Confirm Password">
        <VuelidateHelper :field="v$.confirmPassword" :submitted="submitted"></VuelidateHelper>

        <input type="submit" value="Register">
      </div>
    </form>
  </div>
</template>

<style lang="scss" scoped>
form {
  max-width: 500px;
  margin: 64px auto;
}

input[type=text],
input[type=email],
input[type=password] {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

input[type=submit] {
  width: 100%;
  background-color: #4CAF50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

input[type=submit]:hover {
  background-color: #45a049;
}
</style>
```
