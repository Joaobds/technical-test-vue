<template>
  |<div class="container">
    <div v-if="countries.length > 0">
        <div class="row"  style="z-index: 10;">
          <p style="text-align: start;"><b>1. Consumindo de uma API para exibir quatro países.</b></p>
        </div>
        <div v-if="modalOpen">
          <b-modal id="modal-1" title="BootstrapVue">
            <h1 class="my-4">{{capitalName}}</h1>
          </b-modal>
        </div>
        <div class="row"  style="z-index: 10;">
          <div class="col-md-3" v-for="country in countries.slice(0, 4)" :key="country">
            <div class="card">
              <img class="card-img-top" :src="country.flags.png" :alt="country.flags.alt"> 
              <div class="card-body">
                <h5 class="card-title">{{country.name.common}}</h5>
                <a @click="openModal(country.capital, country.name.common)" class="btn btn-dark">Descobrir Capital</a>
              </div>
            </div>
          </div>
        </div>
    </div>
    <div class="row" style="margin-top: 30px; z-index: 10;">
          <p style="text-align: start;"><b>2. Calculando soma dos valores ímpares de um input.</b></p>
    </div>
    <div class="row" style="margin-top: 10px;">
      <div class="col-md-5"  style="z-index: 10;">
        <div class="form-group">
            <input id="number" class="form-field" type="text" placeholder="Digite o Valor" @keypress="isNumber($event)">
        </div>
      </div>
      <div class="col-md-1" style="z-index: 10;">
        <input @click=sumOddNumbers() class="btn btn-dark" type="button" value="Calcular">
      </div>
      <div class="col-md-4" style="margin-top: 10px;" v-if="number.length > 0">
        <p><b>{{number}}</b></p>
      </div>
    </div>
    <div class="row" style="margin-top: 30px; z-index: 10;">
          <p style="text-align: start;"><b>3. Validando Formulário.</b></p>
    </div>
    <div  id="inputsDiv" class="row" style="max-width: 1000px;">
      <div class="col-md-6">
        <div class="mb-3 label-title">
          <label for="name" style="align-self: start;"><b>Nome</b></label>
          <input type="text" class="form-control" id="name" v-model="name" >
        </div>
        <div class="mb-3 label-title">
          <label for="email" style="align-self: start;"><b>Email</b></label>
          <input type="email" class="form-control" id="email" v-model="email" >
        </div>
        <div class="mb-3 label-title">
          <label for="password" style="align-self: start;"><b>Senha</b></label>
          <input type="password" class="form-control" id="password" v-model="password" >
        </div>
        <div class="mb-3 label-title">
          <label for="confirmationPassword" style="align-self: start;"><b>Confirmação de Senha</b></label>
          <input type="password" class="form-control" id="confirmationPassword" v-model="confirmationPassword">
        </div>
        <div style="text-align: start;">
          <div class="form-check form-check-inline">
            <b>Brasileiro ?</b>
          </div>
          <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" v-model="isBrazilian" id="inlineRadio1" value="Y">
            <label class="form-check-label" for="inlineRadio1">Sim</label>
          </div>
          <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" v-model="isBrazilian" id="inlineRadio2" value="N">
            <label class="form-check-label" for="inlineRadio2">Não</label>
          </div>
        </div>
      </div>
      <div class="col-md-6">
        <div class="mb-3 label-title">
          <label for="birthday" style="align-self: start;"><b>Data de Nascimento</b></label>
          <input type="date" class="form-control" v-model="birthday">
        </div>
        <div class="mb-3 label-title">
          <label for="textarea" class="form-label"><b>Termos e Condições</b></label>
          <textarea readonly class="form-control" id="textarea" rows="6">Browser default checkboxes and radios are replaced with the help of .form-check, a series of classes for both input types that improves the layout and behavior of their HTML elements, that provide greater customization and cross browser consistency. Checkboxes are for selecting one or several options in a list, while radios are for selecting one option from many.
          </textarea>
        </div>
        <div class="form-check form-check-inline ">
            <label class="form-check-label">Aceito os Termos e Condições do Serviço.</label>
            <input class="form-check-input" type="radio" name="terms" v-model="terms" id="inlineRadio1" value="accepted">
        </div>
      </div>
    </div>
    <div class="row" style="max-width: 1000px; margin-top: 10px;">
        <div>
          <input class="btn btn-dark" type="submit" value="Enviar" @click=validateInputs() style="width: 100px;">
        </div>
    </div>
  </div> 
</template>
<script>
import axios from 'axios';
import "./styles/style.css"; 
export default {
  name: 'HelloWorld',
  data(){
    return{
      text: 'text',
      countries: [],
      country:{},
      number: '',
      name: '',
      email: '',
      password: '',
      confirmationPassword: '',
      isBrazilian: '',
      birthday: '',
      terms: '',
      modalOpen: false,
      capitalName: ''
    }
  },
  props: {
    msg: String
  },
  methods:{
    fetchCountries: function(){
      let url = 'https://restcountries.com/v3.1/all';
      axios.get(url).then(response =>{
        this.countries = response.data;
        console.log(response.data);
      })
    },
    sumOddNumbers: function() {
    var inputNumber = document.getElementById('number').value;
    let sum = 0;
    for (let i = 1; i <= inputNumber; i += 2) {
      sum += i;
    }
    this.number = 'A soma dos números ímpares desse valor é ' + sum + ".";
    },
    isNumber: function(evt) {
      evt = (evt) ? evt : window.event;
      var charCode = (evt.which) ? evt.which : evt.keyCode;
      if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
        evt.preventDefault();
      } else {
        return true;
      }
    },
    validateInputs: function(){
      if(this.name.trim().length <= 0 || this.email.trim().length <= 0 
      || this.confirmationPassword.trim().length <= 0 || this.password.trim().length <= 0 
      ||this.isBrazilian.trim().length <= 0 || this.birthday.trim().length <= 0 || this.terms.trim().length <= 0){
        alert("Formulário possui campos não preenchidos.");
        return;
      } else if(!this.email.includes("@")){
        alert("Email inválido");
        return;
      } else if(this.confirmationPassword.trim() != this.password.trim()){
        alert("Senhas divergentes.");
        return;
      }
      
    },
    openModal(city, country) {
      this.modalOpen = true;
      this.capitalName = "A capital de " + country + " é " + city;
    }  
  },
  mounted(){
    this.fetchCountries()
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
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
  color: white;
}

.form-label {
  text-align: start;
}

.label-title{
  text-align: start;
}


</style>
