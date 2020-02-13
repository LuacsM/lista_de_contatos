<template>
  <div>
    <div class="slogan text-center">
      <h1>Agenda de Contatos</h1>
      <br>
      <h5 class="text-secondary">Sistema web de agenda de contatos</h5>
    </div>
    <div id="main">
      <div class="container">
        
        <div class="card">
          <div class="card-body">
              <input class="form-control" type="text" v-model="nome" placeholder="Digite o nome"/><br>
              <input class="form-control" type="text" v-model="numero" placeholder="Digite o número"/><br>
              <input class="form-control" type="text" v-model="email" placeholder="Digite o email"/><br>
              <button type="button" class="btn btn-primary" v-on:click="addContato(nome, numero, email)"><span class="fa fa-plus"></span></button>
          </div>
        </div>
        <br>
        <div class="row">
          <div class="col-md">
            <div id="lista" class="card">
              <div class="card-body">
                <h5 >Contatos: <span class="badge badge-info">{{ nomes.length }}</span></h5>
                <ul class="list-group">
                  <li v-b-modal.modal-1 class="list-group-item" v-for="nome in nomes" v-bind:key="nome" v-on:click="modal(nome)">
                    {{ nome }}
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
        <div>
          <b-modal id="modal-1" title="Contato">
                <ul class="list-group">
                  <li class="list-group-item" v-for="nome in items" v-bind:key="nome" > 
                        {{ nome }}
                  </li>
                </ul>
                <div slot="modal-footer" id="btns">
                      <button class="btn btn-info" v-on:click="deleteContato()"><span class="fa fa-trash"></span></button>
                      <button v-b-modal.modal-2 class="btn btn-info" v-on:click="modal2()"><span class="fa fa-edit"></span></button>
                </div>
          </b-modal>

          <b-modal id="modal-2" title="Editar Contato">
                <input class="form-control" type="text" v-model="nome_edit" placeholder="Digite o nome"/><br>
              <input class="form-control" type="text" v-model="numero_edit" placeholder="Digite o número"/><br>
              <input class="form-control" type="text" v-model="email_edit" placeholder="Digite o email"/><br>
                <div slot="modal-footer" id="btns">
                      <button class="btn btn-info" v-on:click="fecharModal()" >Cancelar</button>
                      <button class="btn btn-info" v-on:click="editContato(nome_edit, numero_edit, email_edit)">Salvar</button>
                </div>
          </b-modal>
        </div>
      </div>
    </div>
    <div id="footer" style="width: 98.95%">
      <div class="row">
        <div class="col-md text-center" style="background-color: #333; color: #ffff;">
          <div class="slogan">
            <h4>Sobre Mim</h4>
            <h5> No final de 2019, me formei na Fundação Matias Machline, antiga
              Fundação Nokia, onde obtive meu diploma de Técnico em
              Informática. Com este, posso dizer que tenho conhecimentos em
              Desenvolvimento Web, especializado em Front-End (HTML, CSS,
              JS); em programação Mobile (Android); Games (Unity); Banco de
              Dados (MySQL) e Desktop (Java).
            </h5>
          </div>
        </div>
        <div class="col-md text-center" style="background-color: #2690d4;; color: #ffff;">
          <div class="slogan">
            <h4>Sobre o Projeto</h4>
            <h5>Este pequeno projeto me foi proposto para avaliar minhas habilidades profissionais.<br><br>
              Foi desenvolvido o Front-end de uma agenda de contatos onde é possível adicionar,
              listar, remover e editar os contatos. 
            </h5>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>



<script>
import "bootstrap/dist/css/bootstrap.css"
import "font-awesome/css/font-awesome.css"

import Vue from 'vue'
import { BModal, VBModal } from 'bootstrap-vue'

Vue.component('b-modal', BModal)
Vue.directive('b-modal', VBModal)


export default {
  name: 'App',

  
  data: function (){
    
    return{
      x: "",
      nome: "", nome_edit: "",
      numero: "", numero_edit: "",
      email: "", email_edit: "",
      nomes: ['Lucas Maia'],
      numeros:['982081756'],
      emails:['lucas.maia.mn@gmail.com'],
      items: ['a', 'b', 'c']
    };
  },

  methods: {

    modal(nome){
      Vue.set(this.items, 0, this.nomes[this.nomes.indexOf(nome)]);
      Vue.set(this.items, 1, this.numeros[this.nomes.indexOf(nome)]);
      Vue.set(this.items, 2, this.emails[this.nomes.indexOf(nome)]);

    },

    modal2(){
      this.$root.$emit('bv::hide::modal', 'modal-1', '#btnShow');
      this.nome_edit = this.items[0];
      this.numero_edit = this.items[1];
      this.email_edit = this.items[2];
    },

    fecharModal(){
     this.$root.$emit('bv::hide::modal', 'modal-2', '#btnShow');
    },

    addContato(nome, numero, email){

      if (!this.nome || !this.numero) {
        alert('O nome e o número são obrigatórios.');
        this.errors.push();
      }

      this.nomes.push(nome);
      this.numeros.push(numero);
      this.emails.push(email);

      this.nome ="";
      this.numero ="";
      this.email ="";
    },

    editContato(nome, numero, email){
      this.x = this.nomes.indexOf(this.items[0]);
      Vue.set(this.nomes, this.x, nome);
      Vue.set(this.numeros, this.x, numero);
      Vue.set(this.emails, this.x, email);
      this.$root.$emit('bv::hide::modal', 'modal-2', '#btnShow');
    },

    deleteContato(){
      this.x = this.nomes.indexOf(this.items[0]);
      this.nomes.splice(this.x, 1);
      this.numeros.splice(this.x, 1);
      this.emails.splice(this.x, 1);
      this.$root.$emit('bv::hide::modal', 'modal-1', '#btnShow')
    },

    checkForm: function (e) {
      

      this.errors = [];

      if (!this.nome) {
        this.errors.push('O nome é obrigatório.');
      }
      if (!this.numero) {
        this.errors.push('A idade é obrigatória.');
      }

      e.preventDefault();
    }
  }
 
}
</script>

<style>
.slogan{
    margin-top: 30px;
    margin-bottom: 30px;
    margin-left: 30px;
    margin-right: 30px;
}

#main{
  background-color: #eaeaea;
  padding-top: 30px;
  padding-bottom: 30px;
}
.selector-for-some-widget {
  box-sizing: content-box;
}

#lista li:hover{
  cursor: pointer;
  background-color: darkgray;
}

#btns button{
  margin-left: 30px;
}
</style>
