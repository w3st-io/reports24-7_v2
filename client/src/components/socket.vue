<template>
	<div>
	</div>
</template>

<script>
	// [IMPORT] //
	import io from 'socket.io-client'

	// [IMPORT] Personal //
	import UserService from '@/services/user/UserService'
	import { EventBus } from '@/main'

	// [EXPORT] //
	export default {
		data() {
			return {
				socket: this.socket = io(),
				user_decoded: {},
				reqData: {},
			}
		},

		async created() {
			if (this.$store.state.node_env !== 'production') {
				this.socket = io('http://localhost:5000')
			}

			await this.handleUserLoggedIn()

			await this.updateNotifications()

			this.socket.on('update-notification', () => {
				setTimeout(() => { this.updateNotifications() }, 1500)
			})

			EventBus.$on('manager-logged-in', () => {
				EventBus.$emit('force-rerender')
			})

			EventBus.$on('manager-logged-out', () => {
				EventBus.$emit('force-rerender')

			})

			EventBus.$on('user-logged-in', () => {
				this.handleUserLoggedIn()
				EventBus.$emit('force-rerender')
			})

			EventBus.$on('user-logged-out', () => {
				this.handleUserLoggedOut()
				EventBus.$emit('force-rerender')

			})

			EventBus.$on('admin-logged-in', () => {
				this.handleAdminLoggedIn()
				EventBus.$emit('force-rerender')

			})

			EventBus.$on('admin-logged-out', () => {
				this.handleAdminLoggedOut()
				EventBus.$emit('force-rerender')
			})
		},

		methods: {
			async handleUserLoggedIn() {
				try {
					if (localStorage.usertoken) {
						this.user_decoded = await UserService.s_getUserTokenDecodeData()

						// [SOCKET] //
						this.socket.emit('join', this.user_decoded.user_id)
					}
				}
				catch (err) { `Socket: Error --> ${err}` }
			},

			async handleUserLoggedOut() {
				this.socket.emit('leave')
			},

			async handleAdminLoggedIn() {
				try {
					if (localStorage.usertoken) {
						this.user_decoded = await UserService.s_getUserTokenDecodeData()

						// [SOCKET] //
						this.socket.emit('join', this.user_decoded.user_id)
					}
				}
				catch (err) { `Socket: Error --> ${err}` }
			},

			async handleAdminLoggedOut() {
				this.socket.emit('admin-leave')
			},

			async updateNotifications() {
				try { EventBus.$emit('update-notification') }
				catch (err) { `Socket: Error --> ${err}` }
			},
		}
	}
</script>