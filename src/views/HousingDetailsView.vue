<template>

<div>
    <BannerWithButtonTemplate />
    <HousingDetailsTemplate :housingProps="housing" />
</div>

  
</template>

<script>
import BannerWithButtonTemplate from "../components/BannerWithButtonTemplate.vue"
import HousingDetailsTemplate from '../components/HousingDetailsTemplate.vue'

export default {
    name: "HousingDetailsView",
    components : {
        BannerWithButtonTemplate,
        HousingDetailsTemplate
    },
    data: function() {
      return {
        housing: {},
        reservations: []
      }
    },
    methods: {
    getLogementById: async function() {
      try {
        const response = await fetch(`http://localhost:9004/logements/${this.$route.params.housingId}`)
        const data = await response.json()
        this.housing = data[0]
      } catch(error) {
        console.log(error)
      }
    }, 
    

  },
  created: function() {
    this.getLogementById()
  }
}


</script>

<style>

</style>