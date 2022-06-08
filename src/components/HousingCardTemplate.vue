<template>
  <div>
    <div v-if="selectedProps==filterAll || selectedProps==''" class="card">
        <img :src="housingProps.url"/>
        <div class="cardFooter">
            <div class="logementTitle">{{housingProps.titre}}</div>
            <router-link class="router-link" :to="{path:`/housingDetails/${housingProps.id}`}"><button class="details">Voir le détail →</button></router-link>
        </div>
    </div>

    <div v-if="selectedProps==filterByReservations" class="card">
        <p>{{ housingProps.nombre_reservations }} réservations</p>
        <img :src="housingProps.url"/>
        <div class="cardFooter">
            <div class="logementTitle">{{housingProps.titre}}</div>
            <router-link class="router-link" :to="{path:`/housingDetails/${housingProps.id}`}"><button class="details">Voir le détail →</button></router-link>
        </div>
    </div>
  </div>
    
  
</template>

<script>
export default {
  name: "HousingCardtemplate",
  props : ["housingProps", "selectedProps"],
  data: function() {
    return {
        filterAll : "Tous les logements",
        filterByReservations: "Top 5 par nombre de réservations"
    }
  }
}
</script>

<style scoped>
.card {
    width: 210px;
    height: 260px;
    margin: 30px;
    border-radius: 10px;  
}

.card img {
     
    width: 100%;
    border-radius: 10px;
    height: 100%;
    object-fit: cover;
   
}

.card p {
    font-family: lato;
    color: #0B295D;
    display: flex;
    justify-content: center;
}

.logementTitle {
    font-size: 14px;
    font-weight: 500;
    width: 65px;
    text-align: center;
    color: white;
    font-family: lato;
    
}

.details {
    font-size: 12px;
    font-family: lato;
    font-weight: 600;
    width: 105px;
    display:flex;
    justify-content: center;
    align-items: center;
    height: 25px;
    border-radius: 15px;
    color: #0B295D;
    background-color: white;
    opacity: 70%;
    border-width: 0;
    border-style: none;
    border-color: none;
    border-image: none;
}

.details:hover {
    background-color: #0B295D;
    color:white;
    cursor: pointer;
    
}

.cardFooter{
    display: flex;
    justify-content: space-around;
    align-items: center;
    position: relative;
    bottom: 60px;
}

.router-link {
    text-decoration: none;
}



</style>
