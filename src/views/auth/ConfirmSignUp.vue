<template>
  <b-container>
    <b-row class="justify-content-md-center">
      <b-col>
        <div class="b-form-1">
          <h2>Confirm Sign Up</h2>
          <p>Please check your email for the account verification code to continue.</p>
          <b-input-group prepend="Email">
            <b-form-input 
              id="emailInput"
              type="text"
              autocomplete="email"
              v-model="email"
              required
              autofocus
              placeholder="jdoe@example.com"/>
          </b-input-group>
          <br>
          <b-input-group prepend="Code">
            <b-form-input 
              id="codeInput"
              type="text"
              v-model="code"
              required
              placeholder="XXXXXX"/>
          </b-input-group>
          <br>
          <b-button-group>
            <b-button 
              type="submit" 
              variant="primary" 
              @click="confirmSignUp">Submit</b-button>
            <b-button 
              variant="outline-secondary" 
              @click="confirmResend">Resend Code</b-button>
          </b-button-group>
        </div>
      </b-col>
    </b-row>
    <b-row class="justify-content-md-center">
      <b-col>
        <v-alert/>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import Vue from "vue"
import { mapGetters } from "vuex"

Vue.component("v-alert", function (resolve) {
  require(['@/components/auth/Alert.vue'], resolve)
});


export default {
  data() {
    return {
      email: "",
      code: "",
    }
  },
  computed: {
    ...mapGetters("auth", ["hasAuthenticationStatus"])
  },
  methods: {
    async confirmSignUp() {
      await this.$store.dispatch("auth/confirmSignUp", {
        username: this.email,
        code: this.code
      })
      if (!this.hasAuthenticationStatus) {
        this.$router.push({ name: 'signIn' })
      }
    },
    async confirmResend() {
      await this.$store.dispatch("auth/confirmResend", {
        email: this.email
      })
    }
  }
}
</script>
