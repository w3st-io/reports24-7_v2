<template>
	<BContainer>
		<BRow class="mt-4">
			<BCol cols="12">
				<BCard bg-variant="white">
					<!-- Title -->
					<h2 class="text-white">Your Admin Profile</h2>

					<table class="table table-border table-dark">
						<tr>
							<td>Admin Id</td>
							<td>{{ adminDecoded.admin_id }}</td>
						</tr>
						<tr>
							<td>Username</td>
							<td>{{ adminDecoded.username }}</td>
						</tr>
						<tr>
							<td>Email</td>
							<td>{{ adminDecoded.email }}</td>
						</tr>
						<tr>
							<td>First Name</td>
							<td>{{ adminDecoded.first_name }}</td>
						</tr>
						<tr>
							<td>Last Name</td>
							<td>{{ adminDecoded.last_name }}</td>
						</tr>
					</table>
				</BCard>
			</BCol>
		</BRow>
	</BContainer>
</template>

<script>
	// [IMPORT] Personal //
	import router from '@/router'
	import AdminService from '@/services/admin/AdminService'

	// [EXPORT] //
	export default {
		data() {
			return {
				adminDecoded: {},
			}
		},

		methods: {
			async getAdminData() {
				try { this.adminDecoded = await AdminService.s_getAdminTokenDecodeData() }
				catch (err) { this.error = err }
			},

			log() {
				console.log('%%% [PAGE] Admin Profile %%%')
				console.log('adminDecoded:', this.adminDecoded)
			},
		},

		async created() {
			// [REDIRECT] Log Required //
			if (!localStorage.admintoken) { router.push({ name: '/' }) }

			// Retrieve User Data //
			await this.getAdminData()

			// [LOG] //
			//this.log()
		},
	}
</script>

<style scoped>
	td { color: white; }
</style>