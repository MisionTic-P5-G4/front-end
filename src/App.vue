<template>
  <div id="app" class="app">
    <header class="header">
      <nav>
        <div>
          <button v-on:click="loadHome">Guarderia</button>
          <button v-on:click="loadProducts">Productos</button>
          <button v-on:click="loadAboutUs">Acerca de nosotros</button>
        </div>
        <div>
          <button v-if="!is_auth" v-on:click="loadLogIn">Ingresar</button>
          <button v-if="!is_auth" v-on:click="loadSignUp">Registrarse</button>
          <button v-if="is_auth" v-on:click="logOut">Cerrar Sesión</button>
        </div>
      </nav>
    </header>

    <div class="container-fluid">
      <div class="row flex-nowrap">
        <div v-if="is_auth" class="col-auto px-0">
          <div
            id="sidebar"
            class="collapse collapse-horizontal show border-end"
          >
            <div
              id="sidebar-nav"
              class="list-group border-0 rounded-0 text-sm-start min-vh-100"
            >
              <br />
              <h2>
                <span>{{ gettingUsername }}</span>
              </h2>

              <div class="divider d-flex align-items-center my-4">
                <p class="text-center fw-bold mx-3 mb-0"></p>
              </div>

              <button v-on:click="loadUserProducts" class="btn btn-light">
                Mis Productos
              </button>
              <button v-on:click="loadUserInfo" class="btn btn-light">
                Mi info
              </button>
              <div
                v-if="gettingAdmin"
                class="divider d-flex align-items-center my-4"
              >
                <p v-if="gettingAdmin" class="text-center fw-bold mx-3 mb-0">
                  Admin
                </p>
              </div>
              <button
                v-if="gettingAdmin"
                v-on:click="loadAdminProducts"
                class="btn btn-light"
              >
                Productos
              </button>
              <button
                v-if="gettingAdmin"
                v-on:click="loadAdminUsers"
                class="btn btn-light"
              >
                Usuarios
              </button>
            </div>
          </div>
        </div>

        <main class="col ps-md-2 pt-2">
          <div class="main-component">
            <router-view
              v-on:completedLogIn="completedLogIn"
              v-on:completedSignUp="completedSignUp"
              v-on:logOut="logOut"
            >
            </router-view>
          </div>
        </main>
      </div>
    </div>

    <footer id="footer" class="footer">
      <div>
        <button v-on:click="loadHome">Guarderia</button>
        <button v-on:click="loadProducts">Productos</button>
        <button v-on:click="loadAboutUs">Acerca de nosotros</button>
      </div>
    </footer>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  name: "App",

  computed: {
    is_auth: {
      get: function () {
        return this.$route.meta.requiresAuth;
      },
      set: function () {},
    },
    gettingUsername: {
      get: function () {
        return (this.username = localStorage.getItem("username") || "");
      },
      set: function () {},
    },
    gettingAdmin: {
      get: function () {
        if (localStorage.getItem("is_admin") === "true")
          return (this.is_admin = true);
        else return (this.is_admin = false);
      },
      set: function () {},
    },
  },

  data: function () {
    return {
      is_admin: "",
      username: "",
    };
  },

  methods: {
    loadLogIn: function () {
      this.$router.push({ name: "logIn" });
    },

    loadSignUp: function () {
      this.$router.push({ name: "signUp" });
    },

    loadHome: function () {
      this.$router.push({ name: "home" });
    },

    loadProducts: function () {
      this.$router.push({ name: "products" });
    },

    loadAboutUs: function () {
      this.$router.push({ name: "about" });
    },

    loadAdminProducts: function () {
      this.$router.push({ name: "AdminProducts" });
    },

    loadAdminUsers: function () {
      this.$router.push({ name: "AdminUsers" });
    },

    loadUserInfo: function () {
      this.$router.push({ name: "UserInfo" });
    },

    loadUserProducts: function () {
      this.$router.push({ name: "UserProducts" });
    },

    completedLogIn: async function (data) {
      localStorage.setItem("token_access", data.token_access);
      localStorage.setItem("token_refresh", data.token_refresh);
      await this.getUserData();
      alert("Autenticación Exitosa");
      this.loadUserProducts();
    },

    completedSignUp: function (data) {
      alert("Registro Exitoso");
      this.completedLogIn(data);
    },

    logOut: function () {
      localStorage.clear();
      alert("Sesión Cerrada");
      this.loadLogIn();
      location.reload();
    },

    getUserData: async function () {
      await this.$apollo
        .query({
          query: gql`
            query UserDetailById {
              userDetailById {
                id
                username
                name
                email
                phone
                admin
              }
            }
          `,
        })
        .then((result) => {
          let dataGet = {
            id: result.data.userDetailById.id,
            username: result.data.userDetailById.username,
            name: result.data.userDetailById.name,
            email: result.data.userDetailById.email,
            phone: result.data.userDetailById.phone,
            admin: result.data.userDetailById.admin,
          };
          localStorage.setItem("is_admin", dataGet.admin);
          localStorage.setItem("username", dataGet.username);
        })
        .catch((error) => {
          console.log(error);
          alert("ERROR: Fallo geUserData");
        });
    },
  },
};
</script>


<style>
*,
*:after,
*:before {
  margin: 0;
  padding: 0;
  box-sizing: inherit;
}

body {
  justify-content: center;
  justify-items: center;
  margin: 0 0 0 0;
  background-color: #fdfefe;
}

.app {
  display: flex;
  flex-direction: column;
  height: 100%;
}

header {
  width: 100%;
  height: 10vh;

  background-color: #f29f05;
  color: white;

  display: flex;
  justify-content: space-between;
  align-items: center;

  margin-bottom: 25px;
  top: 0;
}

header nav {
  width: 100%;
  height: 100%;

  background-color: #f29f05;
  color: white;

  display: flex;
  justify-content: space-around;
  align-items: center;
}

header nav button {
  color: white;
  background: #f29f05;
  border: 1px none #e5e7e9;
  padding: 10px 20px;
}

header nav button:hover {
  color: white;
  background: #634610;
  border: 1px none #e5e7e9;
}

.main-component {
  margin: 0%;
  padding: 0%;

  background: #fdfefe;
}

footer {
  background-color: #ca7c16;
  margin-top: 25px;
  height: 80px;

  padding: 10px 10px 0px 10px;
  bottom: 0;
  width: 100%;
}
footer div {
  display: flex;
  height: 100%;
  align-items: center;
  align-content: space-between;
  justify-content: space-between;
  color: white;
  max-width: 1200px;
  margin: 0 auto;
}

footer nav {
  width: 100%;
  height: 100%;
  padding: 3rem;
  display: flex;
  align-items: center;
  justify-content: space-around;
  color: white;
  float: right;
}

footer button {
  color: white;
  background: #ca7c16;
  border: 1px none #e5e7e9;
}
footer button:hover {
  color: white;
  background: #634610;
  border: 1px none #e5e7e9;
}

html {
  background-color: white;
}

#sidebar-nav {
  width: 200px;
}

.col-auto {
  background-color: rgb(204, 204, 204);
}

.container-fluid {
  /*color: white;*/
  background-color: rgb(255, 255, 255);
  z-index: 1;
  top: 0;
  left: 0;
  bottom: 0;
  padding: 0.5em;

  display: flex;
  flex-direction: column;
}

.btn {
  margin: 1px;
}

h2 {
  text-align: center;
}

.divider {
  color: rgb(119, 119, 119);
}
</style>
