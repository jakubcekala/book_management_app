<template>
  <v-app class="grey lighten-3">
    <Appbar/>
   <v-content class="grey lighten-5">
     <router-view></router-view>
   </v-content>
  </v-app>
</template>

<script>
import Appbar from "@/components/Appbar"

export default {
  name: 'App',
  components: {Appbar},
  comments: {Appbar},
  data: () => ({
    //
  }),
};
</script>
