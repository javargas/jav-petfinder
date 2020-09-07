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

        headers: [
          { text: 'name', value: 'id' }
        ],
    }
  },

  methods: {
    loadAnimalList: function() {

    const config = {
        headers: { Authorization: `Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJhdWQiOiIwbDZaQzFFMThnVjJGcUZKYzlFVVhGMWw2MEs0QkNVOEkwY3VPNTJ0dHRacktMZGhxbSIsImp0aSI6ImNhNWFkMDZkNjFmMGQzNzA3ODAzOTU0YzIwMWMxMzFiNzA3MWIyOTFmNjVmMjFmMWU4OWJhNzM4ODdjOTA0NjExNTc1MTVlZmRhOTc0YTY5IiwiaWF0IjoxNTk5NDQ5Mzk0LCJuYmYiOjE1OTk0NDkzOTQsImV4cCI6MTU5OTQ1Mjk5NCwic3ViIjoiIiwic2NvcGVzIjpbXX0.aP_duyyJmTUwRDSdnQ8Hea8eNwnLphaJ81Ebcu4C8bSskL4nL6bHQjhLZ5d4ptkwHzOpHCNfZU1rMt1oeOihaJN9GJbLxmwrn8BDZfDEgMTC68P84YGgZuODIVGwGqVayeIpsNIFJzD24fvVRXOodgnWXQza8FBdkygJ5EM670ULw6fOPeeWMjGvxzSWIOephNppVpQEOtg6s5hJ7zLkzDrICb6kYQCWTgNCFwOFBE1Jz8DO7W28esgPXQt2dHG1GRvl9Ee9yWqSFTIcZvEVSY-2fAQQ2jyLdgPGEhZJ7vSUvVCOYpSP6WSRpaSVY0uyGal461ZIOZJ30KFZ11UmgA` }
    };

    axios.defaults.headers.common['Access-Control-Allow-Origin'] = '*';

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