<template>
<div id="app">
  <v-app id="inspire ma-16">
    <div>
      <v-data-table
        :headers="headers"
        :items="desserts"
        :options.sync="options"
        :server-items-length="totalDesserts"
        :loading="loading"
        class="elevation-1"
      ></v-data-table>
    </div>
  </v-app>
</div>
</template>

<script>
export default {
  name: "testData",
   data () {
    return {
      totalDesserts: 0,
      desserts: [],
      loading: true,
      options: {},
      headers: [
        {
          text: 'Dessert (100g serving)',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'Calories', value: 'calories',sortable: false, },
        { text: 'Fat (g)', value: 'fat',sortable: false, },

      ],
    }
  },
  watch: {
    options: {
      handler () {
        this.getDataFromApi()
      },
      deep: true,
    },
  },
  mounted () {
    console.log(' mounted run');
    this.getDataFromApi()
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
    /**
     * In a real application this would be a call to fetch() or axios.get()
     */
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
  },
}
</script>