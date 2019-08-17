<template>
  <div>
    <v-container class="grey lighten-5">
      <v-row justify="center">
        <v-col cols="4">
            <v-date-picker v-model="initialDate"></v-date-picker>
        </v-col>
        <v-col cols="4">
            <v-date-picker v-model="finalDate"></v-date-picker>
        </v-col>
        <v-col cols="4">
            <v-btn @click="fetchPedidos"> Visualizar </v-btn>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12">
          <span> Total de pedidos: {{ info.total_pedidos }} </span>
          <span> Valor total: R$ {{ total }} </span>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-timeline
            :align-top="alignTop"
            :dense="dense"
            :reverse="reverse"
          >
            <v-timeline-item
              v-for="(n,i) in sales"
              :key="i"
              :fill-dot="fillDot"
              :hide-dot="hideDot"
              :icon="icon ? 'mdi-star' : ''"
              :icon-color=" iconColor ? 'deep-orange' : ''"
              :left="left"
              :right="right"
              :small="small"
            >
              <span slot="opposite">{{n.valor}}</span>
              <v-card class="elevation-2">
                <v-card-title class="headline">{{n.nome}} {{n.telcelular}}</v-card-title>
                <v-card-text>
                  {{n.descricao}}
                </v-card-text>
              </v-card>
            </v-timeline-item>
          </v-timeline>
        </v-col>
      </v-row>
    </v-container>
  </div>

  <!-- <div>{{ result }}</div> -->
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  name: 'Sales',
  data() {
    return  {
      result: {message: {Vendas: {item: []}}},
      initialDate: '2019-08-01',
      finalDate: moment().format('YYYY-MM-DD'),
    }
  },

  computed: {
    info() {
      return this.result.message.Vendas;
    },
    sales() {
      return this.info.item;
    },
    total() {
      const total = this.parseValue(this.info.valor_total) - this.parseValue(this.info.valor_total_cancelados);
      return total.toFixed(2);
    }
  },

  methods: {
    async fetchPedidos() {
      const initialDate = moment(this.initialDate).format('DD/MM/YYYY');
      const finalDate = moment(this.finalDate).format('DD/MM/YYYY');
      const object = {
        class: {
          class: 'Restaurante',
          acao: 'getVendasDojo'
        },
        message: {
          id_restaurante: this.$route.params.id,
          data_inicio: initialDate,
          data_fim: finalDate,
        }
      };
      const url = `http://192.168.201.253/handlers/WhiteLabel.ashx?json=${JSON.stringify(object)}`;
      const { data } = await axios.get(url);
      this.result = data;
    },
    parseValue(value) {
      return parseFloat(value.replace('.','').replace(',','.').substr(3))
    },
  },

  async mounted() {
    this.fetchPedidos();
  }


}
</script>