

<script setup>
import { reactive, computed } from 'vue';
import useVuelidate from '@vuelidate/core';
import { required, minLength, email, sameAs, helpers } from '@vuelidate/validators';


const formData = reactive({
  username: '',
  email: '',
  password: '',
  confirmPassword: ''
})


const rules = computed(() => {
  return {
  username: {
    required,
    // Custom error messages  // , 
    minLength: helpers.withMessage('Minimum length should be 5', minLength(5))
  },
  email: {
    // Custom error messages  // , 
    required: helpers.withMessage('Username is required', required),
    email
  },
  password: {
    required
  },
  confirmPassword: {
    required, 
    sameAs: sameAs(formData.password)
  }
  }
})
const v$ = useVuelidate(rules,formData)

const submitForm = async () =>{
  const result = await v$.value.$validate()

  if(result){
    alert("success")
  }else{
    alert("Failed")
  }
}
</script>

<template>
  <div class="bg-gray-600 w-full h-screen relative" >
    <div class="w-full max-w-96 m-auto rounded bg-gray-800 py-8 px-5 absolute top-14 left-[50%] translate-x-[-50%]">
   <h3 class="text-white font-medium text-base pb-2 ">Create User</h3>
   <form @submit.prevent="submitForm">
      <!----->
      <label class="block text-base font-normal text-gray-200 pb-1">User name</label>
      <input v-model="formData.username" type="text" class="w-full rounded-sm py-1 px-3 border-none focus:outline-none mb-3" placeholder="">
      <span v-for="error in v$.username.$errors" :key="error.$uid" class="text-red-500 ">
        {{ error.$message }}
      </span>
      
       <!----->
      <label class="block text-base font-normal text-gray-200 pb-1">Email</label>
      <input v-model="formData.email" type="text" class="w-full rounded-sm py-1 px-3 border-none focus:outline-none mb-3" placeholder="">
      <span v-for="error in v$.email.$errors" :key="error.uid" class="text-red-500 block text-center">
          {{ error.$message }}
      </span>
       <!----->
      <label class="block text-base font-normal text-gray-200 pb-1">Password</label>
      <input v-model="formData.password" type="text" class="w-full rounded-sm py-1 px-3 border-none focus:outline-none mb-3" placeholder="">
      <span v-for="error in v$.password.$errors" :key="error.uid" class="text-red-500 block text-center">
          {{ error.$message }}
      </span>
       <!----->
      <label class="block text-base font-normal text-gray-200 pb-1">Confirm Password</label>
      <input v-model="formData.confirmPassword" type="text" class="w-full rounded-sm py-1 px-3 border-none focus:outline-none mb-3" placeholder="">
      <span v-for="error in v$.confirmPassword.$errors" :key="error.uid" class="text-red-500 block text-right">
          {{ error.$message }}
      </span>
       <!----->
      <button type="submit" class="bg-gray-600 text-white py-1 px-3 text-sm mt-3">
        Created User
      </button>

   </form>

    <!-- <span v-for=" error in v$.$errors " :key="error.$uid" class=" text-red-500">
       Errors: {{ error.$property }} : {{ error.$message }}
    </span> -->

 </div>
  </div>
 

</template>

<style scoped>

</style>
