<!-- Portada del Gestor de Tasques -->

<script setup>

import { ref, onMounted, watch } from 'vue'

const tasques = ref([]) //Array buit que contindrá les tasques que l'usuari afegirà a la llista.

//Control de finestras emergents.
const dialogForm = ref(false)
const dialogDelete = ref(false)
const esEdicio = ref(false)

//Index per saber qué element modificar o esborrar.
const indexEdicio = ref(-1)
const indexEliminar = ref(-1)

//Objecte que contindrá les dades del formulari de la tasca.
const formTasca = ref({
  titol: '',
  descripcio: '',
  completada: false //Per defecte, la tasca no està completada quan es crea, per això s'inicialitza a false.
})

//LocalStorage guarda text i conserva les dades encara que es tanqui o actualitzi la pàgina.
onMounted(() => { 
  const dadesDesat = localStorage.getItem('tasques')
  if (dadesDesat) { //Comprovació de dades desades.
    tasques.value = JSON.parse(dadesDesat) //Es converteix de JSON a JavaScript i s'accedeix o modifica el seu contingut mitjançant .value.
  }
})

watch(tasques, (novesTasques) => { //Desat automàtic quan canvia qualsevol dada.
  localStorage.setItem('tasques', JSON.stringify(novesTasques)) //Guardar l'array de tasques convertit a text.
}, { deep: true })


const obrirFormulariNova = () => { //Funció per defecte, quan es vol afegir una nova tasca. 
  esEdicio.value = false 
  formTasca.value = { 
    titol: "",
    descripcio: "",
    completada: false 
  }
  dialogForm.value = true //Mostra la finestra emergent del formulari.
}

const obrirFormulariEditar = (index) => { //Funció per editar una tasca existent.
  esEdicio.value = true 
  indexEdicio.value = index
  formTasca.value = { ...tasques.value[index]
  }
  dialogForm.value = true 
}

const desarTasca = () => { //Funció per desar una tasca nova o editada.
  if (!formTasca.value.titol.trim()) return //Comprovació que el títol no estigui buit.

  if (esEdicio.value) { //Si s'està editant, es modifica la tasca existent.
    tasques.value[indexEdicio.value] = {...formTasca.value } 
  } else { //Si no, es crea una nova tasca.
    tasques.value.push({...formTasca.value })
  }
  dialogForm.value = false
}

//Funció per eliminar. S'obre un diàleg de confirmació abans d'eliminar la tasca.
const confirmarEliminar = (index) => { 
  indexEliminar.value = index
  dialogDelete.value = true
}

const eliminarTasca = () => { //Funció per eliminar la tasca seleccionada.
  if (indexEliminar.value !== -1) { //Comprovació que s'ha seleccionat una tasca per eliminar.
    tasques.value.splice(indexEliminar.value, 1) //Elimina la tasca de l'array.
  }
  dialogDelete.value = false //Tanca el diàleg de confirmació.
}

</script>

<template>
  <v-container> <!--Contenidor principal de la pàgina-->
    <v-row class="mb-4 align-center" justify="space-between"> 
      <v-col cols="12" sm="6">
        <h1 class="text-h4">Gestor de Tasques</h1>
      </v-col>
      <v-col cols="12" sm="6" class="text-sm-right">
        <v-btn color="primary" @click="obrirFormulariNova">
          Nova Tasca
        </v-btn>
        </v-col>
      </v-row>

    <!-- Llistar Tasques -->
     <v-card v-if="tasques.length > 0"> <!--Comprovació que hi hagi tasques per mostrar.-->
      <v-list lines="two"> 
        <template v-for="(tasca, index) in tasques" :key="index"> <!-- Iteració sobre les tasques -->
          <v-list-item>
            <!-- Estat (pendent o completada) -->
            <template #prepend>
              <v-checkbox-btn v-model="tasca.completada"></v-checkbox-btn>
            </template>
    
            <!-- Títol -->
            <v-list-item-title :class="{ 'text-decoration-line-through': tasca.completada }">
              {{ tasca.titol }}
            </v-list-item-title>

            <!-- Descripció -->
            <v-list-item-subtitle>
              {{ tasca.descripcio }}
            </v-list-item-subtitle>

            <!-- Opcions d'editar i eliminar -->
            <template #append>
              <v-btn icon variant="text" @click="obrirFormulariEditar(index)">
                <v-icon>mdi-pencil</v-icon>
              </v-btn>
              <v-btn icon variant="text" color="error" @click="confirmarEliminar(index)">
                <v-icon>mdi-delete</v-icon>
              </v-btn>
            </template>
          </v-list-item>
          <v-divider v-if="index < tasques.length - 1"></v-divider>
        </template>
      </v-list>
    </v-card>

    <v-alert v-else type="info" variant="tonal" class="mt-4">
      No hi ha cap tasca.
    </v-alert>

    <!-- Formulari modal per afegir / editar -->
    <v-dialog v-model="dialogForm" max-width="500px">
      <v-card>
        <v-card-title>{{ esEdicio ? 'Editar Tasca' : 'Nova Tasca' }}</v-card-title>
        <v-card-text>
          <v-text-field
            v-model="formTasca.titol"
            label="Títol"
            required
            :rules="[v => !!v || 'El títol és obligatori']"
          ></v-text-field>

          <v-textarea
            v-model="formTasca.descripcio"
            label="Descripció (opcional)"
          ></v-textarea>

          <v-switch
            v-model="formTasca.completada"
            label="Completada"
            color="success"
          ></v-switch>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn variant="text" @click="dialogForm = false">Cancel·lar</v-btn>
          <v-btn color="primary" @click="desarTasca">Desar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Diàleg per confirmar eliminació -->
    <v-dialog v-model="dialogDelete" max-width="400px">
      <v-card>
        <v-card-title class="text-h6">Eliminar Tasca</v-card-title>
        <v-card-text>Vols eliminar aquesta tasca?</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn variant="text" @click="dialogDelete = false">Cancel·lar</v-btn>
          <v-btn color="error" @click="eliminarTasca">Eliminar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>