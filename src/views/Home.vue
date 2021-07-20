<template>
  <div class="home">
    <h1>Teams</h1>
    <Teams />
  </div>
</template>

<script>
import { defineComponent } from "vue";
import Teams from "@/components/Teams.vue";

export default defineComponent({
  name: "Home",
  components: { Teams },
  setup() {
    console.log(`$$$ In Home setup $$$`);
  },
});
</script>
