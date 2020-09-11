<template>
    <v-container fluid>
      <v-card class="mx-auto" max-width="844" outlined color="#AFcFdF">
        <v-card-text>
          <!--
          <div>
            <v-btn color="primary" @click="loadAnimalList">Get All Animals</v-btn>
          </div>
          -->
          <v-card class="mx-auto" max-width="1024" outlined>
            <div>
              <v-container>
                <v-row >
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
                      class
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
                      class
                      item-text="name"
                      item-value="name"
                    ></v-select>
                  </v-col>
                  <v-col cols="12" sm="6" md="3" class="pa-1">
                    <v-text-field
                      v-model="tagname"
                      class
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
                :items-per-page="20"
                :footer-props="{
                  'items-per-page-options': [10, 20, 30, 40, 100]
                }"
                class="elevation-1"
                :page="page"
                :pageCount="numberOfPages"
                :options.sync="options"
                :server-items-length="totalAnimals"
                :loading="loading"
              >
                <template v-slot:item.picture="{ item }">
                  <v-row >
                    <v-img :src="item.picture" contain position="left center"  @click="seeDetails(item.id)" width="50" height="50"></v-img>
                  </v-row>
                </template>
                <template v-slot:item.name="{ item }">
                  <v-row >
                    <!--<v-icon class="mr-2" color="tblue" @click="seeDetails(item.id)">mdi-eye</v-icon>-->
                    <a @click="seeDetails(item.id)">{{item.name}}</a>
                  </v-row>
                </template>
                <template v-slot:item.type="{ item }">
                  <v-row  >
                    <a @click="seeDetails(item.id)">{{item.type}}</a>
                  </v-row>
                </template>
                <template v-slot:item.breedname="{ item }">
                  <v-row >
                    <a @click="seeDetails(item.id)">{{item.breedname}}</a>
                  </v-row>
                </template>
                <template v-slot:item.listtag="{ item }">
                  <v-row  >
                    <a @click="seeDetails(item.id)">{{item.listtag}}</a>
                  </v-row>
                </template>
              </v-data-table>
            </div>
          </v-card>
        </v-card-text>
      </v-card>

        <!-- Modal para confirmar un accion en un evento -->
        <v-dialog
            v-model="viewModal"
            max-width="500px"
            persistent
            transition="dialog-transition"
        >
             <v-card>
            <v-card-title class="headline grey lighten-2">
              <span class="mb-0 pa-2 primary--text">Name:</span>
              {{infoanimal.name}}
            </v-card-title>
            <v-card-text>
              <AnimalInfo :infoanimal="infoanimal"  />
            </v-card-text>
            <v-divider></v-divider>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn small dark color="error" @click="viewModal=false">Close</v-btn>
            </v-card-actions>
          </v-card>           
        </v-dialog>

    </v-container>
</template>

<script>
import axios from "axios";
import AnimalInfo from './AnimalInfo';

var endpoint = "https://api.petfinder.com/v2/";

export default {

  components: {
    AnimalInfo
  },

  data: function () {
    return {
      items: [],
      token: null,
      page: 1,
      totalAnimals: 0,
      numberOfPages: 0,
      loading: false,
      options: {},

      headers: [
        //{ text: "Id", value: "id" },
        { text: "Picture", value: "picture" },
        { text: "Name", value: "name" },
        { text: "Type", value: "type" },
        { text: "Breed", value: "breedname" },
        { test: "Tags", value: "listtag" },
      ],
      types: [],
      breeds: [],
      typeanimal: "",
      breedanimal: "",
      tagname: "",
      viewModal: false,
      infoanimal : {name:'', age:'', photo:'', description:''}
    };
  },

  created: function () {
    // `this` points to the vm instance
    this.getToken();
  },

  watch: {
    typeanimal: function (val) {
      this.breeds = [];
      this.breedanimal = '';

      if (val != 'All') {
        this.loadBreeds(val);
      }
      this.loadAnimalList();
    },

    breedanimal: function () {
      this.loadAnimalList();
    },
    options: {
      handler() {
        this.loadAnimalList();
      },
    },
    deep: true,
  },

  computed: {
    filteredItems() {
      return this.items.filter((item) => {
        item.listtag = item.tags.join(",").toLowerCase();

        if (Array.isArray(item.photos) && item.photos.length) {
          item.picture = item.photos[0].small;
        }
        item.breedname = item.breeds.primary
        return (
          this.tagname == "" ||
          item.listtag.includes(this.tagname.toLowerCase())
        );
      });
    },
  },

  methods: {
    loadTypes() {
      const config = {
        headers: { Authorization: "Bearer " + this.token },
      };

      let url = endpoint+"types"
      axios
        .get(url, config)
        .then((result) => {
          this.types = result.data.types;
          this.types.unshift({name:'All'});
        })
        .catch((error) => {
          console.error(error);
        });
    },

    seeDetails(id_animal) {
      const config = {
        headers: { Authorization: "Bearer " + this.token },
      };

      let url = endpoint+"animals/"+id_animal
      axios
        .get(url, config)
        .then((result) => {
          this.infoanimal = result.data.animal;
          this.infoanimal.photo = '';
          if (Array.isArray(this.infoanimal.photos) && this.infoanimal.photos.length) {
            this.infoanimal.photo = this.infoanimal.photos[0].medium;
          }
          this.viewModal = true;
      })
        .catch((error) => {
          console.error(error);
      });

    },
    loadBreeds(type) {
      const config = {
        headers: { Authorization: "Bearer " + this.token },
      };

      let url = endpoint+"types/" + type + "/breeds"
      axios
        .get(url, config)
        .then((result) => {
          this.breeds = result.data.breeds;
          this.breeds.unshift({name:'All'});
        })
        .catch((error) => {
          console.error(error);
        });
    },
    getToken() {
      
      var data = {
        grant_type: "client_credentials",
        client_id: "0l6ZC1E18gV2FqFJc9EUXF1l60K4BCU8I0cuO52tttZrKLdhqm",
        client_secret: "n3fxSyV7JHfXyFFgPMk8yIc8IJPrqF3FmHtKrgVK",
      };

      let url = endpoint+"oauth2/token"
      axios
        .post(url, data)
        .then((result) => {
          this.token = result.data.access_token;
          this.loadTypes();
          this.loadAnimalList();
        })
        .catch((error) => {
          console.error(error);
        });
    },

    loadAnimalList: function () {

      if (this.token == null) return;

      this.loading = true;
     
      const { page, itemsPerPage } = this.options;

      const config = {
        headers: { Authorization: "Bearer " + this.token },
      };

      let url = endpoint+'animals?limit='+itemsPerPage+'&page='+page 

      if (this.typeanimal != "" && this.typeanimal != "All") {
        url += "&type=" + encodeURIComponent(this.typeanimal);
      }

      if (this.breedanimal != "" && this.breedanimal != "All") {
        url += "&breed=" + encodeURIComponent(this.breedanimal);
      }

      axios
        .get(url, config)
        .then((result) => {
          this.items = result.data.animals;
          this.loading = false;

            this.totalAnimals = result.data.pagination.total_count;
            this.numberOfPages = result.data.pagination.total_pages;
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
};
</script>