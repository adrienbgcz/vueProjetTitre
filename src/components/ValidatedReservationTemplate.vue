<template>
  <section>
    <div class="reservation">
      <p class="validatedReservationMessage">Réservation validée !</p>
      <p>Vous avez réservé le logement <span>"{{housingProps.titre}}"</span> du <span>{{reservationProps.date_arrivee}}</span> au <span>{{reservationProps.date_depart}}</span></p>
      <p><span>Numéro de réservation :</span> {{reservationWithNumberProps.numero_reservation}}</p>
      <p><span>Nom :</span> {{reservationProps.utilisateur.nom}}</p>
      <p><span>Prénom :</span> {{reservationProps.utilisateur.prenom}}</p>
    </div>
    <button class="downloadButton" @click="printWindow">Télécharger en pdf</button>
  </section>
</template>

<script>
export default {
    name: "ReservationValidatedTemplate",
    props: ["reservationProps", "housingProps", "reservationWithNumberProps"], 
    methods: {
      //on créé une méthode en front afin de retrouver l'id de la réservation en fonction des dates et du logement pour créer un numéro de réservation
      createReservationNumber: function() {
        console.log(this.reservationProps)
      },
      printWindow: function() {
        window.print()
      }
    }, 
    created: function() {
      this.createReservationNumber
    }
    

}
</script>

<style scoped>
.reservation {
    margin-top : 100px;
    width: 500px;
    border: solid;
    box-shadow: 10px 10px 23px -7px rgba(0,0,0,0.66);
    padding: 20px;
    border-width : 3px;
    border-color: #0B295D;
    border-radius: 10px;
}

section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 1000px;
    margin: auto;
    font-family: lato;
    color: #0B295D;
}

span {
  font-weight: bold;
}

.validatedReservationMessage {
  font-size: 20px;
  font-weight: bold;
  text-align: center;
  margin-bottom: 30px;
}

.downloadButton {
    font-size: 16px;
    background-color: #0B295D;
    color: white;
    height: 40px;
    width: 200px;
    margin-top: 50px;
    border-radius: 10px;
    border: none;
    display:flex;
    justify-content: center;
    align-items: center;
}

.downloadButton:hover {
    color: #0B295D;
    background-color: white;
    border:solid;
    cursor: pointer;
}


@media screen and (max-width: 1200px) {
  .reservation {
      width: 500px;
  }

  section {
        width: 90%;
        margin: auto;
        font-family: lato;
        color: #0B295D;
    }

}

@media screen and (max-width: 650px) {
  .reservation {
      width: 95%
  }

  section {
        width: 90%;
        margin: auto;
        font-family: lato;
        color: #0B295D;
    }

}



</style>