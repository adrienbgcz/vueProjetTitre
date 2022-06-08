<template>
<section>
  <h1>{{housingProps.titre}}</h1>
    <div class="mapPositionLogoAndText">
        <img class="mapPositionLogo" src="../assets/map-position.png">
        <h2>{{housingProps.nom_centre_vacances}}</h2>
    </div>
    
    <div class="mainPicture">
    <img :src="housingProps.url">
    </div>

    <div class="allLogos">
       
        <div class="logoWithText">
            <img src="../assets/maison-silhouette-noire-sans-porte.png">
            <p>Surface intérieure</p>
            <p>{{housingProps.surface_interieure}} m²</p>
        </div>
        
        <div class="logoWithText">
            <img src="../assets/umbrella.png">
            <p>Surface extérieure</p>
            <p>{{housingProps.surface_exterieure}} m²</p>
        </div>

        <div class="logoWithText">
            <img src="../assets/living-room.png">
            <p>Nombre de pièces</p>
            <p>{{housingProps.nombre_pieces}}</p>
        </div>

        <div class="logoWithText">
            <img src="../assets/bed.png">
            <p>Nombre de chambres</p>
            <p>{{housingProps.nombre_chambres}}</p>
        </div>

        <div v-if="housingProps.animaux == false"  class="logoWithText">
            <img src="../assets/pawprint.png">
            <p>Animaux</p>
            <p>interdits</p>
        </div>

        <div v-if="housingProps.animaux == true" class="logoWithText">
            <img src="../assets/pawprint.png">
            <p>Animaux</p>
            <p>aurorisés</p>
        </div>

        <div>
            <h3>Description</h3>
            <p class="description">{{housingProps.description}}</p>
        </div>


    </div>

        <div class="reservationButtonPosition">
            <router-link :to="{path:`/reservation/${housingProps.id}`}"><button class="reservationButton">Réserver maintenant</button></router-link>
        </div>

    
</section>
</template>

<script>
export default {
   name: "HousingDetailsTemplate",
   props: ["housingProps"],
   data: function() {
       return {
       
       }
   }

};
</script>

<style scoped>
.mapPositionLogo {
    width: 30px;
    height: 30px;
}

.mapPositionLogoAndText {
    display: flex;
    align-items:center;
    padding-top: 0;
    height: 40px; 
}

h1 {
    font-weight: normal;
    font-size: 30px;
    margin-top: 100px;
    margin-left: 5px;
    margin-bottom: 0;
}

h2 {
    font-weight: 600;
    margin-left: 10px;
}

h3 {
    font-size: 25px;
    margin-top: 50px;
}

section {
    width: 1000px;
    margin: auto;
    font-family: lato;
    color: #0B295D;
}

.mainPicture {
    display: flex;
    justify-content: center;
    margin-top: 50px;
}

.logoWithText {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 150px;
}

.logoWithText p {
    display: flex;
    margin-block-start: none;
    margin-block-end: 0;
    margin-inline-start: 0px;
    margin-inline-end: 0px;
    margin-top: 7px;
}


.logoWithText img {
    width: 50px;
}

.allLogos {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between ;
    margin-top: 35px; 
}

.reservationButton {
    font-size: 16px;
    background-color: #D41418;
    color: white;
    padding : 15px;
    width: 200px;
    margin-top: 35px;
    border-radius: 10px;
    border: none;
   
}

.reservationButton:hover {
    color: #D41418;
    background-color: white;
    border:solid;
    padding: 12px;
    cursor: pointer;
}

.reservationButtonPosition {
    display: block;
    text-align:right;
   
}

@media screen and (max-width: 1200px) {

    

    section {
        width: 90%;
   
    }

    .allLogos {
        margin-top: 15px;
        justify-content: center ;  
    }


    .logoWithText {
        margin-top: 20px;
    }

    .mainPicture img{
        width: 100%
    }


}


@media screen and (max-width: 650px) {


    .bannerTitle {
        font-size: 50px;
        width: 90%;
        margin-bottom: 30px;
    }

    section {
        width: 90%;
        margin: auto;
        font-family: lato;
        color: #0B295D;
    }

    .reservationButtonPosition {
        display: block;
        text-align:center;
       
    }

    .mainPicture img{
        width: 100%
    }


}

</style>