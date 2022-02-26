<template>
  <tr>
      <!-- Наименование компании -->
    <td>{{ items.name }}</td>
    <!-- Адрес -->
    <td v-if="!isEdit" class="td-address">
      {{ items.address }}
      <!-- кнопка редактирования адреса-->
      <button @click="isEdit = true" class="btn btn-edit">
        <img src="https://img.icons8.com/ios-glyphs/30/000000/edit--v1.png" />
      </button>
    </td>
    <!-- если режим редактирования -->
    <td v-else class="td-edit">
      <input v-model="value" ref="input" class="input-edit" @blur="isEdit = false" />
      <div>
        <button @click="saveEdit" class="btn btn-small">
          <img
            src="https://img.icons8.com/material-outlined/24/000000/ok--v1.png"
          />
        </button>
        <button @click="isEdit = false" class="btn btn-small">X</button>
      </div>
    </td>
    <!-- ОГРН-->
    <td>{{ items.ogrn }}</td>
    <!-- ИНН-->
    <td>{{ items.inn }}</td>
    <!-- Дата регистрации-->
    <td>{{ items.date }}</td>
    <td>
      <button
        @click="$emit('deleteCompany', items.id)"
        class="btn btn-small"
      >
        X
      </button>
    </td>
  </tr>
</template>
<script>
export default {
  name: "company-row",
  props: { items: Array },
  data() {
    return {
      isEdit: false,
      value: this.items.address,
    };
  },
  methods: {
    saveEdit() {
      this.$emit("editAddress", this.value, this.items.id);  // отправляем в родительский компонент измененный адрес
      this.isEdit = false;       // убираем режим редактирования
    },
  },
  watch: {
    isEdit(newValue) {
      if (newValue) {            // если режим редактирования, устанавливаем фокус на инпут
        this.$nextTick(() => this.$refs.input.focus());
      }
    },
  },
};
</script>