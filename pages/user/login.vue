<template>
  <section class="section">
    <div class="container">
      <div class="columns is-marginless is-centered">
        <div class="column is-6">
          <nav class="card">
            <header class="card-header">
              <p class="card-header-title">
                Login
              </p>
            </header>
            <div class="card-content">
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
                <button class="button is-primary" @click="login">Login</button>
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
        email: '',
        password: '',
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
    async login() {
      this.$auth.loginWith('local', {
        data: {
          email: this.form.email,
          password: this.form.password
        }
      }).catch(e=>{
        this.Errors = e.response.data.errors;
      })
    }
  }
}

</script>