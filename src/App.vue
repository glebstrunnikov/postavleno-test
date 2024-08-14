<template>
  <the-form @addUser="addUser" :data="data"></the-form>
  <the-table @deleteUser="deleteUser" :data="data"></the-table>
  <the-log :log="log"></the-log>
</template>

<script setup>
// В приложении 4 компонента. Компонент TheForm принимает данные для добавления пользователя, а также проводит валидацию этих данных и рисует ошибки. Компонент TheTable создает таблицу ag grid на основе принимаемых данных, следит за обновлениями и ререндерит таблицу, также он эмитит событие - удаление пользователя. TheLog просто отображает лог. Наконец, App запрашивает/собирает/передает/изменяет все данные.
import TheTable from "./components/TheTable.vue";
import TheForm from "./components/TheForm.vue";
import TheLog from "./components/TheLog.vue";
import { ref, onMounted } from "vue";
const data = ref([]);
const log = ref([]);

// Кастомные события deleteUser и addUser вызывают одноименные функции, которые меняют данные для таблицы и заодно делают запись в логе

// Удаление пользователя реализовано в виде фильтр-функции, сравнивающей все поля, которые мы рендерим. Можно было присваивать при загрузке/создании новых пользователей кастомный айди и удалять, основываясь на нем, но для целей приложения, кажется, и так нормально

function deleteUser(payload) {
  if (data.value) {
    data.value = data.value.filter((user) => {
      if (
        user.name.first === payload.name &&
        user.gender === payload.gender &&
        user.email === payload.email &&
        user.dob.age === payload.age
      ) {
        log.value.push({
          name: payload.name,
          type: "delete",
        });
        return false;
      } else {
        return true;
      }
    });
  }
}
function addUser(user) {
  data.value.push({
    name: { first: user.name },
    gender: user.gender,
    email: user.email,
    dob: {
      age: user.age,
    },
  });
  log.value.push({
    name: user.name,
    type: "add",
  });
}
// При загрузке приложения шлем запрос и суем полученные данные в реф, который передаем как проп компоненту TheTable
onMounted(async () => {
  try {
    let res = await fetch("https://randomuser.me/api/?results=5");
    if (!res.ok) {
      throw new Error(`HTTP error! status: ${res.status}`);
    }
    res = await res.json();
    data.value = res.results;
  } catch (error) {
    console.error("An error occurred:", error);
  }
});
</script>
