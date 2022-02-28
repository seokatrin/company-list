<template>
  <div class="modal-wrapper" @click="closeModal">
    <div class="modal" @click.stop>
      <button class="delete-btn btn" @click="closeModal">X</button>
      <form >
        <form-input v-for="(item, index) in forms" v-model.trim="item.value" :key="index" :title="titles[index]" />
        <p v-show="message" class="error">{{ message }}</p>
        <div class="butons">
          <button @click.prevent="getCompany" class="btn">Загрузить по ИНН</button>
          <button @click.prevent="addCompany" class="btn">Добавить</button>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import FormInput from "./FormInput.vue";

export default {
  name: "modal-window",
  components: {FormInput},
  props: {
    titles: Array
  },
  data() {
    return {
      isOpen: true,
      message: "",
      forms: [
        {field: 'name', value: ''},
        {field: 'address', value: ''},
        {field: 'ogrn', value: ''},
        {field: 'inn', value: ''},
        {field: 'date', value: ''},
      ]
    };
  },
  methods: {
    closeModal () {
      this.$emit('handleModal')    // закрыть модальное окно
      this.cleanModal()           // очистить все поля
    },

    addCompany() {
      if (this.checkFields()) {    // проверить, чтобы все поля были заполнены
        const company = {}        
        for(let i = 0; i < this.forms.length; i++) {
           company[this.forms[i].field] = this.forms[i].value   // создаем объект с данными из формы 
        }
        company.id = Date.now()       
        this.$emit("addCompany", company);        // отправляем в родительский компонент объект с созданной компанией
        this.closeModal()
      }
    },

    checkFields() {
      for(let i = 0; i < this.forms.length; i++) { 
        if(this.forms[i].value === '') {      // проверить, чтобы все поля были заполнены
          this.message = "Заполните все поля";    // если поле пустое, показываем сообщение
          return false;
        }
      }
      return true;             // все поля заполнены
    },

    cleanModal() {         // очистить все поля и сообщение
      for(let i = 0; i < this.forms.length; i++) {
        this.forms[i].value = ''
      }
      this.message = "";
    },

    getCompany() {           // получаем данные компании по ИНН
      if (this.forms[3].value === "") {
        this.message = "Заполните полe ИНН";    // еслт поле ИНН пустое, показываем сообщение
      } else {
        this.message = "";
        const url = "https://suggestions.dadata.ru/suggestions/api/4_1/rs/findById/party";
        const token = "775cf923996c9d3acc8bce7766d73bbdeba73b24";

        const options = {
          method: "POST",
          mode: "cors",
          headers: {
            "Content-Type": "application/json",
            Accept: "application/json",
            Authorization: "Token " + token,
          },
          body: JSON.stringify({ query: this.forms[3].value, "branch_type": "MAIN" }),
        };

        fetch(url, options)
          .then((response) => response.json())
          .then((result) => {                // создаем объект с нужными данными
            result = result.suggestions[0]
            this.forms[0].value = result.value
            this.forms[1].value = result.data.address.value
            this.forms[2].value = result.data.ogrn
            this.forms[4].value = new Date(result.data.ogrn_date).toLocaleDateString()
            
          })
          .catch((error) => {
            this.message = 'Неверный ИНН'   // если ИНН неверный, показываем сообщение
            console.log("error", error)
          } );
      }
    },
  },
};
</script>

