<template>
  <form>
    <div class="mb-3">
      <label for="email">Correo electrónico</label>
      <input
        type="email"
        name="email"
        id="email"
        class="form-control"
        required="required"
        v-model="form.email"
      />
    </div>

    <div class="mb-3">
      <label for="password">Contraseña</label>
      <input
        type="password"
        name="password"
        id="passwords"
        class="form-control"
        required="required"
        v-model="form.password"
      />
    </div>

    <button type="submit" 
        class="btn btn-primary" 
        @click.prevent="loginUser"
    >
      Ingresar
    </button>
  </form>
</template>

<script>
import swal from 'sweetalert'
export default {
  data() {
    return {
      form: {
        email: "",
        password: "",
      },
    };
  },
  methods: {
      loginUser: async function() {
          try {
              let res = await this.$http.post('/api/usuario/login', this.form);
              console.log(res.data);
              let token = res.data.tokenReturn;
              let user = res.data.user;
              localStorage.setItem('jwt', token);
              localStorage.setItem('user', JSON.stringify(user));
              if (token) {
                  swal("Exitoso", "Login exitoso", "success");
                  this.$router.push("/admin");
              }
          } catch (err) {
              swal("Error", "Datos incorrectos", "error");
              console.error(err);
          }
      }
  },
};
</script>

<style>
</style>