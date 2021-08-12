<template>
  <div id="app">
    <v-app id="inspire">
      <v-card color="grey lighten-5 my-10 mx-16">
       <div v-for="i in 10" :key="i">
         <list-item
           >
         </list-item>
       </div>
        <div class="text-center">
          <v-container>
            <v-row justify="center">
              <v-col cols="8">
                <v-container class="max-width">
                  <v-pagination
                      v-model="page"
                      class="my-4"
                      :length="lastPage"
                  ></v-pagination>
                </v-container>
              </v-col>
            </v-row>
          </v-container>
        </div>
      </v-card>
    </v-app>
  </div>
</template>

<script>
import listItem from "./components/listItem";

export default {
  name: 'App',
  components: {
    listItem
  },
  data(){
    return {
      page:1,
      lastPage:15,
    }
  },
  methods:{
    generateFakeData() {

    }
  }
}
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
