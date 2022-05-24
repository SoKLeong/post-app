<template>
    <div class="modal fade" id="loginModal" tabindex="-1" role="dialog">
        <div class="modal-dialog modal-dialog-centered" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLongTitle">Login / Register</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <div class="authSection login">
                        <h6>Login</h6>
                        <form @submit="login">
                            <div class="form-group">
                                <label for="loginEmail">Email</label>
                                <input type="email" v-model="loginEmail" class="form-control" id="loginEmail"
                                    placeholder="Your account email">
                            </div>
                            <button type="submit" class="btn btn-primary btn-block">Login</button>
                        </form>
                    </div>
                    <div class="authSection reg">
                        <h6>Register</h6>
                        <form @submit="register">
                            <div class="form-group">
                                <label for="regEmail">Email</label>
                                <input type="email" v-model="regEmail" class="form-control" id="regEmail"
                                    placeholder="Your email">
                            </div>
                            <div class="form-group">
                                <label for="regName">Name</label>
                                <input type="text" v-model="regName" class="form-control" id="regName"
                                    placeholder="Your name">
                            </div>
                            <div class="form-group">
                                <label for="regGender" class="d-block">Gender</label>
                                <div class="form-check form-check-inline">
                                    <input class="form-check-input" type="radio" name="regGender" id="regMale"
                                        value="male" v-model="regGender">
                                    <label class="form-check-label" for="regMale">Male</label>
                                </div>
                                <div class="form-check form-check-inline">
                                    <input class="form-check-input" type="radio" name="regGender" id="regFemale"
                                        value="female" v-model="regGender">
                                    <label class="form-check-label" for="regFemale">Female</label>
                                </div>
                            </div>
                            <button type="submit" class="btn btn-primary btn-block">Register</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AddPost',
    data() {
        return {
            loginEmail: '',
            regEmail: '',
            regName: '',
            regGender: ''
        }
    },
    emits: ['login', 'register'],
    methods: {
        login(e) {
            e.preventDefault()
            if(!this.loginEmail) {
                alert('Please add your login email.')
                return
            }

            this.$emit('login', this.loginEmail)

            this.loginEmail = ''
        },
        register(e) {
            e.preventDefault()
            if(!this.regEmail) {
                alert('Please add your register email.')
                return
            }
            if(!this.regName) {
                alert('Please add your register name.')
                return
            }
            if(!this.regGender) {
                alert('Please select your register gender.')
                return
            }

            const newAcc = {
                email: this.regEmail,
                name: this.regName,
                gender: this.regGender,
                status: 'active'
            }

            this.$emit('register', newAcc)

            this.regEmail = ''
            this.regName = ''
            this.regGender = ''
        }
    }
}

</script>

<style lang="scss" scoped>
@import "@/scss/_variables.scss";

.authSection {
    padding: 8px;
    border: 1px solid #aaa;
    margin-bottom: 16px;

    h6 {
        text-align: center;
        text-transform: uppercase;
        color: $text-grey;
        margin-bottom: 8px;
        text-decoration: underline;
    }
}

</style>