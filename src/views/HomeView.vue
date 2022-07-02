<template>
  <div>
    <BannerWithoutButtonTemplate />
    <h1>Liste des logements</h1>
    <div class="filtersList">
      <select v-model="selected" @change="getData">
        <option disabled value="">Filtrer par...</option>
        <option>{{ filterAll }}</option>
        <option>{{ filterByReservations }}</option>
      </select>
    </div>
    <div class="allCards">
      <HousingCardTemplate
        v-for="housing in housingList"
        :key="housing.id"
        :housingProps="housing"
        :selectedProps="selected"
      />
    </div>
  </div>
</template>

<script>
import BannerWithoutButtonTemplate from "../components/BannerWithoutButtonTemplate.vue";
import HousingCardTemplate from "../components/HousingCardTemplate.vue";

export default {
  name: "HomeView",
  components: {
    BannerWithoutButtonTemplate,
    HousingCardTemplate,
  },
  data: function() {
    return {
      housingList: [],
      selected: "",
      filterAll: "Tous les logements",
      filterByReservations: "Top 5 par nombre de réservations",
    };
  },
  methods: {
    getData: async function() {
      this.housingList = [];
      if (this.selected == this.filterAll || this.selected == "") {
        try {
          const response = await fetch("http://localhost:9004/logements");
          const data = await response.json();
          this.housingList = data;
        } catch (error) {
          console.log(error);
        }
      }
      if (this.selected == this.filterByReservations) {
        try {
          const response = await fetch(
            "http://localhost:9004/logements?order=reservations&limit=5"
          );
          const data = await response.json();
          this.housingList = data;
        } catch (error) {
          console.log(error);
        }
      }
    },
  },
  created: function() {
    this.getData();
  },
};
</script>

<style scoped>
h1 {
  text-align: center;
  font-family: lato;
  color: #0b295d;
  margin-top: 100px;
  margin-bottom: 30px;
  font-size: 30px;
}

.allCards {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.filtersList {
  display: flex;
  justify-content: center;
  margin-bottom: 50px;
}
</style>
