<template>
	<BContainer class="my-3 nav-spacer">
		<BRow class="mb-3">
			<BCol cols="12">
				<BCard
					bg-variant="white"
					class="m-auto w-100"
					style="max-width: 500px;"
					text-variant="text-light"
				>
					<h5 class="text-center">Create New Password</h5>
					<p>Please enter your new password</p>

					<ValidationObserver v-slot="{ handleSubmit }">
						<form @submit.prevent="handleSubmit(submit)">
							<!-- Password -->
							<ValidationProvider
								tag="div"
								class="form-group"
								rules="required|password:8, 50|confirmed:@confirmation"
								v-slot="{ errors }"
							>
								<input
									v-model="password"
									type="password"
									class="form-control"
									:class="{ 'is-invalid border-danger': errors != '' }"
									placeholder="Password"
								>
								<span class="text-danger">{{ errors[0] }}</span>
							</ValidationProvider>

							<!-- Confirmed Password -->
							<ValidationProvider
								tag="div"
								name="confirmation"
								rules="required"
								class="form-group" 
								v-slot="{ errors }"
							>
								<input
									v-model="confirm"
									name="confirm"
									type="password"
									class="form-control"
									:class="{ 'is-invalid border-danger': errors != '' }"
									placeholder="Repeat Password"
								>
								<span class="text-danger">{{ errors[0] }}</span>
							</ValidationProvider>
					
							<!-- Submit -->
							<BButton
								:disabled="submitted"
								variant="primary"
								class="w-100"
								type="submit"
							>Reset Password</BButton>
						</form>
					</ValidationObserver>
				</BCard>

				<!-- Message -->
				<Alert
					v-if="message"
					variant="info"
					:message="message"
					class="mx-auto mt-3"
					style="max-width: 500px;"
				/>
			</BCol>
		</BRow>
	</BContainer>
</template>

<script>
	// [IMPORT] Personal //
	import Alert from '@/components/inform/Alert'
	import router from '@/router'
	import UserService from '@/services/user/UserService'

	// [EXPORT] //
	export default {
		components: {
			Alert,
		},

		data() {
			return {
				submitted: false,
				password: '',
				confirm: '',
				reqData: '',
				message: '',
			} 
		},

		methods: {
			async submit() {
				this.submitted = true

				try {
					this.reqData = await UserService.s_notLoggedResetPassword(
						this.$route.params.user_id,
						this.$route.params.verification_code,
						this.password
					)

					this.message = this.reqData.message
				}
				catch (err) {
					this.message = err

					this.submitted = false
				}
				
				console.log('reqData:', this.reqData)

				setTimeout(() => { router.push({ name: 'user_login' }) }, 1500)
			},
		},
	}
</script>