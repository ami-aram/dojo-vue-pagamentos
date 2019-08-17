<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Vue</span>
        <span class="font-weight-light">Dojo </span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        text
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
      >
        <span class="mr-2">Sábado 17/08</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <router-view></router-view>
    </v-content>
  </v-app>
</template>

<script>
// import List from './views/List';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    // List,
  },
  data: () => ({
    //
  }),
  async mounted() {
    const object = {
      class: {
        class: 'Restaurante',
        acao: 'getVendasDojo'
      },
      message: {
        id_restaurante: 80,
        data_inicio: '01/08/2019',
        data_fim: '',
      }
    };
    // eslint-disable-next-line
    const url = `http://192.168.201.253/handlers/WhiteLabel.ashx?json=${JSON.stringify(object)}`;
    const { data } = await axios.get(url);
    // eslint-disable-next-line
    console.log(data);
  }
};
</script>
