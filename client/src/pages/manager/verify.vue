<template>
	<BContainer class="my-3 nav-spacer">
		<BCard
			bg-variant="white"
			text-variant="dark"
			class="mx-auto"
			style="max-width: 500px;"
		>
			<div v-if="success">
				<h3 class="text-success text-center">{{ success }}</h3>
				<h1 class="text-success text-center" style="font-size: 6em;">✓</h1>
			</div>

			<div v-if="error">
				<h3 class="text-danger text-center">Could not verify manager account</h3>
				<h1 class="text-danger text-center" style="font-size: 6em;">𝘹</h1>

				<p class="mt-3 text-dark">{{ error }}</p>
				
			</div>
		</BCard>
	</BContainer>
</template>

<script>
	// [IMPORT] Personal //
	import ManagerService from '@/services/manager/ManagerService'

	export default {
		data() {
			return {
				manager_id: this.$route.params.manager_id,
				verificationCode: this.$route.params.verification_code,
				returned: {},
				success: '',
				error: '',
			}
		},

		async created() {
			try {
				this.returned = await ManagerService.s_verify(
					this.manager_id,
					this.verificationCode
				)
			}
			catch (err) { this.error = err }

			if (this.returned.status && this.returned.existance) {
				this.success = 'Verified!'
			}
			else { this.error = this.returned.message }
		},
	}
</script>