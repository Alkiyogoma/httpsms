<template>
  <v-container fluid class="pa-0" :fill-height="$vuetify.breakpoint.lgAndUp">
    <v-row v-if="$vuetify.breakpoint.lgAndUp" align="center" justify="center">
      <div>
        <v-img
          class="mx-auto mb-4"
          max-height="400"
          max-width="90%"
          contain
          :src="require('assets/img/person-texting.svg')"
        ></v-img>
        <div class="text-center">
          <h3 class="text-h5 mt-4">Select a thread</h3>
          <p class="text--secondary">Send and receive messages using our API</p>
        </div>
      </div>
    </v-row>
    <v-row v-else justify="end">
      <v-col class="px-0 py-0">
        <message-thread-header></message-thread-header>
        <message-thread></message-thread>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'ThreadsIndex',
  middleware: ['auth'],
  head() {
    return {
      title: 'Threads - Http SMS',
    }
  },
  async mounted() {
    if (!this.$store.getters.getAuthUser) {
      await this.$store.dispatch('setNextRoute', this.$route.path)
      await this.$router.push({ name: 'index' })
      setTimeout(this.loadData, 2000)
      return
    }
    await this.loadData()
  },

  methods: {
    async loadData() {
      await this.$store.dispatch('loadThreads')
      await this.$store.dispatch('loadPhones')
    },
  },
}
</script>
