<template>
	<BContainer class="my-5 nav-spacer">
		<BCard text-variant="dark" class="mx-auto shadow" style="max-width: 800px;">
			<BRow>
				<!-- Title -->
				<BCol cols="12">
					<h3 class="text-center text-dark">Inspector Login</h3>
				</BCol>

				<!-- Welcome Half -->
				<BCol cols="12" md="6" class="d-none d-md-block">
					<h3 class="m-3 text-center">Welcome Back</h3>
					<div class="text-center">
						<LogInIcon size="8x" />
					</div>
				</BCol>

				<BCol cols="12" md="6">
					<!-- LOG IN FORM -->
					<ValidationObserver v-slot="{ handleSubmit }" tag="section">
						<form @submit.prevent="handleSubmit(login)">
							<!-- Email  -->
							<ValidationProvider
								tag="div"
								class="form-group"
								rules="required"
								v-slot="{ errors }"
							>
								<label for="email">Email</label>
								<input
									v-model="email"
									name="email"
									type="email"
									class="form-control"
									:class="{ 'is-invalid border-danger': errors != '' }"
									placeholder="Example@example.com"
								>
								<span class="text-danger">{{ errors[0] }}</span>
							</ValidationProvider>

							<!-- Password -->
							<ValidationProvider
								tag="div"
								class="form-group"
								rules="required"
								v-slot="{ errors }"
							>
								<label for="password">Password</label>
								<input
									v-model="password"
									name="password"
									type="password"
									class="form-control"
									:class="{ 'is-invalid border-danger': errors != '' }"
									placeholder="******"
								>
								<span class="text-danger">{{ errors[0] }}</span>
							</ValidationProvider>
							<br>

							<!-- Submit -->
							<BButton
								variant="primary"
								size="lg"
								:disabled="submitted"
								class="w-100 mb-1"
								type="submit"
							>Login</BButton>
						</form>
						<RouterLink
							to="/user/password/request"
							class="text-primary"
						>Forgot password?</RouterLink>
					</ValidationObserver>
				</BCol>
			</BRow>
			
			<!-- [ERROR] -->
			<BRow class="mt-3">
				<BCol cols="12">
					<Alert
						v-if="error"
						variant="danger"
						:message="error"
						class="m-auto"
						style="max-width: 800px;"
					/>
				</BCol>
			</BRow>
		</BCard>
	</BContainer>
</template>

<script>
	// [IMPORT] //
	import { LogInIcon } from 'vue-feather-icons'

	// [IMPORT] Personal //
	import Alert from '@/components/inform/Alert'
	import router from '@/router'
	import UserService from '@/services/user/UserService'

	// [EXPORT] //
	export default {
		components: {
			LogInIcon,
			Alert,
		},

		data() {
			return {
				submitted: false,
				email: '',
				password: '',
				reqData: '',
				error: '',
			}
		},

		methods: {
			async login() {
				try {
					// [VALIDATE] //
					if (!this.email || !this.password)  {
						this.error = 'Fields are required'
						return
					}

					// [LOGIN] //
					this.reqData = await UserService.s_login(
						this.email,
						this.password
					)
					
					// Check Validation Status //
					if (
						this.reqData.status == true &&
						this.reqData.validation == true
					) { router.go(-1) }
					else { this.error = this.reqData.message }
				}
				catch (err) { this.error = err }
			},
		},

		async created() {
			// [REDIRECT] Logged //
			if (localStorage.usertoken) { router.push({ name: '/' }) }
		},
	}
</script>