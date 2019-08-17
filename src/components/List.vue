<template>
  <div>
    <span> Lista de restaurantes </span>
    <v-simple-table>
      <thead>
        <tr>
          <th class="text-left">ID</th>
          <th class="text-left">Nome</th>
          <th class="text-left"></th>

        </tr>
      </thead>
      <tbody>
        <tr v-for="item in places" :key="item.id">
          <td>{{ item.id }}</td>
          <td>{{ item.name }}</td>
          <td>
            <v-btn :to="{ name: 'sales', params: { id: item.id }}"> Visualizar </v-btn>
          </td>

        </tr>
      </tbody>
    </v-simple-table>
  </div>
</template>

<script>
import { mapState } from 'vuex';
export default {
  name: 'List',
  data() {
    return {
      hello: 'WOrld',
    };
  },
  computed:
    mapState([
      'places',
    ])
};
</script>