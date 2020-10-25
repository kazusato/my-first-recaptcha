<template>
  <section class="index-page">
    <div>
      v2
      <nuxt-link to="/">v3</nuxt-link>
    </div>

    <v-row>
      <v-col><h2>Sign In</h2></v-col>
    </v-row>

    <v-form @submit.prevent="onSubmitV2">
      <v-row>
        <v-col cols="3">Action:</v-col>
        <v-col cols="5">
          <v-select :items="items" v-model="type"/>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="3">Mail:</v-col>
        <v-col cols="5">
          <v-text-field
            autocomplete="true"
            placeholder="Email"
            type="email"
            v-model="email"
          />
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="3">Password:</v-col>
        <v-col cols="5">
          <v-text-field
            autocomplete="current-password"
            placeholder="Password"
            type="password"
            v-model="password"
          />
        </v-col>
      </v-row>

      <recaptcha
        @error="onError"
        @success="onSuccess"
        @expired="onExpired"
      />

      <v-row>
        <v-col cols="3">
          <v-btn type="submit" color="primary">Sign In</v-btn>
        </v-col>
      </v-row>

      <small>
        This site is protected by reCAPTCHA and the Google
          <a href="https://policies.google.com/privacy">Privacy Policy</a> and
          <a href="https://policies.google.com/terms">Terms of Service</a> apply.
      </small>
    </v-form>
  </section>
</template>

<script>
export default {
  data: () => ({
    email: 'test@example.com',
    password: '123',
    type: 'login',
    items: [
      'login',
      'purchase',
    ]
  }),

  async mounted() {
    try {
      await this.$recaptcha.init()
    } catch (e) {
      console.log(e);
    }
  },

  methods: {
    onError (error) {
      console.log('Error happened:', error)
    },
    async onSubmitV2() {
      try {
        const token = await this.$recaptcha.getResponse()
        console.log('ReCaptcha token:', token)
        await this.$recaptcha.reset()
      } catch (error) {
        console.log('Login error:', error)
      }
    },
    onSuccess (token) {
      console.log('Succeeded:', token)
    },
    onExpired () {
      console.log('Expired')
    }
  },
}
</script>
