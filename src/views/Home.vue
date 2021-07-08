<template>
  <div class="home">
    <h1>Teams</h1>
    <Suspense>
      <template #default>
        <Teams />
      </template>
      <template #feedback>
        <div>Loading ...</div>
      </template>
    </Suspense>
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
