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
 	                                <div class="col-md-6">
 	                                    <label class="form-label">User</label>
 	                                    <input v-model="user.name" placeholder="Your Usename"  class="common-input mb-20 form-control" required="" type="text">
 	                                </div>
 	                                <div class="col-md-6">
 	                                    <label class="form-label">Email</label>
 	                                    <input v-model="user.email" placeholder="Your email address" class="common-input mb-20 form-control" required="" type="email">
 	                                </div>
 	                                <div class="col-12">
 	                                    <label class="form-label">Password</label>
 	                                    <input v-model="user.password" placeholder="Your password"  class="common-input mb-20 form-control" required="" type="password">
 	                                </div>
 	                                <div class="col-12">
 	                                    <label class="form-label">Password Confirmation</label>
 	                                    <input v-model="user.passwordConfirm" placeholder="Confirm password"  class="common-input mb-20 form-control" required="" type="password">
 	                                </div>
									<div v-if="error" class="alert alert-danger" role="alert">
 	            						{{ error }}
 	          						</div>
 	                                <div class="text-center mt-4">
										 <button type="submit" class="btn btn-outline-secondary">Register</button>
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
			name: '',
			email: '',
			password: '',
        },
		passwordConfirmation: '',
		submitting: false,
		error: null,	
      }
    },
	
	methods: {
		async handleSubmit() {
			// Reset the submitting state and error message
			this.submitting = true;
			this.error = null;

			// Check each validation case separately
			if (this.invalidName) {
				this.error = 'Please enter a valid name.';
				this.submitting = false;
				return;
			}

			if (this.invalidEmail) {
				this.error = 'Please enter a valid email address.';
				this.submitting = false;
				return;
			}

			if (this.invalidPassword) {
				this.error = 'Please enter a valid password.';
				this.submitting = false;
				return;
			}

			if (this.invalidPasswordConfirm) {
				this.error = 'Password confirmation does not match.';
				this.submitting = false;
				return;
			}
			//validation passedd 
				// Check if user exists and then add the user
				if (!await this.userStore.userExistsInDB(this.user)) {
					//user does not exist in database ; register new user
					await this.userStore.addUserInDB();
					// Redirect to Message.vue or some success page
					this.$router.push('/message/4');
				} else {
					this.error = 'User already exists with this email.';
				}
			
			this.submitting = false;
		},
		cancel() {
			// Logic to cancel the form or navigate away
			this.router.push({ name: 'Microposts.vue' });
		},
	},	
	computed: {
		invalidName: function () {
			if (this.user.name === '' || this.user.name.length > 15 || this.user.name.match(/[0-9]/g) != null) return true
			else return false
		},
		invalidEmail: function () {
			if (this.user.email === '' || this.user.email.search('@') === -1) return true
			else return false
		},
		invalidPassword: function () {
			if (this.user.password.length < 3) return true
			else return false
		},
		invalidPasswordConfirm: function () {
			if (this.user.passwordConfirm != this.user.password) return true
			else return false
		},
	},
	directives: {
	},
	created() {
		this.submitting = false
		this.error = false
	}

}
</script>

<style scoped>

</style>