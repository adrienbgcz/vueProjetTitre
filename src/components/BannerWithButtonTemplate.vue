<template>
  <div class="banner">
        <div class="bannerTitle">Réservez vos vacances grâce au CE</div>
        <router-link v-if="!isReservationView" class="router-link" :to="{path:'/'}"><button class="returnButton">Retour aux logements →</button></router-link>
        <router-link v-else class="router-link" :to="{path:`/housingDetails/${this.$route.params.housingId}`}"><button class="returnButton">Retour au logement →</button></router-link>
    
    </div>
</template>

<script>
export default {
    name: "BannerWithButtonTemplate",
    data: function(){
        return {
            isReservationView : null
        }
    },
    created: function() {
        const params = "ReservationView"
        const paramsToCompare = this.$route.name
        params == paramsToCompare ? this.isReservationView = true : null
        // console.log(params)
        // console.log(paramsToCompare)
        // console.log(this.$route.params)
    }


}
</script>

<style scoped>
.banner {
    height: 50vh;
    background: url("../assets/banner.jpeg") no-repeat;
    background-size: cover;
    background-position-y: center;
    display:flex;
    flex-direction: column;
    justify-content:flex-end;
    align-items: center;
    padding-bottom:60px;
}

.bannerTitle {
    color: white;
    font-family: lato;
    font-weight:500;
    font-size: 60px;
    width: 700px;
    display: flex;
    justify-content: center;
    text-align: center;
    margin-bottom: 50px;
}

.returnButton {
    font-size: 16px;
    font-family: lato;
    font-weight: 600;
    width: 220px;
    display:flex;
    justify-content: center;
    align-items: center;
    height: 50px;
    border-radius: 30px;
    color: #0B295D;
    background-color: white;
    opacity: 70%;
    border-width: 0;
    border-style: none;
    border-color: none;
    border-image: none;
    
    
}

.returnButton:hover {
    background-color: #0B295D;
    color:white;
    cursor: pointer;
    
}

.router-link {
    text-decoration: none;
    
}


@media screen and (max-width: 650px) {


    .bannerTitle {
        font-size: 50px;
        width: 90%;
        margin-bottom: 30px;
    }
}
</style>