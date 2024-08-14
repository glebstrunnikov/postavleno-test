<template>
  <div class="table-wrapper">
    <div id="myGrid" class="ag-theme-quartz"></div>
  </div>
</template>

<script setup>
import { watch, onMounted, ref } from "vue";
const props = defineProps(["data"]);
const emit = defineEmits(["deleteUser"]);
const gridApi = ref(null);

// Кнопка удаления юзера, которая эмитит событие deleteUser для родительского компонента
const delBtnFunc = (params) => {
  const eDiv = document.createElement("div");
  let button = document.createElement("button");
  button.className = "btn-simple";
  button.textContent = "Удалить";
  const eventListener = () => {
    emit("deleteUser", {
      name: params.data.name.first,
      gender: params.data.gender,
      email: params.data.email,
      age: params.data.dob.age,
    });
  };
  button.addEventListener("click", eventListener);
  eDiv.appendChild(button);

  return eDiv;
};

const gridOptions = {
  rowData: props.data,
  columnDefs: [
    { field: "name.first", flex: 2, headerName: "Name" },
    { field: "gender", flex: 1 },
    { field: "email", flex: 2 },
    {
      field: "dob.age",
      flex: 1,
      headerName: "Age",
      valueFormatter: (p) => p.value.toString(),
    },
    {
      field: "button",
      cellRenderer: delBtnFunc,
      headerName: "Action",
    },
  ],
  domLayout: "autoHeight",
};

// При загрузке приожения рендерим таблицу
onMounted(() => {
  const myGridElement = document.querySelector("#myGrid");
  gridApi.value = agGrid.createGrid(myGridElement, gridOptions);
});

// Главный вотчер, который следит за изменениями рефа с данными и ререндерит таблицу
watch(
  () => props.data,
  (data) => {
    if (data) {
      gridApi.value.setGridOption("rowData", data);
    }
  },
  { deep: true }
);
</script>
<style scoped lang="sass">
.table-wrapper
    width: 800px
#myGrid
    width: 100%
</style>
