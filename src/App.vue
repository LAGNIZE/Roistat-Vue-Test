<template>
  <div id="app">
    <img src="./assets/logo.png" />
    <HelloWorld />
    <div class="container">
      <div>
        <button v-on:click="openModal">Добавить</button>
        <button v-on:click="clearLocal">Очистить</button>
        <table class="mainTable" border="2">
          <tbody>
            <tr>
              <th :class="sortedClass('personName')" @click="sortBy('personName')">Имя</th>
              <th>Телефон</th>
            </tr>
            <tr v-for="item in sortedItems" :key="item.personName">
              <td>{{ item.personName }}</td>
              <td>{{ item.personPhone }}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <ModalWindow ref="modal"> </ModalWindow>
    </div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import ModalWindow from "./components/ModalWindow.vue";

export default {
  data() {
    return {
      tableInfo: [],
      visible: true,
      sort: {
        key: "",
        isAsc: false
      }
    };
  },
  created() {
    this.saveData();
  },
  computed: {
    sortedItems() {
      if (this.tableInfo !== null) {
        const list = this.tableInfo.slice();
        if (!!this.sort.key) {
          list.sort((a, b) => {
            a = a[this.sort.key];
            b = b[this.sort.key];
            return (a === b ? 0 : a > b ? 1 : -1) * (this.sort.isAsc ? 1 : -1);
          });
        }
        return list;
      }
    }
  },
  name: "App",
  components: {
    HelloWorld,
    ModalWindow
  },
  methods: {
    openModal: function() {
      this.$refs.modal.visible = true;
    },
    saveData: function() {
      this.tableInfo = JSON.parse(localStorage.getItem("data"));
    },
    clearLocal: function() {
      localStorage.removeItem("data");
      this.saveData();
      this.$refs.modal.personInfo = [];
    },
    sortedClass(key) {
      return this.sort.key === key
        ? `sorted ${this.sort.isAsc ? "asc" : "desc"}`
        : "";
    },
    sortBy(key) {
      this.sort.isAsc = this.sort.key === key ? !this.sort.isAsc : false;
      this.sort.key = key;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
}

#app table {
  margin: 10px auto;
}

.container {
  display: flex;
  justify-content: center;
}

.mainTable {
  width: 300px;
}

.mainTable th:hover{
  cursor: pointer;
}
</style>
