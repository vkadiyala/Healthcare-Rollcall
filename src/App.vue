<template>
  <div id="app">
    <b-navbar variant="faded" type="dark" id="primary-nav">
      <b-container fluid="md">
        <b-navbar-brand v-on:click="goToDashboard()" role="link" class="branding-link">
          <img alt="City of Baltimore Seal" src="./assets/balt-logo-white.png" class="seal"/>
          <img alt="Healthcare Roll Call" src="./assets/hcrc.svg" class="app-logo"/>
        </b-navbar-brand>
      </b-container>
    </b-navbar>
    <b-navbar type="dark" id="secondary-nav">
      <b-container fluid="md">
        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item v-on:click="goToDashboard()" v-if="this.$root.auth_state && showDashboardLink()">&larr; Return to Dashboard</b-nav-item>
          </b-navbar-nav>
          <b-navbar-nav class="ml-auto" v-if="this.$root.auth_state">
            <b-nav-item-dropdown right>
              <!-- Using 'button-content' slot -->
              <template v-slot:button-content>
                {{ user }}
              </template>
              <b-dropdown-item v-on:click="logout()">Sign Out</b-dropdown-item>
            </b-nav-item-dropdown>
          </b-navbar-nav>
        </b-collapse>
      </b-container>
    </b-navbar>
    <router-view></router-view>
    <b-container fluid="md" id="footer">
      <img alt="Code for Baltimore Logo" src="./assets/CfB.png" width="200"/><br/>
      <strong>Proudly Designed &amp; Developed by <a href="https://codeforbaltimore.org/" target="_blank">Code for Baltimore</a></strong><br/>
      A local chapter of <a href="https://www.codeforamerica.org/" target="_blank">Code for America</a>
    </b-container>
  </div>
</template>

<script>
  export default {
    name: "App",
    data() {
      return {
        user: this.$jwt.decode(this.$root.auth_token).email,
        disableBackBtn: ['dashboard', 'create-contact', 'update-contact', 'facility']
      }
    },
    methods: {
      logout() {
        this.$root.destroySession();
        this.$router.push({ name: "login" });
      },
      goBack() {
        this.$router.go(-1);
      },
      goToDashboard() {
        if(this.$router.currentRoute.name !== "dashboard") {
          this.$router.push({ name: 'dashboard' });
        }
      },
      showDashboardLink() {
        return !(new RegExp(['dashboard','contact'].join('|')).test(this.$router.currentRoute.name));
      }
    }
  };
</script>

<style>
  #app {
    font-family: "Open Sans", OpenSans, Open-Sans, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: left;
    color: #2c3e50;
    width: 100%;
    margin: auto;
  }

  #primary-nav {
    background-color: #31363c;
    height: 120px;
  }

  #primary-nav .container-md, #secondary-nav .container-md {
    padding: 0 15px;
  }

  #secondary-nav {
    background-color: #42484f;
    height: 50px;
    margin-bottom: 30px;
    font-weight: 600;
    font-size: 14px;
  }

  #secondary-nav .nav-item > a {
    color: #ffffff;
    opacity: 1;
  }

  #secondary-nav .nav-item > a:hover {
    opacity: .8;
  }

  body {
    background-color: #f0f0f0;
  }

  h1 {
    padding: 0;
    margin-top: 0;
  }

  p.lead {
    font-size: 18px;
    font-weight: unset;
    margin-top: 30px;
  }

  nav#primary-nav img.seal {
    height: 88px;
    width: 88px;
    max-width: 88px;
    max-height: 88px;
  }

  nav#primary-nav img.app-logo {
    height: 60px;
    max-height: 60px;
    margin-left: 15px;
  }

  strong {
    font-weight: bold;
  }

  #footer {
    margin: 60px auto;
    text-align: center;
  }
  .branding-link {
    cursor: pointer;
  }
</style>
