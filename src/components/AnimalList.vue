<template>
  <div class="hello">
      <v-container fluid>
      
        <v-card
          class="mx-auto"
          max-width="844"
          outlined
          color="#AFcFdF"
        >
                  
    <v-card-text>
                
          <div>
            <v-btn color="primary" @click="loadAnimalList">Get 100 firsts Animals</v-btn>  
          </div>

          <v-card
            class="mx-auto"
            max-width="840"
            outlined
          >
          <div>

            <v-container>
                <v-row>
                    <v-col cols="12" sm="6" md="3" class="pa-1">
                        <v-select
                            v-model="typeanimal"
                            outlined
                            flat
                            rounded
                            dense
                            hide-details
                            :items="types"
                            label="Search by type"
                            placeholder="All"
                            class="" 
                            item-text="name"
                            item-value="name"
                        ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="3" class="pa-1">
                        <v-select
                            v-model="breedanimal"
                            outlined
                            flat
                            rounded
                            dense
                            hide-details
                            :items="breeds"
                            label="Search by breed"
                            placeholder="All"
                            class="" 
                            item-text="name"
                            item-value="name"
                        ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="3" class="pa-1">
                        <v-text-field
                            v-model="tagname"
                            class=""
                            dense
                            rounded
                            flat
                            hide-details
                            label="Search by tags"
                            outlined
                            placeholder="sweet,playful,active"
                        ></v-text-field>
                    </v-col>
                </v-row>
            </v-container>

          </div>

          <div>
            <v-data-table
              :headers="headers"
              :items="filteredItems"
              :items-per-page="5"
              class="elevation-1"
            ></v-data-table>
          </div>
  </v-card>
          
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
        { text: 'Id', value: 'id' },
        { text: 'Name', value: 'name' },
        { text: 'Type', value: 'type' },
        { test: 'Tags', value: 'listtag' }
      ],
      types : [],
      breeds: [],
      typeanimal: '',
      breedanimal: '',
      tagname: ''
    }
  },

    created: function() {
        // `this` points to the vm instance
        this.getToken();
    },

    watch: {
      typeanimal : function(val){
        this.loadBreeds(val);
        this.loadAnimalList();
      },
      breedanimal : function(){
        this.loadAnimalList();
      }
    },

    computed: {
      
              filteredItems() {
                return this.items.filter(item => {
                    item.listtag = item.tags.join(',').toLowerCase(); 
                   return (this.tagname == '' || item.listtag.includes(this.tagname.toLowerCase()));                   
                })
              }
    },

  methods: {
    loadTypes() {

      const config = {
          headers: { Authorization: 'Bearer '+this.token }
      };

      let url = 'https://api.petfinder.com/v2/types';
      console.log("url: ", url);

       axios.get(url, config ).then(result => {           
          this.types = result.data.types;

        }).catch( error => {
            console.error(error);
            
      });
    },
    loadBreeds(type) {

      const config = {
          headers: { Authorization: 'Bearer '+this.token }
      };

      let url = 'https://api.petfinder.com/v2/types/'+type+'/breeds';
      console.log("url: ", url);

       axios.get(url, config ).then(result => {           
          this.breeds = result.data.breeds;

        }).catch( error => {
            console.error(error);
            
      });
    },
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

          this.loadTypes();

        }).catch( error => {
            console.error(error);
            
      });
    },

    loadAnimalList: function() {

      const config = {
          headers: { Authorization: 'Bearer '+this.token }
      };

      let url = 'https://api.petfinder.com/v2/animals?limit=100';
      console.log("url: ", url);

      if (this.typeanimal != '') {
        url += '&type='+this.typeanimal;
      }

      if (this.breedanimal != '') {
        url += '&breed='+this.breedanimal;
      }

       axios.get(url, config ).then(result => {           
          this.items = result.data.animals;

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