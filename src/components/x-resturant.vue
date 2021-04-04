<template>
  <div>
    <h3>Table Overview</h3>
    <div class="row mx-0  border p-3">
      <XTable
        ref="table_ref"
        v-for="table in tables"
        :key="table.id"
        :table="table"
        v-on:evictTable="evictTable($event)"
        v-on:countdownDurationTimeForTableById="
          countdownDurationTimeForTableById($event)
        "
      />
    </div>
  </div>
</template>

<script>
import XTable from "./x-table.vue";
export default {
  name: "XResturant",
  components: { XTable },
  props: ["tables"],

  created() {},
  methods: {
    evictTable(event) {
      this.$emit("evictTableById", event);
    },
    countdownDurationTimeForTableById(event) {
      this.$emit("countdownDurationTimeForTableById", event);
    },
    async awaitTable(numberOfPeople) {
      let currentTables = [...this.tables];
      let sotredTables = currentTables.sort((a, b) => a.capacity - b.capacity);
      for (let index = 0; index < sotredTables.length; index++) {
        if (sotredTables[index].capacity >= +numberOfPeople) {
          for (const table1 of this.$refs.table_ref) {
            if (table1.table.id == sotredTables[index].id) {
              return await table1.untilAvailable();
            }
          }
        }
      }
    }
  }
};
</script>

<style></style>
