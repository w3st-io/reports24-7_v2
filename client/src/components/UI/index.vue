<template>
	<div>
		<!-- Top Navbar & Side Menu -->
		<NavBar />

		<!-- Admin Bottom Bar -->
		<AdminNavBar v-if="adminLogged" />

		<!-- Pop Up Notifications -->
		<PopUpNotifications v-if="userLogged" />

		<!-- Floating Pop Up Banner -->
		<PopUpBanner
			v-if="message"
			:user_decoded="user_decoded"
			:message="message"
			BGColor="info"
		/>
	</div>
</template>

<script>
	// [IMPORT] Personal //
	import AdminNavBar from '@/components/admin/AdminNavBar'
	import PopUpBanner from '@/components/inform/PopUpBanner'
	import NavBar from '@/components/UI/nav/NavBar'
	import PopUpNotifications from '@/components/notifications/PopUpNotifications'
 
	export default {
		components: {
			AdminNavBar,
			PopUpBanner,
			NavBar,
			PopUpNotifications,
		},

		data() {
			return {
				userLogged: false,
				adminLogged: false,
				message: '',
			}
		},

		async created() {
			if (localStorage.usertoken) { this.userLogged = true }
			if (localStorage.admintoken) { this.adminLogged = true }
		},
	}
</script>