<template>
  <div>
    <BannerWithButtonTemplate />
    <ReservationFormTemplate
      v-if="isValidated == false"
      :housingProps="housing"
      :reservationsProps="currentReservations"
      @send-data-event="displayReservation"
    />
    <ValidatedReservationTemplate v-if="isValidated == true" :reservationProps="reservation" :housingProps="housing" :reservationWithNumberProps="reservationWithNumber"/>

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
      reservationWithNumber : {}
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
    displayReservation: function (reservation) {
      this.reservation = reservation;
      this.isValidated = true
      this.postReservation(reservation)
    },
    postReservation: async function(reservation) {
     // on envoie les données récupérées du formulaire sur le service API createReservation qui va se charger lui-même d'appeler createUser
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
      this.getReservationsByLogementToCreateReservationNumber()
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
    },
    getReservationsByLogementToCreateReservationNumber: async function() {
      // on récupère la liste de toutes les réservations du logement, et on filtre celui ajouté juste avant, celà permet de récupérer l'id autogénéré après le post
      let reservationFiltered = {}
      const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
      try {
        const response = await fetch(`http://localhost:9004/logements/${this.$route.params.housingId}/reservations`)
        const data = await response.json()

        reservationFiltered = data.filter(element => {
          return (element.date_arrivee == new Date(this.reservation.date_arrivee).toISOString() && element.date_depart == new Date(this.reservation.date_depart).toISOString() && element.id_logement == this.reservation.id_logement)
        })
      } catch(error) {
        console.log(error)
      }
      console.log(reservationFiltered)
      this.reservationWithNumber = {
        date_arrivee : new Date(reservationFiltered[0].date_arrivee).toLocaleDateString("fr", options),
        date_depart : new Date(reservationFiltered[0].date_depart).toLocaleDateString("fr", options),
        id : reservationFiltered[0].id,
        id_logement : reservationFiltered[0].id_logement,
        id_utilisateur : reservationFiltered[0].id_utilisateur,
        numero_reservation : "ImmIT-RES" + reservationFiltered[0].id + "-U" + reservationFiltered[0].id_utilisateur
      }
      this.updateReservationWithReservationNumber(this.reservationWithNumber.numero_reservation, this.reservationWithNumber.id)
    },
    updateReservationWithReservationNumber: async function(reservationNumber, reservationId) {
      const reservationNumberToSend = {
        numero_reservation : reservationNumber
      }
      console.log(reservationNumberToSend)
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