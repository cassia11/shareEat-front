<template class="container">
  <div class="container">
    <img src="../assets/voltar.png" @click="redirect()" width="40" height="20" />
    <h1 id="title">
      <b>{{ title }}</b>
    </h1>
    <br />
    <div id="box container">
      <b-form>
        <label class="text">Nome do Prato</label>
        <b-input placeholder="Prato" id="name" type="text" required v-model.trim="form.name"></b-input>
        <label class="text">Valor</label>
        <b-input-group prepend="R$ " class="mb-2 mr-sm-2 mb-sm-0">
          <b-input placeholder=" R$ 0,00" type="number" id="value" v-model.number="form.value"></b-input>
        </b-input-group>
        <label class="text">Descrição do Prato</label>
        <b-textarea
          rows="4"
          max-rows="6"
          maxlength="200"
          placeholder="Insira uma descrição"
          id="description"
          v-model.trim="form.description"
        ></b-textarea>
        <p class="text">*A descrição deve conter até 200 caracteres.</p>
        <b-container fluid>
          <div>
            <b-button block variant="warning" @click="submitForm()">
              <b>Salvar</b>
            </b-button>
          </div>
        </b-container>
      </b-form>
    </div>
  </div>
</template>

<script>
import platesService from "../services/plates";
import placesService from "../services/places";
export default {
  data: () => ({
    title: "",
    form: {
      name: "",
      value: null,
      description: "",
      places_id: null
    }
  }),
  methods: {
    redirect() {
      window.history.go(-1);
    },
    submitForm() {
      let place = this.$route.params.id;
      this.form.places_id = place;
      platesService
        .save(this.form)
        .then(response => {
          alert(response.data.message);
          this.clearFields();
        })
        .catch(error => {});
    },
    clearFields() {
      this.form.name = "";
      this.form.value = null;
      this.form.description = "";
      this.form.places_id = null;
    }
  },
  created() {
    placesService
      .getPlaceId(this.$route.params.id)
      .then(response => {
        response.data.forEach(e => {
          this.title = e.name;
        });
      })
      .catch(error => {});
  }
};
</script>

<style scoped>
.section {
  text-align: center;
}
#title {
  font: adobe-clean, sans-serif;
  font-weight: bold;
  font-size: 200%;
  color: #f3aa00;
}
.text {
  text-align: center;
  font-family: "Gill Sans Extrabold", Helvetica, sans-serif;
  color: #ffffff;
  opacity: 1;
  text-align: center;
}
</style>