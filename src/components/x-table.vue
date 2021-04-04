<template>
  <div class="col-md-3 my-3">
    <div
      class="card p-2 d-flex shadow border-0  h-100"
      v-bind:class="{ 'bg-info': !table.isAvailable }"
    >
      <span class="m-auto py-5 text-center" v-if="table.isAvailable"
        >Table Available <br />
        (Capacity = {{ table.capacity }})</span
      >
      <span class="m-auto py-5 text-center" v-if="!table.isAvailable"
        >Occupied By {{ table.occupiedBy }} <br />
        (Free in = {{ Math.floor(+table.durationOfStay / 60) }}:{{
          table.durationOfStay % 60
        }})
        <br />
        <button
          class="btn btn-danger"
          type="button"
          v-on:click="changeStatus(table.id)"
        >
          Evict
        </button>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "XTable",
  props: ["table"],
  created() {
    setInterval(() => {
      if (!this.table.isAvailable) {
        this.$emit("countdownDurationTimeForTableById", this.table.id);
      }
    }, 1000);
  },
  methods: {
    changeStatus(id) {
      this.$emit("evictTable", id);
    },
    async untilAvailable() {
      alert(
        `Waiting for table with capacity ${this.table.capacity} to be free `
      );
      await this.timeout(this.table.durationOfStay * 1000);
      return true;
    },
    timeout(ms) {
      return new Promise(resolve => setTimeout(resolve, ms));
    }
  }
};
</script>

<style></style>
