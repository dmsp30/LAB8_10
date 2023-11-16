<template>
    <div>
        <Menu />
				<div id="post-form" class="container">
					<div v-if="!userLoggedIn" class="container mt-5 d-flex justify-content-center align-items-center" style="min-height: 50vh;">
						<div class="text-center">		            
							<h3 style="text-align: center;">Login first </h3>
				        </div>
					</div>
					<div v-else>
						<div class="container mt-5 pt-5">
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
											<form @submit.prevent="handleSubmit">
												<div class="form-group">
													<textarea v-model="post.content"></textarea>
													<div v-if="error" class="alert alert-danger" role="alert">
														{{ error }}
													</div>
												</div>
												<p style="float:left;">
												<button @click="cancel()" class="btn btn-warning">Cancel</button>
												</p>
												<p style="float:right;">
												<button type="submit" class="btn btn-primary">Add post</button>
												</p>
											</form>
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>
				</div>
        <Footer />
    </div>

</template>

<script>
import Footer from '@/components/Footer.vue'
import Menu from '@/components/Menu.vue'

import { useUserStore } from '@/store/user'
import { useMicropostsStore } from '@/store/microposts'

export default { 

	setup() {
		const userStore = useUserStore()
		const micropostsStore = useMicropostsStore()
		return { userStore, micropostsStore}
  	},

    components: {
		Footer,
        Menu
	},	
	data() {
      return {
		submitting: false,
		error: false,
        post: {
            id:"",
            content:"",
            user_id:"",        
            author:"", 
            created_at:"",   
            updated_at:"", 
            likes:"",                             
        },
		user: {
			id: '', 
			name: '', 
			email: '', 
			session_id: ''
		},
      }
    },

	mounted() {
		this.getUser()
	},
	
	methods: {
        getUser() {
			this.user = this.userStore.getUser
		},
	},
	
	computed: {
		userLoggedIn: function () {
			this.getUser()
			for (var i in this.user) return true
			return false
		},
	},
	directives: {

	},
}
</script>

<style scoped>



</style>