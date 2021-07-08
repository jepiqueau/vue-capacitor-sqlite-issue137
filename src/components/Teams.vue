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
  async setup() {
    console.log("in setup Teams");
    const app = getCurrentInstance();
    // Retrieve exisiting sqlite connection
    const sqliteConnection =
      app?.appContext.config.globalProperties.$sqliteConnection;
    const teams = ref([]);
    const getTeams = async () => {
      console.log("in get Teams");
/*      return [
        { id: 1, name: "Team 1", last_modified: 1624370619 },
        { id: 2, name: "Team 2", last_modified: 1624370619 },
      ];
      */

      let db;
      if (
        !(await sqliteConnection.isConnection("MY_DB")) ||
        !sqliteConnection.checkConnectionsConsistency()
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
      // Store teams
      return res.values;
      // Close database connection
      // await sqliteConnection.closeConnection("MY_DB", false);

    };
    onMounted(async () => {
      teams.value = await getTeams();
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
