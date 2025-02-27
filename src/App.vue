<template>
  <div id="app">
    <v-app id="inspire">
      <v-data-table
        :headers="headers"
        :items="desserts"
        hide-default-footer
        class="elevation-1 ma-16"
        :page.sync="page"
        :items-per-page="3"
        @page-count="pageCount = $event"
        :options.sync="options"
        :server-items-length="totalDesserts"
        :loading="loading"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-spacer></v-spacer>
            <v-dialog v-model="dialog" max-width="500px">
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  color="primary"
                  dark
                  class="mb-2"
                  v-bind="attrs"
                  v-on="on"
                >
                  New Item
                </v-btn>
              </template>
              <v-card color="red">
                <v-card-title>
                  <span class="text-h5">{{ formTitle }}</span>
                </v-card-title>
                <v-card-text>
                  <v-textarea
                    auto-grow
                    solo
                    flat
                    v-model="editedItem.name"
                    label="Dessert name"
                  ></v-textarea>
                  <v-textarea
                    auto-grow
                    solo
                    flat
                    v-model="editedItem.calories"
                    label="Calories"
                  ></v-textarea>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" text @click="close">
                    Cancel
                  </v-btn>
                  <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <v-dialog v-model="dialogDelete" max-width="500px">
              <v-card>
                <v-card-title class="text-h5"
                  >Are you sure you want to delete this item?</v-card-title
                >
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" text @click="closeDelete"
                    >Cancel</v-btn
                  >
                  <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                    >OK</v-btn
                  >
                  <v-spacer></v-spacer>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-toolbar>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-btn small class="mr-2" @click="editItem(item)"> edit </v-btn>
          <v-btn small @click="deleteItem(item)"> delete </v-btn>
        </template>
       
        <div class="text-center pt-2"></div>
      </v-data-table>
      <v-pagination
        v-model="page"
        :key="page"
        :length="pageCount"
      ></v-pagination>
      <v-card>
        <v-row justify="center">
          <v-col cols="8">
             <test-data />
          </v-col>
        </v-row>
  
      </v-card>
    </v-app>
  </div>
</template>

<script>

import testData from './components/testData.vue';

export default {
  name: "App",
  components: {testData},
  data: () => ({
    page: 1,
    pageCount: 3,
    dialog: false,
    dialogDelete: false,
     totalDesserts: 0,
      desserts: [],
      loading: true,
      options: {},
    headers: [
      {
        text: "Dessert (100g serving)",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Calories", sortable: false, value: "calories" },
      { text: "Actions", value: "actions", sortable: false },
    ],
       editedIndex: -1,
    editedItem: {
      name: "",
      calories: 0,
    },
    defaultItem: {
      name: "",
      calories: 0,
    },
  }),
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
  },
  mounted () {
    console.log(' mounted run');
    this.getDataFromApi()
  },
  watch: {
    options: {
      handler () {
        this.getDataFromApi()
      },
      deep: true,
    },
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },
  methods: {
    getDataFromApi () {
      this.loading = true
      this.fakeApiCall().then(data => {
         console.log('data', data);
        this.desserts = data.items
        this.totalDesserts = data.total
        this.loading = false
      })
    },
        fakeApiCall () {
      return new Promise((resolve,) => {
        const { page, itemsPerPage } = this.options

        let items = this.getDesserts()
        const total = items.length

        if (itemsPerPage > 0) {
          items = items.slice((page - 1) * itemsPerPage, page * itemsPerPage)
        }

        setTimeout(() => {
          resolve({
            items,
            total,
          })
        }, 2000)
      })
    },
      getDesserts () {
      return [
        {
          name: 'Frozen Yogurt',
          calories: 159,
          fat: 6.0,
         
        },
        {
          name: 'Ice cream sandwich',
          calories: 237,
          fat: 9.0,
       
        },
        {
          name: 'Eclair',
          calories: 262,
          fat: 16.0,
                },
        {
          name: 'Cupcake',
          calories: 305,
          fat: 3.7,
     
        },
        {
          name: 'Gingerbread',
          calories: 356,
          fat: 16.0,
       
        },
        {
          name: 'Jelly bean',
          calories: 375,
          fat: 0.0,
       
        },
        {
          name: 'Lollipop',
          calories: 392,
          fat: 0.2,
         
        },
        {
          name: 'Honeycomb',
          calories: 408,
          fat: 3.2,
          
        },
        {
          name: 'Donut',
          calories: 452,
          fat: 25.0,
          
        },
        {
          name: 'KitKat',
          calories: 518,
          fat: 26.0,
        
        },
      ]
    },
    editItem(item) {
      this.editedIndex = this.desserts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.desserts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.desserts.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem);
      } else {
        this.desserts.push(this.editedItem);
      }
      this.close();
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
