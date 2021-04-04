<template>
  <div class="container">
    <h1 class="pt-5">Resturant Manager</h1>
    <XResturant
      ref="resturant"
      class="pt-5"
      :tables="tables"
      v-on:evictTableById="evictTableById($event)"
      v-on:countdownDurationTimeForTableById="
        countdownDurationTimeForTableById($event)
      "
    />
    <div class="pt-5">
      <h3>Title</h3>
      <p>
        Lorem ipsum dolor sit, amet consectetur adipisicing elit. Consequuntur,
        ipsam.
      </p>
      <form>
        <div class="form-group">
          <input
            type="number"
            class="form-control"
            v-model="numberOfPeople"
            placeholder="Number of People"
          />
        </div>
        <div class="form-group">
          <input
            type="number"
            class="form-control"
            v-model="durationOfStay"
            placeholder="Duration Of Stay"
          />
        </div>
        <div class="form-group">
          <input
            type="text"
            class="form-control"
            v-model="fussMessage"
            placeholder="Fuss Message"
          />
        </div>
        <div class="form-group text-right">
          <button
            class="btn btn-info"
            type="buttons"
            v-on:click="reserveParty()"
          >
            Add
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import XResturant from "./components/x-resturant.vue";
import { v4 } from "uuid";
import moment from "moment";

export default {
  name: "App",
  components: { XResturant },
  created() {
    this.tables = JSON.parse(localStorage.getItem("tables")) || this.tables;
  },
  data: () => {
    return {
      tables: [
        {
          id: v4(),
          capacity: 10,
          isAvailable: true,
          occupiedBy: null,
          durationOfStay: null,
          fussMessage: null
        },
        {
          id: v4(),
          capacity: 8,
          isAvailable: true,
          occupiedBy: null,
          durationOfStay: null,
          fussMessage: null
        },
        {
          id: v4(),
          capacity: 10,
          isAvailable: true,
          occupiedBy: null,
          durationOfStay: null,
          fussMessage: null
        },
        {
          id: v4(),
          capacity: 6,
          isAvailable: true,
          occupiedBy: null,
          durationOfStay: null,
          fussMessage: null
        },
        {
          id: v4(),
          capacity: 2,
          isAvailable: true,
          occupiedBy: null,
          durationOfStay: null,
          fussMessage: null
        },
        {
          id: v4(),
          capacity: 6,
          isAvailable: true,
          occupiedBy: null,
          durationOfStay: null,
          fussMessage: null
        },
        {
          id: v4(),
          capacity: 4,
          isAvailable: true,
          occupiedBy: null,
          durationOfStay: null,
          fussMessage: null
        },
        {
          id: v4(),
          capacity: 12,
          isAvailable: true,
          occupiedBy: null,
          durationOfStay: null,
          fussMessage: null
        }
      ],
      numberOfPeople: "",
      durationOfStay: "",
      fussMessage: ""
    };
  },
  methods: {
    async reserveParty() {
      let currentTables = [...this.tables];
      currentTables.sort((a, b) => a.capacity - b.capacity);
      let isExistButBusy = false;

      for (const table of currentTables) {
        if (table.capacity >= +this.numberOfPeople)
          if (table.isAvailable) {
            table.isAvailable = false;
            table.occupiedBy = this.numberOfPeople;
            table.durationOfStay = this.durationOfStay;
            table.fussMessage = this.fussMessage;
            return;
          } else isExistButBusy = true;
      }
      let numberOfPeople = this.numberOfPeople;
      let durationOfStay = this.durationOfStay;
      let fussMessage = this.fussMessage;
      if (isExistButBusy) {
        let b = false;
        b = await this.$refs.resturant.awaitTable(+this.numberOfPeople);
        if (b) {
          this.numberOfPeople = numberOfPeople;
          this.durationOfStay = durationOfStay;
          this.fussMessage = fussMessage;
          this.reserveParty();
        }
      } else setTimeout(await alert("no such table is available"), 1);
    },
    evictTableById(id) {
      let table = this.tables.filter(table => table.id == id)[0];
      table.isAvailable = true;
      alert(table.fussMessage);
      localStorage.setItem("tables", JSON.stringify(this.tables));
    },
    countdownDurationTimeForTableById(event) {
      let table = this.tables.filter(table => table.id == event)[0];
      if (table.durationOfStay == 0) table.isAvailable = true;
      else {
        table.durationOfStay = table.durationOfStay - 1;
      }
      localStorage.setItem("tables", JSON.stringify(this.tables));
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
