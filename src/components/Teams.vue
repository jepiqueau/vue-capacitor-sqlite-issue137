<template>
  <div class="teams-container">
    <ul>
      <li v-for="team in teams" :key="team.id">
        {{ team.name }}
      </li>
    </ul>
  </div>
</template>
<script>
import { defineComponent, onMounted, ref, getCurrentInstance } from "vue";
export default defineComponent({
  name: "Teams",
  setup() {
    console.log("in setup Teams");
    const app = getCurrentInstance();
    // Retrieve exisiting sqlite connection
    const sqliteConnection =
      app?.appContext.config.globalProperties.$sqliteConnection;
    const teams = ref([]);
    onMounted(async () => {
      let db;
      if (
        !(await sqliteConnection.isConnection("MY_DB")).result ||
        !(await sqliteConnection.checkConnectionsConsistency()).result
      ) {
        console.log("in Teams createConnection");
        // Create a connection to the MY_DB db
        db = await sqliteConnection.createConnection("MY_DB", false);
        // Open the MY_DB db
        await db.open();
      } else {
        // Get existing db connection
        console.log("in Teams retrieveConnection");
        db = await sqliteConnection.retrieveConnection("MY_DB");
      }
      // Query the MY_DB db for all teams
      let res = await db.query("SELECT * FROM teams");
      console.log(`res.values: ${JSON.stringify(res.values)}`);
      teams.value = res.values;
      console.log(`teams: ${JSON.stringify(teams.value)}`);
    });
    return { teams };
  },
});
</script>

<!--
  <router-link :to="'/teams/' + team.id + '/groups'">
    {{ team.name }}
  </router-link>
  <class-actions />
-->
