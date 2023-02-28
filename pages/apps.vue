<template>
  <div class="d-flex flex-row">
    <v-card outlined shaped class="flex-grow-1 mr-2">
      <v-simple-table>
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">Title</th>
              <th class="text-left">Users</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="app in apps"
              :key="app.name"
              @click="selectApp(app)"
              :class="{ active: isSelected(app.key) }"
            >
              <td>{{ app.title }}</td>
              <td>{{ app.userCount }}</td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </v-card>

    <v-card outlined shaped class="flex-grow-1 mr-2" v-if="selectedApp.key">
      <div class="information__wrapper">
        <h3>{{ selectedApp.title }}</h3>
        <h4>{{ selectedApp.description }}</h4>
        <div class="users__wrapper">
          <span>Users:</span>
          <span :style="{ color: calculateLimitUsers }">
            {{ `${selectedApp.totalUsers} out of ${selectedApp.userLimit}` }}
          </span>
        </div>
        <div class="usage__wrapper">
          <span>Usage:</span>
          <span :style="{ color: calculateLimitUsage }">
            {{ `${selectedApp.usage} out of ${selectedApp.usageLimit}` }}
          </span>
        </div>
      </div>
    </v-card>
  </div>
</template>

<script>
import { mapState, mapActions } from "vuex";

export default {
  mounted() {
    this.$store.dispatch("Apps/loadApps");
  },

  computed: {
    ...mapState("Apps", ["apps", "selectedApp"]),

    calculateLimitUsers() {
      const percentage =
        (100 * this.selectedApp.totalUsers) / this.selectedApp.userLimit;
      if (percentage < 70) {
        return "green";
      } else if (percentage >= 70 && percentage <= 90) {
        return "yellow";
      } else {
        return "red";
      }
    },

    calculateLimitUsage() {
      const percentage =
        (100 * this.selectedApp.usage) / this.selectedApp.usageLimit;
      if (percentage < 70) {
        return "green";
      } else if (percentage >= 70 && percentage <= 90) {
        return "yellow";
      } else {
        return "red";
      }
    },
  },

  methods: {
    ...mapActions("Apps", ["loadApp"]),
    selectApp(app) {
      this.loadApp(app.key);
    },

    isSelected(key) {
      console.log(key, this.selectedApp.key);
      return this.selectedApp.key === key;
    },
  },
};
</script>

<style scoped>
tbody tr {
  cursor: pointer;
}

.information__wrapper {
  padding: 10px 10px 0 10px;
}

.users__wrapper {
  display: flex;
  justify-content: space-between;
}

.usage__wrapper {
  display: flex;
  justify-content: space-between;
}
</style>
