<template>
  <div class="h-full d-flex center">
    <b-card body-class="max-w-350">
      <span class="title"> Welcome </span>
      <img class="heart-img" :src="images.heart" />
      <b-form novalidate class="px-15" @submit="onSubmit" @reset="onReset">
        <template v-if="signUp !== true">
          <b-card-text> Log in to John's secret area to continue </b-card-text>
        </template>
        <template v-else>
          <b-card-text>
            Create an acount to enter John's secret area
          </b-card-text>
        </template>
        <b-form-group id="input-group-1" label-for="input-1">
          <b-input-group class="mb-2">
            <b-input-group-prepend is-text>
              <b-icon icon="person-fill"></b-icon>
            </b-input-group-prepend>
            <b-form-input
              id="input-1"
              v-model="form.email"
              type="email"
              placeholder="Enter email"
              @input="validateEmail"
              :state="emailState"
              required
            ></b-form-input>
          </b-input-group>
        </b-form-group>

        <b-form-group id="input-group-2" label-for="input-2">
          <b-input-group class="mb-2">
            <b-input-group-prepend is-text>
              <b-icon icon="lock"></b-icon>
            </b-input-group-prepend>
            <template v-if="signUp !== true">
              <b-form-input
                id="input-2"
                :type="inputType"
                v-model="form.password"
                :state="passwordState"
                placeholder="Enter password"
                required
              ></b-form-input>
            </template>
            <template v-else>
              <b-form-input
                id="input-2"
                :type="inputType"
                v-model="form.password"
                placeholder="Enter password"
                :state="validPassword"
                @input="validatePassword"
                required
              ></b-form-input>
            </template>
            <b-input-group-append>
              <b-button @click="togglePassword" size="sm" text="Button">{{
                message
              }}</b-button>
            </b-input-group-append>
          </b-input-group>
        </b-form-group>
        <template v-if="signUp === true">
          <b-tooltip
            :show.sync="show"
            target="input-2"
            title="Tooltip title"
            triggers="focus"
          >
            <div :class="`flex flex-start ${length ? green : red}`">
              <span class="icon"
                ><b-icon
                  :icon="`${length ? 'check-circle' : 'backspace-reverse'}`"
                ></b-icon
              ></span>
              least 8 characters in length
            </div>
            <div :class="`flex flex-start ${lowerCase ? green : red}`">
              <span class="icon"
                ><b-icon
                  :icon="`${lowerCase ? 'check-circle' : 'backspace-reverse'}`"
                ></b-icon
              ></span>
              Contain Lower case latters (a-z)
            </div>
            <div :class="`flex flex-start ${upperCase ? green : red}`">
              <span class="icon"
                ><b-icon
                  :icon="`${upperCase ? 'check-circle' : 'backspace-reverse'}`"
                ></b-icon
              ></span>
              Contain Upper case latters (A-Z)
            </div>
            <div :class="`flex flex-start ${number ? green : red}`">
              <span class="icon"
                ><b-icon
                  :icon="`${number ? 'check-circle' : 'backspace-reverse'}`"
                ></b-icon
              ></span>
              Contain Numbers (i.e. 0.9)
            </div>
            <div :class="`flex flex-start ${specialChar ? green : red}`">
              <span class="icon"
                ><b-icon
                  :icon="`${
                    specialChar ? 'check-circle' : 'backspace-reverse'
                  }`"
                ></b-icon
              ></span>
              Contain Special characters (e.g. !@#$%^&*)
            </div>
          </b-tooltip>
        </template>

        <template v-if="signUp === true">
          <b-form-group id="input-group-3" label-for="input-3">
            <b-input-group class="mb-2">
              <b-input-group-prepend is-text>
                <b-icon icon="lock"></b-icon>
              </b-input-group-prepend>
              <b-form-input
                id="input-3"
                type="password"
                v-model="form.confirmPassword"
                :state="confirmPasswordState"
                placeholder="Confirm password"
                required
              ></b-form-input>
            </b-input-group>
          </b-form-group>
        </template>

        <template v-if="signUp !== true">
          <b-button type="submit" variant="primary">Log in</b-button>
          <div class="grey py-20">
            Don't have an account?
            <b-button size="sm" variant="success" @click="switchPage"
              >Sign up</b-button
            >
          </div>
        </template>
        <template v-else>
          <b-button type="submit" variant="primary">Sign up</b-button>
          <div class="grey py-20">
            Already have an account?
            <b-button size="sm" variant="success" @click="switchPage"
              >Log in</b-button
            >
          </div>
        </template>
        <div class="horizontal"></div>
        <b-form-group>
          <div class="my-10">
            <b-button
              target="_self"
              :href="googleOauth2"
              class="hover w-280 text-align-left"
              variant="outline-primary"
              ><img class="social-icon" :src="images.google" />
              Continue with Google
            </b-button>
          </div>
          <div class="my-10">
            <b-button
              target="_blank"
              :href="facebook"
              class="hover w-280 text-align-left"
              variant="outline-primary"
              ><img class="social-icon" :src="images.facebook" />
              Continue with Facebook
            </b-button>
          </div>
        </b-form-group>
      </b-form>
    </b-card>
  </div>
</template>

<script>
export default {
  data() {
    const authUrl = "https://john0924.us.auth0.com/authorize?";
    const responseType = "token";
    const clientId = "hzCIVCYEa29xikYvkQGUq2TfCqMvK1Fy";
    const connections = { googleOauth: "google-oauth2", facebook: "facebook" };
    const redirectUri = "http://localhost:8080/";
    const googleOauth2 = `${authUrl}response_type=${responseType}&client_id=${clientId}&connection=${connections.googleOauth}&redirect_uri=${redirectUri}`;
    const facebook = `${authUrl}response_type=${responseType}&client_id=${clientId}&connection=${connections.facebook}&redirect_uri=${redirectUri}`;
    return {
      emailState: null,
      passwordState: null,
      confirmPasswordState: null,
      inputType: "password",
      message: "show",
      facebook,
      googleOauth2,
      signUp: false,
      length: false,
      lowerCase: false,
      upperCase: false,
      number: false,
      specialChar: false,
      green: "green",
      red: "red",
      validPassword: null,
      form: {
        email: "",
        password: "",
        confirmPassword: "",
      },
      show: false,
      images: {
        heart: require("../assets/images/heart.webp"),
        google: require("../assets/images/google.png"),
        facebook: require("../assets/images/facebook.png"),
      },
    };
  },
  methods: {
    isPasswordMatched() {
      if (
        this.form.confirmPassword &&
        this.form.password === this.form.confirmPassword
      ) {
        this.confirmPasswordState = true;
      } else {
        this.confirmPasswordState = false;
      }
      return this.confirmPasswordState;
    },
    checkEmpty() {
      if (!this.form.password.length) return (this.passwordState = false);
      return (this.passwordState = true);
    },
    validateEmail() {
      const regex = /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/;
      if (regex.test(this.form.email)) return (this.emailState = true);
      return (this.emailState = false);
    },
    switchPage() {
      this.signUp = !this.signUp;
      this.reSet();
    },
    reSet() {
      this.form.email = "";
      this.form.password = "";
      this.form.confirmPassword = "";
      this.inputType = "password";
      this.message = "Show";
      this.emailState = null;
      this.passwordState = null;
      this.validPassword = null;
      this.confirmPasswordState = null;
    },
    togglePassword() {
      if (this.inputType === "password") {
        this.inputType = "text";
        this.message = "Hide";
      } else {
        this.inputType = "password";
        this.message = "Show";
      }
    },
    showTooltip() {
      if (this.validPassword) {
        this.show = false;
      } else {
        this.show = true;
      }
    },
    validatePassword(str) {
      let count = 0;
      if (str.length >= 8) {
        this.length = true;
        count++;
      } else {
        this.length = false;
        count--;
      }
      if (/[a-z]/.test(str)) {
        this.lowerCase = true;
        count++;
      } else {
        this.lowerCase = false;
        count--;
      }
      if (/[A-Z]/.test(str)) {
        this.upperCase = true;
        count++;
      } else {
        this.upperCase = false;
        count--;
      }
      if (/[0-9]/.test(str)) {
        this.number = true;
        count++;
      } else {
        this.number = false;
        count--;
      }
      if (/[^A-Za-z0-9]/.test(str)) {
        this.specialChar = true;
        count++;
      } else {
        this.specialChar = false;
        count--;
      }
      if (count === 5) {
        this.validPassword = true;
      } else {
        this.validPassword = false;
      }
      this.showTooltip();
    },
    login() {
      const email = this.validateEmail();
      const password = this.checkEmpty();
      if (!(email && password)) {
        console.log("something went wrong");
        return false;
      }
      console.log("login success");
      return true;
    },
    signup() {
      const email = this.validateEmail();
      const password = this.validatePassword(this.form.password);
      const isPasswordMatched = this.isPasswordMatched();
      if (!(email && password && isPasswordMatched)) {
        console.log("something went wrong");
        return false;
      }
      console.log("sign up success");
      return true;
    },
    onSubmit(event) {
      event.preventDefault();
      if (!this.signUp) {
        this.login();
      } else {
        this.signup();
      }
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      this.form.email = "";
      this.form.password = "";
      this.form.confirmPassword = null;
    },
  },
};
</script>

<style scoped>
.grey {
  color: rgb(121, 127, 139);
}
.px-15 {
  padding-left: 15px;
  padding-right: 15px;
}
.py-20 {
  padding-top: 15px;
  padding-bottom: 15px;
}
.h-full {
  height: 100vh;
}
.center {
  justify-content: center;
  align-items: center;
}
.heart-img {
  width: 100px;
}
.title {
  display: block;
  font-size: 1.5rem;
}
.green {
  color: #7fff00;
}
.red {
  color: #ff6347;
}
.max-w-350 {
  max-width: 350px;
}
.icon {
  display: inline-block;
  width: 2rem;
}
.flex {
  display: flex;
}
.flex-start {
  justify-content: flex-start;
}
.text {
  color: white;
}
.social-icon {
  max-width: 25px;
  margin-right: 15px;
}
.btn-outline-primary.hover:hover {
  color: #fff;
  background-color: #2b8ff9;
  border-color: #007bff;
}
.w-280 {
  min-width: 280px;
}
.text-align-left {
  text-align: left;
}
.my-10 {
  margin-top: 10px;
  margin-bottom: 10px;
}
.horizontal {
  align-items: center;
  border-bottom: 1px solid #dadde1;
  display: flex;
  margin-bottom: 15px;
  text-align: center;
}
</style>
