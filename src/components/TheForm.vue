<template>
  <form @submit.prevent="addUser">
    <label for="input1">Name</label>
    <input v-model="user.name" type="text" id="input1" name="firstName" />

    <label for="input2">Gender</label>
    <select v-model="user.gender" id="input2" name="option">
      <option value="">не выбран</option>
      <option value="male">male</option>
      <option value="female">female</option>
    </select>

    <label for="input3">Email</label>
    <input v-model="user.email" type="email" id="input3" name="email" />

    <label for="input4">Age</label>
    <input v-model="user.age" type="number" id="input4" name="number" />

    <button type="submit">Добавить</button>
  </form>
  <div v-if="errorMsg" ref="errorDiv" class="error-div">
    <h4>{{ errorMsg }}</h4>
  </div>
</template>

<script setup>
import { ref, reactive } from "vue";
const errorDiv = ref(null);
const props = defineProps(["data"]);
const emit = defineEmits(["addUser"]);
const errorMsg = ref(null);
const user = reactive({
  name: "",
  gender: "",
  email: "",
  age: "",
});

function drawError(errorText) {
  errorMsg.value = errorText;
  setTimeout(() => {
    errorMsg.value = null;
  }, 1000);
}

function validate(user) {
  let userValid = true;
  if (
    user.name === "" &&
    user.gender === "" &&
    user.email === "" &&
    user.age === ""
  ) {
    drawError("Cannot add an empty user!");
    userValid = false;
  }
  props.data.forEach((userInData) => {
    if (
      userInData.name.first === user.name &&
      userInData.gender === user.gender &&
      userInData.email === user.email &&
      userInData.dob.age === user.age
    ) {
      drawError("Cannot add the same user twice!");
      userValid = false;
    }
  });
  return userValid;
}

function addUser() {
  if (validate(user)) {
    emit("addUser", user);
  }
}
</script>

<style lang="sass" scoped>
h4
    margin-bottom: 20px
    color: red
input
    width: 100px
form
    display: flex
    align-items: center
    justify-content: center
    gap: 10px
    margin: 20px
</style>
