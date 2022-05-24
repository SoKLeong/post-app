<template>
  <Header @toggle-side-nav="toggleSideNav" @toggle-to-do="toggleTodo" :sideNavView="sideNavView" />
  <div class="container-fluid">
    <div class="row">
      <div class="col-0 col-xl-2"></div>
      <div class="col-0 col-xl-2 sideNav">
        <SideNav :sideNavView="sideNavView" :username="username" @logout="logout" @close-side-nav="closeSideNav" />
      </div>
      <div class="col-12 col-xl-7 p-4">
        <router-view :user_id="user_id" />
        <Login @login="login" @register="register" />
      </div>
      <TodoList :user_id="user_id" :todoView="todoView" @check-login="checkLogin" />
      <Footer />
    </div>
    <div :class="[sideNavView ? 'active' : '', 'overlay']" @click="toggleSideNav"></div>
    <button class="btn btn-primary scrollTop" @click="scrollTop"><i class="fa fa-arrow-up"></i></button>
  </div>

</template>

<script>

import Header from '@/components/Header.vue';
import Footer from './components/Footer.vue';
import SideNav from './components/SideNav.vue';
import TodoList from './components/TodoList.vue';
import { useCookies } from "vue3-cookies";
import Login from "./components/Login.vue";


export default {
  name: 'App',
  components: {
    Header,
    Footer,
    SideNav,
    TodoList,
    Login
  },
  data() {
    return {
      sideNavView: false,
      todoView: false,
      username: '',
      user_id: 0
    }
  },
  methods: {
    toggleSideNav() {
      this.sideNavView = !this.sideNavView
    },
    toggleTodo() {
      this.todoView = !this.todoView
    },
    closeSideNav() {
      this.sideNavView = false
    },
    async login(email) {
      const data = await fetch(`https://gorest.co.in/public/v2/users?email=${email}`, {
        headers: {
          'Authorization': 'Bearer e32603fef138c73daea4e6fe78d735976da850a63065c41804a67b1a938e5bd3'
        }
      }).then(response => {
        if (response.ok) {
          return response.json()
        } else {
          return Promise.reject('Error: ' + response.status)
        }
      }).then(data => {
        if (data.length == 1) {
          this.username = data[0].name
          this.user_id = data[0].id
          this.cookies.set("user_id", data[0].id)
          this.cookies.set("username", data[0].name)
          alert(`Login successfully, welcome ${this.username}`)
          this.sideNavView = false
          $('#loginModal').modal('hide')
        } else {
          alert(`Login failed, failed to retrieve account ${email}`)
        }
      }).catch(error => {
        alert(`Login failed, ${error}`)
      })
    },
    async register(user) {
      await fetch(`https://gorest.co.in/public/v2/users`, {
        method: 'POST',
        headers: {
          'Authorization': 'Bearer e32603fef138c73daea4e6fe78d735976da850a63065c41804a67b1a938e5bd3',
          'Content-type': 'application/json'
        },
        body: JSON.stringify(user)
      }).then(response => {
        if (response.ok) {
          return response.json()
        } else {
          return Promise.reject('Error: ' + response.status)
        }
      }).then(data => {
        this.username = data.name
        this.user_id = data.id
        this.cookies.set("user_id", data.id)
        this.cookies.set("username", data.name)
        alert(`Register successfully, welcome ${this.username}`)
        this.sideNavView = false
        $('#loginModal').modal('hide')
      }).catch(error => {
        alert('Failed to register, error: ' + error)
      })
    },
    async checkLogin() {
      if (this.cookies.get("user_id") === null) {
        $('#loginModal').modal('show')
      } else {

        this.user_id = Number(this.cookies.get("user_id"))
        this.username = this.cookies.get("username")
      }
    },
    logout() {
      if (confirm(`Logout account ${this.username}?`)) {
        this.username = ''
        this.user_id = null
        this.cookies.remove("user_id")
        this.cookies.remove("username")
        this.sideNavView = false
        this.$router.push('/')
      }
    },
    scrollTop() {
      window.scrollTo({ top: 0, behavior: 'smooth' })
    }
  },
  setup() {
    const { cookies } = useCookies()
    return { cookies }
  },
  mounted() {
    this.checkLogin()
  }
}

</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
@import "@/scss/_variables.scss";

//Overwrite bootstrap styling (this is not the best practice)
.btn-primary:hover,
.btn-primary:active,
.btn-primary:visited,
.btn-primary:focus {
  background-color: darken($primaryColor, 5%) !important;
  color: white !important;
}

.btn-primary {
  color: white !important;
  background-color: $primaryColor !important;
}

.btn-outline-primary {
  border-color: $primaryColor !important;
}

.text-primary {
  color: $primaryColor !important;
}

.container-fluid {
  background-color: #fafafa;

  .sideNav {
    background-color: white;
  }
}

.page-item {
  .page-link {
    color: $primaryColor;
  }

  &.active button {
    background-color: $primaryColor !important;
  }
}

body {
  font-family: 'Poppins', sans-serif;
}

.scrollTop {
  border-radius: 50%;
  position: fixed;
  right: 2%;
  bottom: 5%;
  z-index: 5;
}

.overlay {
  width: 100%;
  height: 100%;
  background-color: #000000;
  position: fixed;
  top: 0;
  left: 0;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.4s;
  z-index: 4;

  &.active {
    background-color: #000000;
    opacity: 0.8;
    visibility: visible;
  }
}

.sideNav {
  position: fixed;
  z-index: 10;
}

</style>
