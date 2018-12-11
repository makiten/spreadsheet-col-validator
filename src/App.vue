<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Validate Your Spreadsheet</span>
        <span class="font-weight-light">MATERIAL DESIGN</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        flat
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
      >
        <span class="mr-2">Latest Release</span>
      </v-btn>
    </v-toolbar>

    <v-content>
      <demo-form/>
    </v-content>
  </v-app>
</template>

<script>
// import HelloWorld from './components/HelloWorld'
import DemoForm from './components/DemoForm'

export default {
  name: 'App',
  components: {
    // HelloWorld,
    DemoForm
  },
  data () {
    return {
      //
    }
  }
}
</script>
