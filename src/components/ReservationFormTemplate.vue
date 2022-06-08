<template>
    <section>
        <h1>Formulaire de réservation pour <span>{{housingProps.titre}}</span></h1><img @click="displayInformations" class="informationsLogo" src="../assets/informations.png" />

        <form @submit.prevent="sendDataEvent(reservation.date_arrivee, reservation.date_depart)" action="">
                
                <div class="inputAndLabel">
                    <label for="input-nom" >Nom</label>
                    <input v-model="reservation.utilisateur.nom" @input="isValidatedLastName(reservation.utilisateur.nom)" type="text" name="nom" id="input-nom" required>
                </div>
                <div class="inputAndLabel">
                    <label for="input-prenom">Prénom</label>
                    <input v-model="reservation.utilisateur.prenom" @input="isValidatedFirstName(reservation.utilisateur.prenom)" type="text" name="prenom" id="input-prenom" required>
                </div>
                <div class="inputAndLabel">
                    <label for="input-pseudo">Pseudo</label>
                    <input v-model="reservation.utilisateur.pseudo" @input="isValidatedPseudo(reservation.utilisateur.pseudo)" type="text" name="pseudo" id="input-pseudo" required >
                </div>
                <div class="inputAndLabel">
                    <label for="input-numero">Numéro de salarié</label>
                    <input v-model="reservation.utilisateur.numero_salarie" @input="isValidatedEmployeeNumber(reservation.utilisateur.numero_salarie)" type="text" name="numero" id="input-numero" required>
                </div>
                <div class="inputAndLabel">
                    <label for="input-mail">Mail</label>
                    <input v-model="reservation.utilisateur.mail" @input="isValidatedMail(reservation.utilisateur.mail)" type="mail" name="mail" id="input-mail" required>
                </div>
                <div class="inputAndLabel">
                    <label for="input-date-arrivee">Date d'arrivée</label>
                    <input v-model="reservation.date_arrivee" @input="isValidatedDate()" class="inputDate" type="date" name="numero" id="input-date-arrivee" required>
                </div>
                <div class="inputAndLabel">
                    <label for="input-date-depart">Date de départ</label>
                    <input v-model="reservation.date_depart" @input="isValidatedDate()" class="inputDate" type="date" name="numero" id="input-date-depart" required>
                </div>
                
                <input v-if="!dataFormAreOk" class="reservationButtonDisabled" type="submit" value="Réserver maintenant" disabled>
                <input v-else class="reservationButton" type="submit" value="Réserver maintenant">
            </form>

            <div class="currentReservations">
                <h3>Réservations actuelles</h3>
                <div v-if="reservationsProps.length != 0">
                    <p v-for="reservationProps in reservationsProps" :key="reservationProps.id"> Du {{reservationProps.date_arrivee}} au {{reservationProps.date_depart}}</p>
                </div>
                <div v-else>
                    <p>Aucune réservation actuellement</p>
                </div>
            </div>
    </section>
</template>

