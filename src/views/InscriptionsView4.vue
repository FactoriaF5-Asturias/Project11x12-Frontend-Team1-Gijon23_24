<script setup>
import BotonAtras from "@/components/inscriptions/BotonAtras.vue";
import BotonesLaterales from "@/components/inscriptions/BotonesLaterales.vue";
import BotonSiguiente from "@/components/inscriptions/BotonSiguiente.vue";
import ComboParticipante from "@/components/inscriptions/ComboParticipante.vue";
import ParticipantesOpciones from "@/components/inscriptions/ParticipantesOpciones.vue";
import { useAuthStore } from "@/stores/auth";
import { useRouter } from "vue-router";
import { ref } from "vue";
import { provide } from "vue";
import { useSchoolStore } from "@/stores/schoolStore";
import { useSelectedParticipantStore } from "@/stores/selectedParticipantStore";
import { useSelectedCampStore } from "@/stores/selectedCampStore";

const router = useRouter();

const goStep3 = () => {
  router.push("/inscriptionPaso3");
};
const goStep5 = () => {
  router.push("/inscriptionPaso5");
};
const authStore = useAuthStore();
const schoolStore = useSchoolStore();
const selectedSchool = schoolStore.selectedSchool;

const selectedCamp = useSelectedCampStore();
const selectedParticipantStore = useSelectedParticipantStore();
const selectedParticipant = selectedParticipantStore.selectedParticipants;

const pasoActual = ref(3);
provide('pasoActual', pasoActual);

</script>
<template>
  <main>
    <div>
      <div class="saludo">
        <h2>¡Hola {{ authStore.user.username }}!</h2>
      </div>
      <div class="bajoSaludo">
        <aside class="barraLateral">
          <BotonesLaterales />
          <img
            class="logo11x12"
            src="../assets/img/11x12.jpg"
            alt="imagen 11x12 rosa con letras blancas"
          />
        </aside>
        <div class="cuerpoInscripcion">
          <div class="campamento">
            <h3> {{selectedCamp.campDetails.camp_name}}</h3>
            <h4>{{ selectedCamp.campDetails.schedule}}</h4>
          </div>
          <div class="colegio">
            <h3>{{selectedSchool}}</h3>
            <!-- <h4>Plazas disponibles {places_num}</h4> -->
          </div>
          <div class="participantes">
            <h3>Seleccionar Participante:</h3>
            <ul>
              <li v-for="participant in selectedParticipant" :key="participant.id_participant">
                {{ participant.participantName }} {{ participant.participantSurname }}
              </li>
            </ul>
            <!-- <button @click="showComboParticipante">Añadir Participante</button> -->
          </div>
          
          <div class="participantes">
            <h3>Seleccionar Participante:</h3>
            <ComboParticipante/>
            
          </div>
          <!-- <div class="opcionesParticipantes">
            <ParticipantesOpciones/>
          </div> -->

          <div class="AtrasSiguiente">
            <div class="Siguiente">
              <BotonSiguiente @goToNextStep="goStep5" />
            </div>
            <div class="Atras">
               <!-- <BotonAtras @goToPreviusStep="goStep3" /> -->
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped lang="scss">
@import "../assets/scss/variables.scss";

h2 {
  padding-left: 2%;
  padding-top: 2%;
}

.campamento {
  font-family: Arial;
}

.bajoSaludo {
  display: flex;
  align-items: center;
  margin-top: 0px;
}

aside {
  width: 20%;
  align-items: center;
  margin: 1%;
}

.logo11x12 {
  width: 100%;
  margin-top: 2%;
}

.cuerpoInscripcion {
  background-color: $gray-form;
  width: 100%;
  height: 550px;
  margin-right: 1%;
}

.AtrasSiguiente {
  display: flex;
  flex-direction: row-reverse;
  margin: 1%;
  gap: 1%;
}
.Siguiente {
  text-align: right;
  margin-right: 2%;
}

@media only screen and (max-width: 768px) {
  .barraLateral {
    display: none;
  }

  .AtrasSiguiente {
  display: flex;
  flex-direction: row-reverse;
  margin: 2%;
  gap: 1%;
  justify-content: space-evenly;
}
}
</style>
