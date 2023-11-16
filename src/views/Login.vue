<template>
<div>

    <Menu />

	<section>
 	        <div  v-if="!userLoggedIn" id="register-form" class="container mt-5 pt-5">
 	            <div class="row">
 	                <div class="col-12 col-sm-7 col-md-6 m-auto">
 	                    <div class="card border-1 shadow">
 	                        <div class="card-body">
								<div style="text-align: center;">
 	                                <svg class="mx-auto my-3" xmlns="http://www.w3.org/2000/svg" width="50" height="50"
 	                                    fill="currentColor" clas="bi bi-person-circle" viewBox="0 0 16 16"
 	                                    style="display: block; margin: auto;">
 	                                    <path d="M11 6a3 3 0 1 1-6 0 3 3 0 0 1 6 0z" />
 	                                    <path fill-rule="evenodd"
 	                                        d="M0 8a8 8 0 1 1 16 0A8 8 0 0 1 0 8zm8-7a7 7 0 0 0-5.468 11.37C3.242 11.226 4.805 10 8 10s4.757 1.225 5.468 2.37A7 7 0 0 0 8 1z" />
 	                                </svg>
 	                            </div>
 	                            <form @submit.prevent="handleSubmit" class="row g-3">
 	                                <div class="mb-3">
	 	                                <label class="form-label">Email</label>
	 	                                <input v-model="user.email" placeholder="Your email address" class="common-input mb-20 form-control" required="" type="email">
	 	                            </div>
 	                                <div class="mb-3">
 	                                    <label class="form-label">Password</label>
 	                                    <input v-model="user.password" placeholder="Your password"  class="common-input mb-20 form-control" required="" type="password">
 	                                </div>
									<div v-if="error" class="alert alert-danger" role="alert">
 	            						{{ error }}
 	          						</div>
 	                                <div class="text-center mt-4">
										 <button type="submit" class="btn btn-outline-secondary">Login</button>
									</div>
									<div class="text-center mt-2">
										<button @click="cancel()" class="btn btn-outline-danger">Cancel</button>
									</div>	
 	                            </form>
 	                        </div>
 	                    </div>
 	                </div>
 	            </div>
 	        </div>
 	    </section>

    <Footer />

</div>
</template>

<script>

import Footer from '@/components/Footer.vue'
import Menu from '@/components/Menu.vue'

import { useUserStore } from '@/store/user'

export default {

	setup() {
		const userStore = useUserStore()
		return { userStore }
  	},

	components: {
		Footer,
        Menu
	},   
	data() {
      return {	
        user: {
			email: '',
			password: '',
        },
		submitting: false,
		error: null,
      }
    },
	
	methods: {
		handleSubmit: function () {
			this.submitting = true
			this.clearStatus()

			if (this.invalidEmail || this.invalidPassword) {
				this.error = 'Login failed: wrong email or password';
				return
			}

			this.loginUser(this.user)


		},
		clearStatus: function () {
			this.error = null
		},

		async loginUser(user) {
			if (await this.userStore.loginUserInDB(user)) {
				//login valid				
				this.$router.push('/message/5')
			}
			else {
				//login failed				
				this.error = 'Wrong credentials';
				this.submitting = false
			}

		},
		cancel() {
			this.$router.push({ name: 'Microposts.vue' });
		},
	},
	
	computed: {
		invalidPassword: function () {
			if (this.user.password === '') return true
			else return false
		},

		invalidEmail: function () {
			if (this.user.email === '' || this.user.email.search('@') === -1) return true
			else return false
		},
	},
	directives: {

	},
	created: function () {
		this.submitting = false
		this.error = false
	}
}
</script>

<style scoped>

</style>