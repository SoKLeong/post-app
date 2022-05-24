<template>
    <nav :class="sideNavView ? 'show' : ''">
        <p id="greeting" v-show="username ? true: false">Welcome, {{ username }}</p>
        
        <span>MENU</span>
        <router-link @click="$emit('close-side-nav')" to="/" :class="this.$route.path === '/' ? 'active': ''"><i class="fa fa-newspaper"></i> Post</router-link>
        <router-link @click="$emit('close-side-nav')" v-show="username ? true : false" to="/mypost" :class="this.$route.path === '/mypost' ? 'active': ''"><i class="fa fa-edit"></i> My Post</router-link>
        <router-link @click="$emit('close-side-nav')" to="/about" :class="this.$route.path === '/about' ? 'active': ''"><i class="fa fa-question-circle"></i> About</router-link>
        <button v-show="username ? false: true" class="btn btn-outline-primary text-primary mt-3" @click="showLogin">Login / Sign Up</button>
        <button v-show="username ? true: false" class="btn btn-outline-danger text-danger mt-3" @click="$emit('logout')">Logout</button>
    </nav>
</template>

<script>
export default {
    name: 'SideNav',
    props: {
        sideNavView: Boolean,
        username: String
    },
    emits: ['logout', 'login', 'close-side-nav'],
    methods: {
        showLogin() {
            $('#loginModal').modal('show')
        }
    }
}
</script>

<style lang="scss" scoped>
@import "@/scss/_variables.scss";

nav {
    padding: 50px 0;
    background-color: white;
    display: flex;
    flex-direction: column;
    height: calc(100vh - 83px);
    z-index: 10;
    overflow-y: auto;
    
    p {
        padding: 10px 24px;
        margin-bottom: 30px;
    }

    a {
        padding: 10px 24px;
        color: black;
        transition: 0.2s;

        &:hover {
            text-decoration: none;
            color: $primaryColor;
            background-color: $primaryLight;
            padding-left: 32px;
        }

        &.active {
            background-color: $primaryLight;
            border-left: 5px solid $primaryColor;
            padding-left: 19px;
        }
    }

    span {
        text-transform: capitalize;
        color: $text-grey;
        margin: 10px 36px;
        font-size: 12px;
    }

    .btn-outline-danger:hover {
        background-color: lighten(red, 40%)
    }
}

@media only screen and (max-width: 1200px) {
    nav {
        position: fixed;
        width: 50%;
        left: -100%;
        transition: left 0.4s;
        padding: 8px;
        height: 100%;
        z-index: 10;

        &.show {
            left: 0;
        }
    }
}

@media only screen and (max-width: 576px) {
    nav {
        position: fixed;
        width: 80%;
        transition: left 0.4s;
        padding: 8px;
    }
}
</style>