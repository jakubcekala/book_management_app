<template>
  <nav>
    <v-app-bar flat app class="text-uppercase black--text">
      <v-app-bar-nav-icon @click="drawer = !drawer">
      </v-app-bar-nav-icon>
      <v-toolbar-title>
        <span>Books Management System</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <span>Jakub Cekała (277391)</span>
    </v-app-bar>

    <v-navigation-drawer app v-model="drawer">
      <v-list>
        <v-list-item v-for="link in links" :key="link.text" router :to="link.route">
          <v-list-item-icon>
            <v-icon v-text="link.icon"></v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>{{ link.text }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      drawer: false,
      links: [
        { icon: 'dashboard', text: 'Books management', route: '/'},
        { icon: 'info', text: 'About', route: '/about'},
      ]
    }
  }
}
</script>
