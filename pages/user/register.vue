<template>
  <section class="section">
    <div class="container">
      <div class="columns is-marginless is-centered">
        <div class="column is-6">
          <nav class="card">
            <header class="card-header">
              <p class="card-header-title">
                Register
              </p>
            </header>
            <div class="card-content">
              <div class="field">
                <label class="label">Name</label>
                <div class="control">
                  <input class="input" type="text" v-model="form.full_name" :class="{ 'is-danger' : errors('full_name') }" required>
                </div>
                <div class="help is-danger" v-text="messages('full_name')"></div>
              </div>
              <div class="field">
                <label class="label">Email</label>
                <div class="control">
                  <input class="input" type="text" v-model="form.email" :class="{ 'is-danger' : errors('email') }" required>
                </div>
                <div class="help is-danger" v-text="messages('email')"></div>
              </div>
              <div class="field">
                <label class="label">Password</label>
                <div class="control">
                  <input class="input" type="password" v-model="form.password" :class="{ 'is-danger' : errors('password') }" required>
                </div>
                <div class="help is-danger" v-text="messages('password')"></div>
              </div>
              <div class="field">
                <label class="label">Password Verify</label>
                <div class="control">
                  <input class="input" type="password" v-model="form.password_confirm" :class="{ 'is-danger' : errors('password_confirm') }" required>
                </div>
                <div class="help is-danger" v-text="messages('password_confirm')"></div>
              </div>
              <div class="field">
                <button class="button is-primary" @click="submit">Register</button>
              </div>
            </div>
          </nav>
        </div>
      </div>
    </div>
  </section>
</template>
<script>
export default {
  middleware: 'auth',
  auth: 'guest',
  data() {
    return {
      form: {
        full_name: '',
        email: '',
        password: '',
        password_confirm: '',
      },
      Errors: {}
    }
  },
  methods: {
    errors(w) {
      return this.Errors[w] ? true : false;
    },
    messages(w) {
      if (this.Errors && this.Errors[w]) {
        return this.Errors[w]['msg']
      }
    },
    submit() {
      this.$axios.post('/api/users/register', this.form)
        .then((response) => {
          console.log(response)
          if (response.data._id) {
            this.$router.push({ full_name: 'user-login', params: { registered: 'yes' } })
            // log in if successfully registered
            this.$auth.loginWith('local', {
                data: {
                  email: this.form.email,
                  password: this.form.password
                }
              })
              .catch((error) => {
                console.log(error)
              })
          }
        })
        .catch((error) => {
          console.log(error)
          if (error.response.data.errors) {
            this.errors = error.response.data.errors
          }
        });
    }
  }
}

</script>
