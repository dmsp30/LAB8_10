<template>
    <div>
		<nav class="navbar navbar-dark bg-dark fixed-top">
	        <div class="container-fluid">
	            <router-link class="navbar-brand" to="/">BLOG</router-link>
	            <button class="navbar-toggler" type="button" data-bs-toggle="offcanvas"
	                data-bs-target="#offcanvasDarkNavbar" aria-controls="offcanvasDarkNavbar"
	                aria-label="Toggle navigation">
	                <span class="navbar-toggler-icon"></span>
	            </button>
	            <div class="offcanvas offcanvas-end text-bg-dark" tabindex="-1" id="offcanvasDarkNavbar"
	                aria-labelledby="offcanvasDarkNavbarLabel">
	                <div class="offcanvas-header">
	                    <router-link class="offcanvas-title navbar-brand" id="offcanvasDarkNavbarLabel" to="/">POSTS</router-link>
	                    <button type="button" class="btn-close btn-close-white" data-bs-dismiss="offcanvas"
	                        aria-label="Close"></button>
	                </div>
	                <div class="offcanvas-body">
	                    <ul class="navbar-nav justify-content-end flex-grow-1 pe-3">
	                        <li class="nav-item">
								<li v-if="userIsEmpty"><a>WELCOME User</a></li>
								<li v-else><a>WELCOME {{ userStore.getUser.name }}</a></li>
	                        </li>
	                        <li class="nav-item dropdown">
	                            <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown"
	                                aria-expanded="false">MENU</a>
	                            <ul class="dropdown-menu dropdown-menu-dark">
	                                <li v-if="userIsEmpty"><router-link to="/register" class="dropdown-item">Register</router-link></li>
									<li v-else><router-link to="/post" class="dropdown-item">Post Blog</router-link></li>
	                                <li>
	                                    <hr class="dropdown-divider">
	                                </li>
	                                <li v-if="userIsEmpty"><router-link to="/login" class="dropdown-item">Login</router-link></li>
	                                <li v-else><router-link to="/logout" class="dropdown-item">Logout</router-link></li>
	                            </ul>
	                        </li>
	                    </ul>
	                </div>
	            </div>
	        </div>
	    </nav>
    </div>
</template>

<script>
import { useUserStore } from '@/store/user'

export default {
	setup() {
		const userStore = useUserStore()
		return { userStore}
  	},
	data() {
		return {
			user: {
				id: '', 
				name: '', 
				email: '', 
				session_id: ''
			},
		}
	},
	mounted() {
	},
    methods: {	
	},
	computed: {
		userIsEmpty: function () {
			let obj = this.userStore.getUser
			return Object.keys(obj).length === 0;
		}
	}
}
</script>