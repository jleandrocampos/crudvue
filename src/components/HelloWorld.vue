<template>
  <b-container>
    <b-row>
      <b-col>
        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
          <b-form-group id="input-group-1" label="Nome:" label-for="input-1">
            <b-form-input
              id="input-1"
              v-model="form.name"
              type="text"
              required
              placeholder="Digite seu nome"
            ></b-form-input>
          </b-form-group>

          <b-form-group id="input-group-2" label="E-mail:" label-for="input-2">
            <b-form-input
              type="email"
              id="input-2"
              v-model="form.email"
              required
              placeholder="Digite seu E-mail"
            ></b-form-input>
          </b-form-group>

          <b-button
            v-show="form.update == false"
            type="submit"
            class="m-2"
            variant="primary"
            >Cadastrar</b-button
          >
          <b-button
            v-show="form.update == false"
            type="reset"
            class="m-2"
            variant="info"
            >Limpar</b-button
          >
          <b-button
            v-show="form.update"
            @click="updateUser2"
            class="m-2"
            variant="success"
            >Atualizar</b-button
          >
          <b-button
            v-show="form.update"
            @click="cancelUpdate"
            class="m-2"
            variant="danger"
            >Cancelar</b-button
          >
        </b-form>
      </b-col>

      <b-col>
        <b-table
          striped
          hover
          :items="items"
          :fields="fields"
          :tbody-tr-class="rowClass"
        >
          <template slot="actions" slot-scope="row">
            <b-button
              :disabled="form.update"
              variant="success"
              size="sm"
              @click="updateUser(row.item.id)"
              class="mr-1"
            >
              Atualizar
            </b-button>
            <b-button
              :disabled="form.update"
              variant="danger"
              size="sm"
              @click="deleteUser(row.item.id)"
            >
              Deletar
            </b-button>
          </template>
        </b-table>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      form: {
        email: "",
        name: "",
        update: false,
      },
      show: true,
      items: [],
      fields: [
        { key: "name", label: "Nome", sortable: true, sortDirection: "desc" },
        { key: "email", label: "E-mail", class: "text-center" },
        { key: "actions", label: "Ações" },
      ],
      con: 0,
    };
  },
  methods: {
    rowClass(item) {
      if (!item.update) return "table-light";
      if (item.update) return "table-warning";
    },
    onSubmit(evt) {
      evt.preventDefault();
      this.items.push({
        id: this.con,
        name: this.form.name,
        email: this.form.email,
        update: false,
      });
      this.con++;
    },
    onReset(evt) {
      evt.preventDefault();
      // LIMPAR CAMPOS
      this.form.email = "";
      this.form.name = "";
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
    deleteUser(id) {
      for (let index = 0; index < this.items.length; index++) {
        if (id == this.items[index].id) {
          this.items.splice(index, 1);
        }
      }
    },
    updateUser(id) {
      for (let index = 0; index < this.items.length; index++) {
        if (id == this.items[index].id) {
          this.form.email = this.items[index].email;
          this.form.name = this.items[index].name;
          this.form.update = true;
          this.items[index].update = true;
        }
      }
    },
    updateUser2() {
      for (let index = 0; index < this.items.length; index++) {
        if (this.items[index].update) {
          this.items[index].name = this.form.name;
          this.items[index].email = this.form.email;
          this.items[index].update = false;
          this.form.email = "";
          this.form.name = "";
          this.form.update = false;
        }
      }
    },
    cancelUpdate() {
      this.form.email = "";
      this.form.name = "";
      this.form.update = false;
      for (let index = 0; index < this.items.length; index++) {
        this.items[index].update = false;
      }
    },
  },
};
</script>


<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
