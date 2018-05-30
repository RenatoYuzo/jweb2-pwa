<template>
    <v-container>
        <v-layout row>
          <h1 class="display-4">Novo usuário</h1>
        </v-layout>
        <v-layout row>
          <v-flex xs6 offset-xs3>
            <v-form ref="form">
              <v-text-field
                v-model="usuario.nome"
                label="Nome"
                required
              ></v-text-field>
              <v-text-field
                v-model="usuario.email"
                label="Email"
                required
              ></v-text-field>
              <v-text-field
                v-model="usuario.senha"
                label="Senha"
                type="password"
                required
              ></v-text-field>

              <v-btn
                @click="salvar"
              >
                Salvar
              </v-btn>
              <v-btn @click="limpar">Cancelar</v-btn>
            </v-form>
          </v-flex>
        </v-layout>
        <v-layout row wrap class="text-xs-center">
            <h1 class="display-4">Usuários cadastrados</h1>
        </v-layout>
        <v-data-table
                :headers="headers"
                :items="usuarios"
                hide-actions
                class="elevation-1"
              >
                <template slot="items" slot-scope="usu">
                  <td>{{ usu.item.nome }}</td>
                  <td>{{ usu.item.email }}</td>
                </template>
              </v-data-table>
        <v-layout>

        </v-layout>
    </v-container>
</template>

<script>
  import axios from 'axios'

  export default {
    data () {
      return {
        usuario: {},
        usuarios: [],
        headers: [
          { text: 'Nome', value: 'nome', sortable: true },
          { text: 'Email', value: 'email', sortable: true }
        ]
      }
    },
    methods: {
      async buscarTodos(){
        // HTTP request GET (http://localhost:8080/jweb2/usuarios)
        var response = await axios.get('http://localhost:8080/jweb2/usuarios');
        this.usuarios = response.data;
      },
      async salvar(){
        // HTTP request POST (http://localhost:8080/jweb2/usuarios)
        var response = await axios.post('http://localhost:8080/jweb2/usuarios', this.usuario);
        this.buscarTodos();
        this.limpar();
      },
      limpar(){
        this.usuario.nome = "";
        this.usuario.email = "";
        this.usuario.senha = "";
      }
    },
    mounted(){
      this.buscarTodos();
    }

  }
</script>
<style scoped>
  h1{
    font-size: 30pt !important;
  }
</style>
