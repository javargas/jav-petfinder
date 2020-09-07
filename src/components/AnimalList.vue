<template>
  <div class="hello">
      <v-container fluid>
      
        <v-card
          class="mx-auto"
          max-width="644"
          outlined
          color="#AFcFdF"
        >
                  
    <v-card-text>
                
          <div>
            <v-btn color="primary" @click="loadAnimalList">Domain List</v-btn>  
          </div>

          <div>
            <v-data-table
              :headers="headers"
              :items="items"
              :items-per-page="5"
              class="elevation-1"
            ></v-data-table>
          </div>
          
    </v-card-text>
          </v-card>
      </v-container>
  </div>
</template>

<script>

import axios from 'axios'

export default {
  props: {
    msg: String
  },
    
  data: function() {
    return {
      fields: ['Domain'],
      items: [],
      token:null,

        headers: [
          { text: 'name', value: 'id' }
        ],
    }
  },

    created: function() {
        // `this` points to the vm instance
        this.getToken();
    },

  methods: {
    getToken() {

      let url = 'https://api.petfinder.com/v2/oauth2/token';
      var data = {
            grant_type: 'client_credentials',
            client_id : '0l6ZC1E18gV2FqFJc9EUXF1l60K4BCU8I0cuO52tttZrKLdhqm',
            client_secret : 'n3fxSyV7JHfXyFFgPMk8yIc8IJPrqF3FmHtKrgVK'
      };
      console.log("url: ", url);

       axios.post(url, data ).then(result => { 
          console.log(result) 
          
          this.token = result.data.access_token;

        }).catch( error => {
            console.error(error);
            
      });
    },

    loadAnimalList: function() {

    const config = {
        headers: { Authorization: 'Bearer '+this.token }
    };

      let url = 'https://api.petfinder.com/v2/animals';
      console.log("url: ", url);

       axios.get(url, config ).then(result => { 
          console.log(result.data) 
          
          this.items = result.data.items;

        }).catch( error => {
            console.error(error);
            
      });
    
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>