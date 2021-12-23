<template>
  <div class="h-full d-flex center">
    <b-card body-class="max-w-350">
      <span class="title"> Welcome </span>
      <img class="heart-img" :src="images.heart" />
      <b-form class="px-15" @submit="onSubmit" @reset="onReset">
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
              required
            ></b-form-input>
          </b-input-group>
        </b-form-group>

        <template v-if="signUp !== true">
          <b-form-group id="input-group-2" label-for="input-2">
            <b-input-group class="mb-2">
              <b-input-group-prepend is-text>
                <b-icon icon="lock"></b-icon>
              </b-input-group-prepend>
              <b-form-input
                id="input-2"
                type="password"
                v-model="form.password"
                placeholder="Enter password"
                required
              ></b-form-input>
            </b-input-group>
          </b-form-group>
        </template>
        <template v-else>
          <b-form-group id="input-group-2" label-for="input-2">
            <b-input-group class="mb-2">
              <b-input-group-prepend is-text>
                <b-icon icon="lock"></b-icon>
              </b-input-group-prepend>
              <b-form-input
                id="input-2"
                type="password"
                v-model="form.password"
                placeholder="Enter password"
                @input="validatePassword"
                required
              ></b-form-input>
            </b-input-group>
            <b-tooltip target="input-2" title="Tooltip title" triggers="focus">
              <div :class="length ? green : red">
                At least 8 characters in length
              </div>
              <div :class="lowerCase ? green : red">
                Contain Lower case latters (a-z)
              </div>
              <div :class="upperCase ? green : red">
                Contain Upper case latters (A-Z)
              </div>
              <div :class="number ? green : red">
                Contain Numbers (i.e. 0.9)
              </div>
              <div :class="specialChar ? green : red">
                Contain Special characters (e.g. !@#$%^&*)
              </div>
            </b-tooltip>
          </b-form-group>
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
            <b-button size="sm" variant="success" @click="signUp = true"
              >Sign up</b-button
            >
          </div>
        </template>
        <template v-else>
          <b-button type="submit" variant="primary">Sign up</b-button>
          <div class="grey py-20">
            Already have an account?
            <b-button size="sm" variant="success" @click="signUp = false"
              >Log in</b-button
            >
          </div>
        </template>
      </b-form>
    </b-card>
    <!-- <b-card>
      <span class="title"> Welcome </span>
      <img class="heart-img" :src="images.heart" />
      <b-form class="px-15" @submit="onSubmit" @reset="onReset">
        <b-card-text>
          Create an acount to enter John's secret area
        </b-card-text>
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
              required
            ></b-form-input>
          </b-input-group>
        </b-form-group>

        <b-form-group id="input-group-2" label-for="input-2">
          <b-input-group class="mb-2">
            <b-input-group-prepend is-text>
              <b-icon icon="lock"></b-icon>
            </b-input-group-prepend>
            <b-form-input
              id="input-2"
              type="password"
              v-model="form.password"
              placeholder="Enter password"
              @input="validatePassword"
              required
            ></b-form-input>
          </b-input-group>
          <b-tooltip target="input-2" title="Tooltip title" triggers="focus">
            <div :class="length ? green : red">
              At least 8 characters in length
            </div>
            <div :class="lowerCase ? green : red">
              Contain Lower case latters (a-z)
            </div>
            <div :class="upperCase ? green : red">
              Contain Upper case latters (A-Z)
            </div>
            <div :class="number ? green : red">Contain Numbers (i.e. 0.9)</div>
            <div :class="specialChar ? green : red">
              Contain Special characters (e.g. !@#$%^&*)
            </div>
          </b-tooltip>
        </b-form-group>

        <b-form-group id="input-group-3" label-for="input-3">
          <b-input-group class="mb-2">
            <b-input-group-prepend is-text>
              <b-icon icon="lock"></b-icon>
            </b-input-group-prepend>
            <b-form-input
              id="input-3"
              type="password"
              v-model="form.confirmPassword"
              placeholder="Confirm password"
              required
            ></b-form-input>
          </b-input-group>
        </b-form-group>

        <b-button type="submit" variant="primary">Sign up</b-button>
        <div class="grey py-20">
          Already have an account?
          <router-link to="#"><span>Log in</span></router-link>
        </div>
      </b-form>
    </b-card> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      signUp: false,
      length: false,
      lowerCase: false,
      upperCase: false,
      number: false,
      specialChar: false,
      green: "green",
      red: "red",
      validPassword: false,
      form: {
        email: "",
        name: "",
        food: null,
        checked: [],
      },
      foods: [
        { text: "Select One", value: null },
        "Carrots",
        "Beans",
        "Tomatoes",
        "Corn",
      ],
      show: true,
      images: {
        heart: require("../assets/images/heart.webp"),
      },
    };
  },
  methods: {
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
        console.log("this.validPassword", this.validPassword);
      }
    },
    onSubmit(event) {
      event.preventDefault();
      alert(JSON.stringify(this.form));
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      this.form.email = "";
      this.form.name = "";
      this.form.food = null;
      this.form.checked = [];
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
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
</style>
