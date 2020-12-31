<template>
    <div class="form-items-compartilhados row" v-if="this.$root.credentials">
        <div class="col-md-10 col-md-offset-1 text-left">
            <h2 class="form-title">Compartilhar item: {{item.nome}}</h2>

            <div class="success" v-if="success">
                Os dados do item foram atualizados.
            </div>

            {{compartilhamento.emailDestinatario}}
            {{compartilhamento.dataInicio}}
            {{compartilhamento.dataFim}}


            <form @submit.prevent="processForm">
                <div class="form-group">
                    <label for="email">Email</label>
                    <input type="text" class="form-control" id="email"
                           placeholder="Email da pessoa com quem deseja compartilhar"
                           v-model="compartilhamento.emailDestinatario"
                           >
                    <span class="error" v-if="error.nome">{{error.nome}}</span>
                </div>

                <div class="form-group">
                    <label for="data-inicio">De</label>
                    <input type="date" id="data-inicio" v-model="compartilhamento.dataInicio">
                    <span class="error" v-if="error.descricao">{{error.descricao}}</span>
                </div>

                <div class="form-group">
                    <label for="data-fim">Até</label>
                    <input type="date" id="data-fim" v-model="compartilhamento.dataFim">
                    <span class="error" v-if="error.descricao">{{error.descricao}}</span>
                </div>

                <button
                        type="submit"
                        class="btn btn-primary"
                        @click="compartilha"
                >Compartilhar</button>
            </form>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: "CompartilhaItemCompartilhado",
        props: ['item'],
        data() {
            return {
                success: false,
                compartilhamento: {
                    nomeItem: '',
                    emailDestinatario: '',
                    dataInicio: null,
                    dataFim: null,
                },
                error: {},
                httpOptions: {
                    baseURL: this.$root.config.url,
                    headers: {
                        'Accept': 'application/json',
                        'Content-Type': 'application/json',
                        'Authorization': 'Bearer ' + this.$root.credentials.token
                    }
                },
            }
        },
        methods: {
            compartilha: function() {
                this.compartilhamento.nomeItem = this.item.nome;
                axios.put("api/compartilhamento/novo", this.compartilhamento, this.httpOptions)
                    .then(response => {
                        this.success = true;
                        this.error = {};
                        setTimeout(this.goBackToDetails, 3000);
                        console.log('ENTRA na response');
                    })
                    .catch(error => {
                        console.log('ERRO 1');
                        this.error = error.response.data.errors;
                        console.log(error)
                        console.log('ERRO 2');
                    });
            },
            goBackToDetails: function () {
                this.$router.push({
                    name: 'item-details',
                    params: {item: this.item}
                })
            }
        }
    }
</script>

<style scoped>

</style>
