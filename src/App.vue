<script lang="ts">
import { defineComponent } from "vue";
import Calculator from "./components/Calculator.vue";

export default defineComponent({
  name: "App",
  components: { Calculator },
});
</script>

<template>
  <v-app>
    <v-main class="d-flex justify-center align-center">
      <Calculator></Calculator>
    </v-main>
  </v-app>
</template>
