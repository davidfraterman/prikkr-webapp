<template>
  <section class="container">
    <h1 v-if="this.mode === 'login'">Inloggen</h1>
    <h1 v-else>Registreren</h1>
    <form @submit.prevent="submitForm">
      <div class="form-control">
        <label for="email">E-mail Adres</label>
        <input
          type="email"
          name="email"
          id="email"
          autocomplete="on"
          v-model="email"
        />
      </div>
      <div class="form-control">
        <label for="password">Wachtwoord</label>
        <input
          type="password"
          name="password"
          id="password"
          autocomplete="on"
          v-model="password"
        />
      </div>
      <p class="error" v-if="!formIsValid">
        Voer alstublieft een geldig email-adres en wachtwoord in
      </p>
      <base-button class="btn">{{ submitButtonCaption }}</base-button>
      <base-button
        type="button"
        class="btn"
        mode="secondary"
        @click="switchAuthMode"
        >{{ switchLoginOrSignup }}</base-button
      >
    </form>
    <br />
    <p style="color: white; font-size: 12px">
      Voorbeeld account: <br /><br />email: voor@beeld.com <br />
      password: voorbeeld
    </p>
  </section>
</template>

<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      formIsValid: true,
      mode: "login",
    };
  },
  computed: {
    submitButtonCaption() {
      if (this.mode === "signup") {
        return "Registreer";
      } else {
        return "Log in";
      }
    },
    switchLoginOrSignup() {
      if (this.mode === "login") {
        return "Registreren?";
      } else {
        return "Inloggen?";
      }
    },
  },
  methods: {
    async submitForm() {
      this.formIsValid = true;
      if (
        this.email === "" ||
        !this.email.includes("@") ||
        this.password.length < 6
      ) {
        this.formIsValid = false;
      } else {
        try {
          if (this.mode === "login") {
            await this.$store.dispatch("login", {
              email: this.email,
              password: this.password,
            });
          } else {
            await this.$store.dispatch("signup", {
              email: this.email,
              password: this.password,
            });
          }
        } catch (err) {
          console.log(err);
        }

        const url = "/dashboard";
        this.$router.replace(url);
      }
    },
    switchAuthMode() {
      if (this.mode === "login") {
        this.mode = "signup";
      } else {
        this.mode = "login";
      }
    },
  },
};
</script>

<style scoped>
.container {
  padding: 1rem;
  margin: 2rem auto;
  margin-top: 10vh;
  max-width: 25vw;
}
form {
  margin-top: 3rem;
}
label {
  margin-bottom: 0.25rem;
  display: block;
  color: rgb(255, 255, 255);
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  border: 2px solid #24b4d3;
  background-color: rgb(61, 61, 61);
  color: white;
  padding: 0.15rem;
  margin-bottom: 1rem;
  border-radius: 6px;
}

input:focus,
textarea:focus {
  border-color: #24b4d3;
  background-color: rgb(75, 75, 75);
  outline: none;
}

.btn {
  margin-top: 2rem;
  margin-right: 1rem;
}

h1 {
  margin-bottom: 3rem;
}
.error {
  color: rgb(255, 67, 67);
}

@media only screen and (max-width: 1400px) {
  .container {
    max-width: 50vw;
  }
}

@media only screen and (max-width: 900px) {
  .container {
    max-width: 70vw;
  }
}

@media only screen and (max-width: 700px) {
  .container {
    max-width: 90vw;
  }
}
</style>