<template>
	<Menu />
	<div class="container mt-5 d-flex justify-content-center align-items-center" style="min-height: 100vh;">
	        <div class="text-center">
	            <h1>{{ message }}</h1>
	            <p>BACK TO HOME PAGE IN 3 SECONDS</p>
	            <div class="spinner-border" role="status">
	                <span class="sr-only"></span>
	            </div>
	        </div>
	</div>
	<Footer />
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
            message: ''
        }
	},
	mounted() {
		this.setMessage()
	},


	methods: {
        setMessage() {
				if (this.$route.params.id == 1) {
                this.message = "Success: Post updated"
                } 
				if (this.$route.params.id == 2) {
                this.message = "Success: User deleted"
                }
				if (this.$route.params.id == 3) {
                this.message = "Success: Post added to database"
                }
				if (this.$route.params.id == 4) {
                this.message = "Welcome! You can now login"
                }
				if (this.$route.params.id == 5) {
                this.message = "Welcome back " + this.userStore.getUser.name + "!"
                }
				if (this.$route.params.id == 6) {
                this.message = `Bye ${this.userStore.user.name}, come back soon!`
                }	
				if (this.$route.params.id == 7) {
                this.message = "Success: Comment added to database"
                }																
				
				var timeleft = 2;
				var downloadTimer = setInterval(() => {
					if(timeleft <= 0){
						clearInterval(downloadTimer)
						this.$router.push('/')
					}
				timeleft -= 1
				}, 1000)
		},


	},
   

}
</script>

<style scoped>

</style>