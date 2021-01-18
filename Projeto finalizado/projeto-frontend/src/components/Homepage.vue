<template lang="html">
  <div>
    <div class="login">
      <div class="row jumbotron-row">
        <div class="col-md-offset-1 col-md-10">
          <div class="jumbotron text-center">
            <img src="https://lh3.googleusercontent.com/proxy/FdavGDqLh3GIpftYA61b-YcvZwWTyUyjZlPLAwHA2ac2D9HQJEs-KVhgBm5L_RZdTDL4Q92rl8DN09i5-A0wyLLlhmvQ_dYpDTCssJikTsYhVaCW_B6biJaP4jpFvJoJTi7LRoIScu9Fl1G6r0w3aR5yHbviAPtHzg">
            <h1>Economia Compartilhada</h1>
            <p class="lead">O objetivo deste trabalho é ser uma aplicação Web para o compartilhamento de itens. O proprietário registra
            seus itens no sistema e indica que, durante um determinado período, eles serão utilizados por outros usuários. O usuário indicado para receber o item pode confirmar ou cancelar o uso do item.</p>

            <p v-if="!$root.credentials">
              <router-link class="link" :to="{ name: 'login' }">
                <a class="btn btn-lg btn-info" href="#" role="button">Login</a>
              </router-link>
            </p>
          </div>
        </div>
      </div>
  
      <div class="row marketing">
        <div class="col-md-offset-1 col-md-4">
          <h4>Subheading</h4>
          <p>Donec id elit non mi porta gravida at eget metus. Maecenas faucibus mollis interdum. Donec id elit non mi porta gravida at eget metus. Maecenas faucibus mollis interdum.</p>

          <h4>Subheading</h4>
          <p>Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Cras mattis consectetur purus sit amet fermentum. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Cras mattis consectetur purus sit amet fermentum.</p>

          <h4>Subheading</h4>
          <p>Maecenas sed diam eget risus varius blandit sit amet non magna, consectetur ac, vestibulum at eros. Maecenas sed diam eget risus varius blandit sit amet non magna, consectetur ac, vestibulum at eros. Maecenas sed diam eget risus varius blandit sit amet non magna, consectetur ac, vestibulum at eros. </p>
        </div>

        <div class="col-md-offset-2 col-md-4">
          <h4>Subheading</h4>
          <p>Donec id elit non mi porta gravida at eget metus. Maecenas faucibus mollis interdum. Donec id elit non mi porta gravida at eget metus. Maecenas faucibus mollis interdum.</p>

          <h4>Subheading</h4>
          <p>Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Cras mattis consectetur purus sit amet fermentum. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Cras mattis consectetur purus sit amet fermentum.</p>

          <h4>Subheading</h4>
          <p>Maecenas sed diam eget risus varius blandit sit amet non magna, consectetur ac, vestibulum at eros. Maecenas sed diam eget risus varius blandit sit amet non magna, consectetur ac, vestibulum at eros. Maecenas sed diam eget risus varius blandit sit amet non magna, consectetur ac, vestibulum at eros. </p>
        </div>
      </div>
    </div>

    <div class="row footer-row">
      <div class="col-md-offset-1 col-md-10">
        <div class="footer">
          <p>©2020 UNIRIO, Universidade Federal do Estado do Rio de Janeiro {{compartilhamentos}}</p>
        </div>
      </div>
    </div>
  
        <div class="modal show" tabindex="-1" role="dialog" v-if="showModal && compartilhamentos.length">
          <div class="modal-dialog" role="document">
            <div class="modal-content">
              <div class="modal-header">
                <h3 class="modal-title">Olá, você tem {{ compartilhamentos.length }} {{ compartilhamentos.length > 1 ? 'itens' : 'item'}} com status aberto</h3>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                  <span aria-hidden="true" v-on:click="fecharModal()">&times;</span>
                </button>
              </div>
              <div class="modal-body">
                  <div v-for="item in compartilhamentos">
                     <h3 class="text-info">{{ item.nomeItem }} </h3>
                    <p>Data de início do compartilhamento: <b>{{ item.dataInicio }}</b></p>
                    <p>Data de termino do compartilhamento: <b>{{ item.dataTermino }}</b></p>
                    <hr>
                  </div>
                    <h5>Vá para itens compartilhados</h5>
                    <p> Para acessar a tela de itens compartilhados clique aqui: <router-link class="link" :to="{ name: 'item-received-list' }">Compartilhados comigo</router-link></p>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal"  v-on:click="fecharModal()">Fechar</button>
              </div>
            </div>
          </div>
      </div>
</div>

 

</template>

<script>
import axios from "axios";

export default {
  data() {
      return {
        compartilhamentos: [],
        showModal: false,
        httpOptions: {
          baseURL: this.$root.config.url,
          headers: {
            Accept: "application/json",
            "Content-Type": "application/json",
            Authorization: "Bearer " + this.$root.credentials != null ? this.$root.credentials.token : '',
          },
        },
    } 
  },
  created: function () {
    if (this.$root.credentials != null) {
      this.loadCompartilhamentos();
    }
  },
  methods: {
    loadCompartilhamentos: function () {
      axios
        .get("/api/compartilhamento/recebidos/abertos", this.httpOptions)
        .then((response) => {
          this.compartilhamentos = response.data.data;
          this.showModal = true;
        })
        .catch((error) => {
         
        });
    },
    fecharModal: function (){
      this.showModal = false
    }
  }
};
</script>

<style lang="css" scoped>
div.jumbotron-row {
  margin-top: 32px;
}
.marketing h4 {
  font-size: 20px;
}
.footer {
  margin-top: 64px;
  margin-bottom: 32px;
  padding-top: 8px;
  color: #777;
  border-top: 1px solid #e5e5e5;
}
</style>
