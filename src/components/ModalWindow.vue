<template>
  <div class="cardModal" v-if="visible">
    <div>
      Добавление пользователя
    </div>
    <div class="modalClose" v-on:click="closeModal()">&#10006;</div>
    <div class="modalOption">
      <div>Имя</div>
      <input v-model="personName" type="text" />
    </div>
    <div class="modalOption">
      <div>
        Телефон
      </div>
      <input v-model="personPhone" type="text" />
    </div>
    <div class="modalOption">
      <div>
        Начальник
      </div>
      <select @change="onchange()" v-model="key">
        <option v-for="option in personInfo" v-bind:key="option.personName">{{ option.personName }}</option>
      </select>
    </div>
    <div>
      <button v-on:click="saveData">Сохранить</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ModalWindow",
  data() {
    return {
      personName: "",
      personPhone: "",
      visible: false,
      personInfo: [],
      key: ""
    };
  },
  mounted() {
    if (localStorage.getItem("data") !== null) {
      this.personInfo = JSON.parse(localStorage.getItem("data"));
    }
  },
  methods: {
    closeModal() {
      this.visible = false;
      this.personName = "";
      this.personPhone = "";
    },
    saveData() {
      this.personInfo.push({
        personName: this.personName,
        personPhone: this.personPhone
      });
      let json = JSON.stringify(this.personInfo);
      localStorage.setItem("data", json);
      this.$parent.saveData();
    },
    onchange: function() {
    	console.log(this.key)
    }
  }
};
</script>

<style scoped>
.modalOption {
  display: flex;
  justify-content: space-between;
  margin: 10px 30px 10px 0;
}

.modalOption {
  display: flex;
  justify-content: space-between;
  margin: 10px 30px 10px 0;
}

.modalOption div{
  margin: 0 30px 0 0;
}

.cardModal {
  width: 300px;
  height: 150px;
  border-radius: 20px;
  border: 1px black solid;
  margin: 0 0 0 50px;
  position: relative;
  text-align: left;
  padding: 10px 10px;
}

.modalClose {
  position: absolute;
  margin: 5px 10px 0 0;
  top: 0;
  right: 0;
}

.modalTitle {
  margin: 10px 0 10px 10px;
}

.modalDescription {
  margin: 10px 10px 0 10px;
}
</style>
