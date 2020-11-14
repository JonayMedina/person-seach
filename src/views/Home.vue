<template>
  <div class="home">
    <Search msg="Bienvenidos al panel de Busqueda" />
  </div>
</template>

<script>
// @ is an alias to /src
import Search from "@/components/Search.vue";
export default {
  name: "Home",
  components: {
    Search
  }
};
</script>
