<template>
<div>
  <v-row v-if="infoanimal.photo != ''">
    <v-col cols="12">
      <v-img :src="infoanimal.photo" contain position="left center"></v-img>
    </v-col>
  </v-row>
  <v-row>
    <v-col cols="12">{{infoanimal.description}}</v-col>
  </v-row>
  <v-row >
    <v-col cols="4">
      <span class="mb-0 pa-2 primary--text subtitle-2">Age:</span>
      {{infoanimal.age}}
    </v-col>
    <v-col cols="4">
      <span class="mb-0 pa-2 primary--text subtitle-2">Size:</span>
      {{infoanimal.size}}
    </v-col>
    <v-col cols="4">
      <span class="mb-0 pa-2 primary--text subtitle-2">Gender:</span>
      {{infoanimal.gender}}
    </v-col>
  </v-row>
  <v-row >
    <v-col cols="4">
      <span class="mb-0 pa-2 primary--text subtitle-2">Status:</span>
      {{infoanimal.status}}
    </v-col>
    <v-col cols="4">
      <span class="mb-0 pa-2 primary--text subtitle-2">Coat:</span>
      {{infoanimal.coat}}
    </v-col>
    <v-col cols="4">
      <span class="mb-0 pa-2 primary--text subtitle-2">Specie:</span>
      {{infoanimal.species}}
    </v-col>
  </v-row>
  <v-row>
    <v-col>
      <span class="mb-0 pa-2 primary--text subtitle-2">Contact:</span>
      {{infoanimal.contact.email}}
    </v-col>
  </v-row>
  <v-row>
    <v-col>
      <span class="mb-0 pa-2 primary--text subtitle-2">Phone:</span>
      {{infoanimal.contact.phone}}
    </v-col>
  </v-row>
  </div>
</template>

<script>
export default {
  props: {
    infoanimal: { name: "", age: "", photo: "", description:'' },
  },

  data: () => ({}),

  mounted: function(){
      this.infoanimal.description = this.unescapeHtml(this.infoanimal.description);

  },

  methods: {
    unescapeHtml: function (safe) {
        return safe.replace(/&amp;/g, '&')
            .replace(/&lt;/g, '<')
            .replace(/&gt;/g, '>')
            .replace(/&quot;/g, '"')
            .replace(/&#039;/g, "'");
    }

  }


};
</script>
