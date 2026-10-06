<!-- Pàgina de la targeta de contacte-->

<script setup>
import { ref } from 'vue' //ref(...): Crea variables reactives.
                          //Significa que si en algún moment canviem el contingut de tel o nom, la pantalla s'actualitzará automáticament sense necessitat de recargar la pàgina.


const companyia = ref("INS CASTELLET") //Guardem les dades que desprès es mostrarán dinámicament al HTML utilitzant la sintaxis de claus dobles {{ variable }.
const nom = ref("Natalia Ruiz")
const tel = ref("8756312903")
const mmail = ref("nataliaruizcorreas@gmail.com")

</script>

<template>
  <v-container> <!-- Centra el contingut a meitat de la pantalla i afegueix marges de seguretat als laterals.-->
    <v-card color="blue"> <!-- Crea una targeta amb bordes rodons i una hombra. L'atribut color="blue" li dona el fonç blau.-->

      <v-row class="ma-1 pa-5"> <!-- <v-row>: Crea una fila horitzontal per a organitzar el contingut en columnes.-->
                                <!-- ma-1 -> Aplica un petit marge exterior al voltant de la fila.-->
                                <!-- pa-5 -> Afegueix un padding interior perque el contingut no quedi pegat als bordes de la targeta blau.-->


        <v-col cols="6"> <!-- Al sistema de 6 columnes de Vuetify, divideix la pantalla a la meitat per a tenir dues zones de contingut.-->
        <v-img
            src="https://img.freepik.com/free-vector/bird-colorful-logo-gradient-vector_343694-1365.jpg?t=st=1730029690~exp=1730033290~hmac=d3ebc9f71b5615696033dbe93fdc70c240fab2b9e9512971be97ae787655a2b6&w=826"
          />
          <h1 class="text-center">{{ companyia }}</h1> <!-- Centra el text del <h1> que mostra la variable companyia ("INS CASTELLET").-->
        </v-col>
        <v-col class="d-flex flex-column justify-center align-center" cols="6">
          <div class="d-flex pt-4">
            <h1>{{ nom }}</h1>
          </div>
          <div class="d-flex pt-4"> <!-- (Padding top level 4) -Dona un espai superior de separació a cada bloc de contacte perque no quedin pegats entre sí.-->
            <v-icon class="mr-2">mdi-phone</v-icon>
            <p>{{ tel }}</p>
          </div>
          <div class="d-flex pt-4">
            <v-icon class="mr-2">mdi-email</v-icon>
            <p>{{ mmail }}</p> 
          </div>
        </v-col>
      </v-row>
    </v-card>
  </v-container>
</template>