<script>
export default {
    name: "ReservationFormTemplate",
    props: ["housingProps", "reservationsProps"],
    data: function(){
        return {
            reservation : {
                id_logement : parseInt(this.$route.params.housingId),
                numero_reservation: "",
                date_arrivee: "",
                date_depart: "",
                utilisateur: {
                    nom: "",
                    prenom: "",
                    pseudo: "",
                    numero_salarie: "",
                    mail: ""
                } 
            },
            firstNameIsOk : false,
            lastNameIsOk : false, 
            pseudoIsOk : false,
            employeeNumberIsok : false,
            mailIsOk : false,
            datesAreOk : false,
            dataFormAreOk : false,
        }
    },
    methods: {
        sendDataEvent: async function(arrivalDate, departureDate) {
            let currentsReservations = []
            try {
                const response = await fetch(`http://localhost:9004/logements/${this.$route.params.housingId}/reservations`)
                currentsReservations = await response.json()
            } catch(error) {
                console.log(error)
            }

            let reservationsOnTheSameDates = currentsReservations.filter(element => {
                return ((  (departureDate <= element.date_depart) && (departureDate >= element.date_arrivee)  ) || (  (arrivalDate <= element.date_depart) && (arrivalDate >= element.date_arrivee) || (arrivalDate <= element.date_arrivee) && (departureDate >= element.date_depart) ))
            })
          
            if (reservationsOnTheSameDates.length == 0) {
                this.$emit("send-data-event", this.reservation)
            } else {
                window.alert(`
                    Le logement est déjà réservé aux dates choisies
                `)
            }
        },
        isValidatedLastName: function(inputValue) {
            const regex = /^[a-zA-Zéèàêïî]{2,20}$/
            const found = inputValue.match(regex)
            if(found != null) {
                this.lastNameIsOk = true
            } else {
                this.lastNameIsOk = false
            }
            this.isValidatedDataForm()
        },
        isValidatedFirstName: function(inputValue) {
            const regex = /^[a-zA-Zéèàêïî]{2,20}$/
            const found = inputValue.match(regex)
            if(found != null) {
                this.firstNameIsOk = true
            } else {
                this.firstNameIsOk = false
            }
            this.isValidatedDataForm()
        },
        isValidatedPseudo: function(inputValue) {
            const regex = /^[a-zA-Z0-9éèàêïî]{2,20}$/
            const found = inputValue.match(regex)
            if(found != null) {
                this.pseudoIsOk = true
            } else {
                this.pseudoIsOk = false
            }
            this.isValidatedDataForm()
        }, 
        isValidatedEmployeeNumber: function(inputValue) {
            const regex = /^[0-9]{4}$/
            const found = inputValue.match(regex)
            if(found != null) {
                this.employeeNumberIsok = true
            } else {
                this.employeeNumberIsok = false
            }
            this.isValidatedDataForm()
        }, 
        isValidatedMail: function(inputValue) {
            const regex = /^[a-z0-9._\-]{2,30}@[a-z0-9]{2,30}\.[a-z]{2,4}$/
            const found = inputValue.match(regex)
            if(found != null) {
                this.mailIsOk = true
            } else {
                this.mailIsOk = false
            }
            this.isValidatedDataForm()
        },
        isValidatedDate: function() {
            if (this.reservation.date_arrivee != "" && this.reservation.date_depart != "") {
                const dateArrivee = new Date(this.reservation.date_arrivee)    
                if((this.reservation.date_arrivee < this.reservation.date_depart) && dateArrivee > Date.now()) {
                    this.datesAreOk = true
                    this.isValidatedDataForm()
                } else if ((this.reservation.date_arrivee > this.reservation.date_depart)) {
                    this.datesAreOk = false
                    this.isValidatedDataForm()
                }
            } 
        },
        isValidatedDataForm: function() {
            if(this.lastNameIsOk && this.firstNameIsOk && this.pseudoIsOk && this.employeeNumberIsok && this.mailIsOk && this.datesAreOk) {
                this.dataFormAreOk = true
            } else {
                this.dataFormAreOk = false
            }
        }, 
        displayInformations: function() {
            window.alert(`
            Le nom, prénom et pseudo doit contenir entre 2 et 20 caractères
            Le nom et le prénom ne doit contenir que des lettre
            Le pseudo peut contenir des lettres et des chiffres
            Le numéro de salarié doit contenir 4 chiffres
            La date de départ doit être supérieure à la date d'arrivée
            `)
        }
        
    }
}
</script>

<style scoped>

.informationsLogo {
    width: 60px
}


.inputAndLabel {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}


form {
    margin-top: 50px;
    width: 500px;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 50px;
    background-color:#c4fffc;
    border-radius: 10px;
    
}

input {
    width : 300px;
    height: 20px;
    margin-top: 10px;
}

.inputDate {
    font-family: lato;
    height: 25px;
}

.reservationButton {
    font-size: 16px;
    background-color: #D41418;
    color: white;
    height: 40px;
    width: 200px;
    margin-top: 30px;
    border-radius: 10px;
    border: none;
    display:flex;
    justify-content: center;
}

.reservationButton:hover {
    color: #D41418;
    background-color: white;
    border:solid;
    
}

.reservationButtonDisabled {
    font-size: 16px;
    background-color: lightgray;
    color: white;
    height: 40px;
    width: 200px;
    margin-top: 30px;
    border-radius: 10px;
    border: none;
    display:flex;
    justify-content: center;
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


h1 {
    font-family: lato;
    font-weight: normal;
    color: #0B295D;
    width: 600px;
    text-align: center;
    margin-top: 150px;
}

span {
    font-weight: 700;
}

.reservation {
    margin-top : 100px;
    width: 500px;
    border: solid;
    box-shadow: 10px 10px 23px -7px rgba(0,0,0,0.66);
    padding: 20px;
    border-width : 1px;
    border-color: #0B295D;
}

.currentReservations {
    margin-top: 30px;
}

h3 {
    text-align: center;
}

@media screen and (max-width: 1200px) {

h1 {
    width: 90%
}

section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100%;
    margin: auto;
    font-family: lato;
    color: #0B295D;
}

}


</style>



 


<!-- const isoDateArrivee = new Date(arrivalDate).toISOString()
const isoDateDepart = new Date(departureDate).toISOString()