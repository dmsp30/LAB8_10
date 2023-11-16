<template >
	<div>
		<Menu />
			<div id="carouselExampleCaptions" class="carousel slide">
		        <div class="carousel-indicators">
		            <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" class="active"
		                aria-current="true" aria-label="Slide 1"></button>
		            <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1"
		                aria-label="Slide 2"></button>
		            <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2"
		                aria-label="Slide 3"></button>
		        </div>
		        <div class="carousel-inner">
		            <div class="carousel-item active c-item">
		                <img src="/src/assets/images/img3.jpg" class="d-block w-100 c-img" alt="image 1">
		                <div class="carousel-caption d-none d-md-block">
		                    <p>“Don't stop when you're tired, stop when you're done.”</p>
		                </div>
		            </div>
		            <div class="carousel-item c-item">
		                <img src="/src/assets/images/img2.jpg" class="d-block w-100 c-img" alt="iamge 2">
		                <div class="carousel-caption d-none d-md-block">
		                    <p>“The worst thing that can happen to a man is to become civilized”</p>
		                </div>
		            </div>
		            <div class="carousel-item c-item">
		                <img src="/src/assets/images/img1.jpg" class="d-block w-100 c-item" alt="iamg 3">
		                <div class="carousel-caption d-none d-md-block">
		                    <p>“Mental toughness is a lifestyle.”</p>
		                </div>
		            </div>
		        </div>
		        <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions"
		            data-bs-slide="prev">
		            <span class="carousel-control-prev-icon" aria-hidden="true"></span>
		            <span class="visually-hidden">Previous</span>
		        </button>
		        <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions"
		            data-bs-slide="next">
		            <span class="carousel-control-next-icon" aria-hidden="true"></span>
		            <span class="visually-hidden">Next</span>
		        </button>
			</div>	
		<!-- BEGIN MICROPOSTS -->
			<div class="posts">
	            <div class="container">
	                <div class="row">
	                    <div v-for="micropost in microposts" :key="micropost.id" class="col-sm-4 mb-4">
	                        <div class="card">
	                            <div class="card-body">
 	                                <h5 class="card-title">POST {{ micropost.id }}</h5>
	                                <h6 class="card-subtitle mb-3 mt-2">By: {{ micropost.name }}</h6>
	                                <p class="card-text">{{ micropost.content }}</p>
	                                <p class="card-text created-updated">Created: {{ micropost.created_at }}</p>
	                                <p class="card-text created-updated">Updated: {{ micropost.updated_at }}</p>
									<!-- Link de atualização condicional -->
	                                <p v-if="micropost.user_id === user.id"><router-link :to="'/updatePost/' + micropost.id">Update Post</router-link></p>
	                            </div>
	                        </div>
	                    </div>
	                </div>
	            </div>
	        </div>
			<!-- END MICROPOSTS -->	
		<Footer />

	</div>
</template>

<script>
import Footer from '@/components/Footer.vue'
import Menu from '@/components/Menu.vue'

import { useMicropostsStore } from '@/store/microposts'
import { useUserStore } from '@/store/user'
import { useCommentsStore } from '@/store/comments'

export default {

	setup() {
		const userStore = useUserStore()
		const micropostsStore = useMicropostsStore()
		const commentsStore = useCommentsStore()
		return { userStore, commentsStore, micropostsStore }
  	},

	components: {
		Footer,
        Menu
	},
	data() {
		return {
			microposts: [],
			comments: [],
			user: {
				id: '', 
				name: '', 
				email: '', 
				session_id: ''
			},
			show: '',
		}
	},
	mounted() {
		
		this.getPosts() 
		this.getUser()

	},
	methods: {
		async getPosts(){
			    await this.micropostsStore.getMicropostsInDB()
				this.microposts = this.micropostsStore.microposts
				console.log('posts ', this.microposts)
		},
		getUser() {
			this.user = this.userStore.getUser
		},
		
	},
	computed: {

	}
}
</script>

<style scoped>

</style>