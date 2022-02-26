<template>
  <div class="app-wrapper">
    <button @click="isModal = true" class="btn btn-add">Добавить</button>
    <table>
      <thead>
        <td v-for="elem in thead" :key="elem">{{ elem }}</td>
      </thead>
      <company-row
          v-for="company in companies"
          :items="company"
          :key="company.id"
          @deleteCompany="deleteCompany"
          @editAddress="editAddress"
        />
    </table>
    <modal-window
      v-show="isModal"
      :titles="thead"
      @handleModal="handleModal"
      @addCompany="addCompany"
    />
  </div>
</template>

<script>
import ModalWindow from "./components/ModalWindow";
import CompanyRow from "./components/CompanyRow";

export default {
  name: "App",
  components: { ModalWindow, CompanyRow},
  data() {
    return {
      isModal: false,
      thead: [
        "Наименование",
        "Адрес",
        "ОГРН",
        "ИНН",
        "Дата регистрации",
        "Удалить",
      ],
      companies: [],
    };
  },
  methods: {
    handleModal () {
      this.isModal = false;    // закрываем модальное окно
    },

    addCompany (company) {
      this.companies.push(company);
    },

    deleteCompany(id) {
      this.companies = this.companies.filter((company) => company.id !== id);
    },

    editAddress(value, id) {            // изменяем адрес компании
      const editCompany = this.companies.find(company => company.id === id);  
      editCompany.address = value
    }
  },
};
</script>

<style lang="scss">
@import "./styles/style.scss";
</style>
