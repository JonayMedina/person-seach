<template>
  <v-app>
    <v-app-bar app color="goldenrod" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="@/assets/logo.png"
          transition="scale-transition"
          width="100"
        />
      </div>
    </v-app-bar>

    <v-main>
      <Search />
    </v-main>
  </v-app>
</template>

<script>
import Search from "./components/Search";

export default {
  name: "App",

  components: {
    Search
  },

  data: () => ({
    //
  })
};
</script>
<style>
.theme--light.v-application {
  background-image: linear-gradient(
    to right top,
    #f2f2f2,
    #c1c1c1,
    #929293,
    #666567,
    #3d3c3e,
    #332e36,
    #2c1f2d,
    #251022,
    #371232,
    #4a1142,
    #5e0d53,
    #730364
  );
}
</style>
