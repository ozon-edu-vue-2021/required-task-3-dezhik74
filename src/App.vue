<template>
  <div id="app">
    <div class="office">
      <Map v-on:table-select="currentEmpChanged" />
      <SideMenu
        :isUserOpenned="showPersonCard"
        :person="employees[currentEmpID]"
        v-on:update:isUserOpenned="currentEmpID = -1"
      />
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";
import people from "./assets/data/people.json";

export default {
  name: "App",
  components: {
    Map,
    SideMenu,
  },
  data() {
    return {
      currentEmpID: -1,
      employees: [],
    };
  },
  methods: {
    currentEmpChanged(newPlaceID) {
      if (newPlaceID === undefined) {
        this.currentEmpID = -1;
        return;
      }
      let newEmpl = this.employees.find((it) => it.tableId == newPlaceID);
      if (newEmpl === undefined) {
        this.currentEmpID = -1;
      } else {
        this.currentEmpID = newEmpl._id;
      }
    },
  },
  computed: {
    showPersonCard: function () {
      return this.currentEmpID > -1;
    },
  },
  mounted() {
    this.employees = people;
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  background-color: #fafafa;
  padding: 24px;
  box-sizing: border-box;
}

html,
body,
#app {
  height: 100%;
}

* {
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

.office {
  display: grid;
  grid-template-columns: 1fr 400px;
  border-radius: 6px;
  border: 1px solid #ccd8e4;
  height: 100%;
  background: white;
  max-width: 1500px;
  margin: 0 auto;
}
</style>
