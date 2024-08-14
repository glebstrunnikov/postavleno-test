<template>
  <ul>
    <li v-for="entry in log" :key="entry.id">
      {{
        `Пользователь ${entry.name} ${
          entry.type === "add"
            ? "добавлен"
            : entry.type === "delete"
            ? "удален"
            : null
        }`
      }}
    </li>
  </ul>
</template>

<script setup>
const props = defineProps(["log"]);
</script>

<style lang="sass" scoped>
ul
    list-style-type: none
</style>
