<template>
  <div class="registerPage">
    <div class="container">
      <div class="row">
        <!-- <b-col col sm="4"></b-col> -->
        <b-col col md="6" offset-md="3">
          <div class="text-center">
            <nuxt-link to="/"><b-img src="/img/logo-black.png"></b-img></nuxt-link>
          </div>

          <b-form class="mt-3">
            <div class="a-box a-spacing-extra-large">
              <div class="a-box-inner">
                <h1 class="a-spacing-small">Create account</h1>
                <!-- Your Name -->
                <div class="a-row a-spacing-base">
                  <label for="ap_customer_name" class="a-form-label">Your name</label>
                  <input
                    type="text"
                    id="ap_customer_name"
                    v-model="name"
                    class="a-input-text form-control auth-atofocus auth-required-field auth-verification-request-info" />
                </div>
                <!-- Email -->
                <div class="a-row a-spacing-base">
                  <label for="ap_customer_email" class="a-form-label">Email</label>
                  <input
                    type="email"
                    id="ap_customer_email"
                    v-model="email"
                    class="a-input-text form-control auth-atofocus auth-required-field auth-verification-request-info" />
                </div>
                <!-- Password -->
                <div class="a-row a-spacing-base">
                  <label for="ap_customer_password" class="a-form-label">Password</label>
                  <input
                    type="password"
                    id="ap_customer_password"
                    v-model="password"
                    class="a-input-text form-control auth-atofocus auth-required-field auth-verification-request-info" />
                  <div class="a-alert-container pl-0">
                    <div class="a-alert-content">
                      Password must be at least 6 characters
                    </div>
                  </div>
                </div>
                <!-- Button -->
                <div class="a-row a-spacing-extra-large mb-4">
                  <span class="a-button-primary">
                    <span class="a-button-inner">
                      <span class="a-button-text" @click="onSignup">Create your Amazon account</span>
                    </span>
                  </span>
                  <div
                    class="a-row a-spacing-top-medium a-size-small text-center">
                    <b>
                      By creating an account, you agree to Amazon's
                      <a href="#">Conditions of use</a> and
                      <a href="#">Privacy notice</a>
                    </b>
                  </div>
                </div>
                <hr />
                <div class="a-row text-center">
                  <b>
                    Already have an account?
                    <nuxt-link to="/login" class="a-link-emphasis">Sign in</nuxt-link>
                  </b>
                </div>
              </div>
            </div>
          </b-form>
        </b-col>
      </div>
    </div>
  </div>
</template>

<script>
import infoToastMixin from "~/mixins/infoToast";
const apiUrl = process.env.BASE_URL || 'http://localhost:3010'

export default {
  layout: "admin",
  transition(to, from) {
    if (!from) {
      return "slide-left";
    }
    return "slide-right";
  },
  middleware: "isGuest",
  // auth: 'guest',
  // layout: 'none',
  head() {
    return {
      title: "Signup",
    };
  },
  data() {
    return {
      customerName: "",
      name: "",
      email: "",
      password: "",
    };
  },
  mixins: [infoToastMixin],
  methods: {
    async onSignup() {
      // debugger;
      try {
        let data = {
          name: this.name,
          email: this.email,
          password: this.password,
        };
        // debugger;
        let response = await this.$axios.$post(`${apiUrl}/auth/signup`, data);

        // console.log(response);

        if (response.success) {
          await this.$auth
            .loginWith("local", {
              data: {
                email: this.email,
                password: this.password,
              },
            })
            .then((_) => {
              this.$root.$bvToast.toast(`Welcome ${this.name}`, {
                title: `Login`,
                variant: "success",
                autoHideDelay: 2000,
                solid: true,
              });
              this.$router.push("/");
            });
        } else {
          // console.log(response);
          this.$bvToast.toast(`${response.message}`, {
            title: `Signup Error`,
            variant: "danger",
            solid: true,
          });
        }
      } catch (err) { }
    },
  },
};
</script>

<style lang="scss" scoped></style>
