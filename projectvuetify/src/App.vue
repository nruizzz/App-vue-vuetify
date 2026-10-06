<!-- Aquest arxiu serveix per a configurar l'aplicació principal de Vuetify -->


<script setup>
  import { ref } from 'vue';


  const drawer = ref(false);
  const items = [
    { title: 'Home', icon: 'mdi-home', route: '/' },
    { title: 'Columnes', icon: 'mdi-pillar', route: '/columnes' },
    { title: 'Fitxa', icon: 'mdi-card-account-mail', route: '/fitxa' },
  ];

</script>
<template>

  <v-app>

    <!-- Navigation Drawer -->
    <v-navigation-drawer v-model="drawer" app>
      <v-list>

      <!--Això es un bucle que recorre l'array items i crea un element de llista per a cada item.-->
      <!--Cada element de llista té un enllaç a la ruta corresponent i mostra el títol i la icona de l'item.-->

        <v-list-item 
          v-for="(item, index) in items"
          :key="index"
          :title="item.title"
          :prepend-icon="item.icon"
          :href="item.route"
          link
          @click="drawer = false"
        />
      </v-list>
    </v-navigation-drawer>


    <!-- Això és una barra de navegació -->
    <v-toolbar color="purple" app> <!-- Crea una barra de navegació a la part superior de l'aplicació amb un color blau.-->
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Navegatooor</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon> 
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon>mdi-heart</v-icon>
      </v-btn>
      <v-btn icon>
 <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-toolbar>


    <!-- Main Content -->
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>
