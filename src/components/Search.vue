<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mb-4">
        <v-card elevation="14">
          <v-card-title primary-title>
            <div>
              <h1 class="display-2 font-weight-bold mb-3">
                App de Busqueda
              </h1>
            </div>
          </v-card-title>
          <v-card-text>
            <v-text-field
              type="text"
              v-model="search"
              prepend-inner-icon="mdi-account-box-outline"
              placeholder="Ingrese Nombre"
              required
              label="Buscar persona por nombre"
            />
          </v-card-text>
          <v-card-actions>
            <v-btn
              color="purple"
              class="white--text"
              :disabled="!search"
              prepend-inner-icon="mdi-magnify"
              :loading="load"
              @click="find(search)"
            >
              Buscar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
      <v-container v-if="load">
        <div class="text-xs-center">
          <v-progress-circular
            indeterminate
            :size="150"
            :width="8"
            color="purple"
          >
          </v-progress-circular>
        </div>
      </v-container>
      <v-container v-else grid-list-xl>
        <div v-if="itunes[0] || tvmaze[0] || crcind.name">
          <h1 class="display-2 font-weight-bold mb-3 white--text">
            Coincidencias con {{ search }}
          </h1>
        </div>
        <v-layout wrap>
          <div v-if="crcind.name">
            <v-flex xs12 mb-2>
              <v-card elevation="14">
                <v-img src="@/assets/crc.png" aspect-ratio="1"></v-img>
                <v-card-title primary-title>
                  <div>
                    <h2>{{ crcind.name }}</h2>
                    <div>SSN: {{ crcind.ssn }}</div>
                    <div>DOB: {{ crcind.dob }}</div>
                    <div>SITIO WEB: {{ crcind.site }}</div>
                  </div>
                </v-card-title>
              </v-card>
            </v-flex>
          </div>
          <v-flex xs3 v-for="(item, index) in itunes" :key="index" mb-2>
            <v-img
              elevation-12
              v-show="itunes"
              alt="Itunes"
              class="shrink mr-2"
              contain
              src="@/assets/itunes.png"
              transition="scale-transition"
              width="40"
            />
            <v-card elevation="10">
              <v-img :src="item.artworkUrl30" aspect-ratio="1"></v-img>
              <v-card-title primary-title>
                <div>
                  <h2>{{ item.Track }}</h2>
                  <div>Tipo: {{ item.kind }}</div>
                  <div>Artista: {{ item.artistName }}</div>
                  <div>Nombre de pista: {{ item.trackName }}</div>
                  <div>Nombre sensurado: {{ item.trackCensoredName }}</div>
                </div>
              </v-card-title>
              <!-- <v-card-actions class="justify-center">
                <v-btn flat color="green" @click="singleMovie(item.imdbID)"
                  >View</v-btn
                >
              </v-card-actions> -->
            </v-card>
          </v-flex>
          <v-flex xs3 v-for="(item, index) in tvmaze" :key="index" mb-2>
            <v-img
              v-show="tvmaze"
              alt="TVM"
              class="shrink mr-2"
              contain
              src="@/assets/tvm-header.png"
              transition="scale-transition"
              width="40"
              height="30"
            />
            <v-card elevation="10">
              <v-img
                v-if="item.show.image && item.show.image.medium"
                :src="item.show.image.medium"
                aspect-ratio="1"
              >
              </v-img>
              <v-img
                v-else-if="item.show.image && item.show.image.original"
                :src="item.show.image.original"
                aspect-ratio="1"
              >
              </v-img>
              <v-card-title primary-title>
                <div>
                  <h2>{{ item.show.name }}</h2>
                  <div>Tipo: {{ item.show.type }}</div>
                  <div v-if="'Canal: ' + item.network.name"></div>
                  <div>
                    Genero:
                    <span
                      v-for="(gen, index1) in item.show.genres"
                      :key="index1"
                      v-text="gen"
                    >
                    </span>
                  </div>
                  <div>Puntuacion: {{ item.score }}</div>
                </div>
              </v-card-title>
              <!-- <v-card-actions class="justify-center">
                <v-btn flat color="green" @click="singleMovie(item.imdbID)"
                  >View</v-btn
                >
              </v-card-actions> -->
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  name: "Search",

  data: () => ({
    search: "",
    load: null,
    results: [],
    itunes: [],
    tvmaze: [],
    crcind: []
  }),
  methods: {
    find(search) {
      let me = this;
      me.load = true;
      axios
        .get("http://localhost:8000/api/v1/search/" + search)
        .then(response => {
          me.itunes = response.data[0].itunes.results;
          me.tvmaze = response.data[1].tvmaze;
          me.crcind = response.data[2].crcind;
        })
        .catch(err => console.log(err))
        .finally(() => (me.load = false));
    }
  }
};
</script>
<style scoped>
.v-progress-circular {
  margin: 1rem;
}
</style>
