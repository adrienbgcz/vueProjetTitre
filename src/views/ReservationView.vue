<template>
  <div>
    <BannerWithButtonTemplate />
    <ReservationFormTemplate
      v-if="isValidated == false"
      :housingProps="housing"
      :reservationsProps="currentReservations"
      @send-data-event="displayValidatedReservation"
    />
    <ValidatedReservationTemplate v-if="isValidated == true" :reservationProps="reservation" 
    :housingProps="housing" :reservationNumberProps="reservationNumber"/>

  </div>
</template>

<script>
import BannerWithButtonTemplate from "../components/BannerWithButtonTemplate.vue";
import ReservationFormTemplate from "../components/ReservationFormTemplate.vue";
import ValidatedReservationTemplate from "../components/ValidatedReservationTemplate.vue"

export default {
  name: "ReservationView",
  components: {
    BannerWithButtonTemplate,
    ReservationFormTemplate,
    ValidatedReservationTemplate
  },
  data: function () {
    return {
      housing: {},
      isValidated : false,
      reservation : {}, 
      currentReservations : [],
      reservationNumber : ""
    };
  },
  methods: {
    getLogementById: async function () {
      try {
        const response = await fetch(
          `http://localhost:9004/logements/${this.$route.params.housingId}`
        );
        const data = await response.json();
        this.housing = data[0];
      } catch (error) {
        console.log(error);
      }
    },
    displayValidatedReservation: async function (reservation) {
      this.reservation = reservation;
      await this.postReservation(reservation);
      this.isValidated = true;
    },
    postReservation: async function(reservation) {
     // on envoie les données récupérées du formulaire sur le service API createReservation qui 
     // va se charger lui-même d'appeler createUser
      const dataReservation = {
        id_logement: reservation.id_logement,
        nom : reservation.utilisateur.nom,
        prenom : reservation.utilisateur.prenom,
        pseudo : reservation.utilisateur.pseudo,
        numero_salarie : reservation.utilisateur.numero_salarie,
        mail : reservation.utilisateur.mail,
        date_arrivee : new Date(reservation.date_arrivee),
        date_depart : new Date(reservation.date_depart)
      }

      const response = await fetch("http://localhost:9004/reservations", {
        method: "POST",
        headers: {
          Accept: "application/json",
          "Content-type": "application/json",
        },
        body: JSON.stringify({reservation : dataReservation}),
      });
      let data = await response.json()
    
      this.getLastReservationOfLogementToCreateReservationNumber(data.idReservation)
    }, 
    
    getLastReservationOfLogementToCreateReservationNumber: async function(idReservation) {
      // on récupère la dernière réservation pour obtenir l'id de l'utilisateur généré et créer un 
      //numéro de réservation personnalisé
      
      const response = await fetch(`http://localhost:9004/reservations/${idReservation}`)
      const reservation = await response.json()

      this.reservationNumber = `ImmIT-RES${idReservation}-U${reservation[0].id_utilisateur}`
  
      this.updateReservationWithReservationNumber(this.reservationNumber, idReservation)
    },
    updateReservationWithReservationNumber: async function(reservationNumber, reservationId) {
      const reservationNumberToSend = {
        numero_reservation : reservationNumber
      }
      try {
          const response = await fetch(`http://localhost:9004/reservations/${reservationId}`, {
            method: "PATCH",
            headers: {
              Accept: "application/json",
              "Content-type": "application/json", 
            },
            body: JSON.stringify(reservationNumberToSend),
          });
        } catch (error) {
          console.log(error);
        }
    },
    getReservationsByLogementToDisplay: async function() {
      const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
      try {
        const response = await fetch(`http://localhost:9004/logements/${this.$route.params.housingId}/reservations`)
        const data = await response.json()
        // on recréé les réservations récupérées en base afin de reformater les dates pour l'affichage
        data.forEach(element => {
            this.currentReservations.push({
            date_arrivee : new Date(element.date_arrivee).toLocaleDateString("fr", options),
            date_depart : new Date(element.date_depart).toLocaleDateString("fr", options),
            id : element.id,
            id_logement : element.id_logement,
            id_utilisateur : element.id_utilisateur,
            numero_reservation : element.numero_reservation
          })
        }) 
      } catch(error) {
        console.log(error)
      }
    }
  },
  created: function () {
    this.getLogementById();
    this.getReservationsByLogementToDisplay();
  } 
};
</script>

<style scoped>
</style>

















<!--date_arrivee : new Date(reservation.date_arrivee).toISOString(),
date_depart : new Date(reservation.date_depart).toISOString()