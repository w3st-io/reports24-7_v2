<template>
	<BContainer>
		<BCard bg-variant="white" class="my-3">
			<h3 class="mb-3 text-light">
				In Reply to Comment "{{ comment_id }}"
			</h3>

			<!-- Comment Edit Component -->
			<CommentCreate
				v-if="!loading && comment != {}"
				:post_id="comment.post"
				:replyToComment_id="comment_id"
			/>
		</BCard>

		<!-- [ALERTS] -->
		<Alert v-if="error" variant="danger" :message="error" class="mt-3" />
	</BContainer>
</template>

<script>
	// [IMPORT] Personal //
	import CommentCreate from '@/components/comment/Create'
	import Alert from '@/components/inform/Alert'
	import CommentService from '@/services/user/CommentService'
	import PageService from '@/services/PageService'
	import router from '@/router'

	// [EXPORT] //
	export default {
		data() {
			return {
				// Default //
				comment_id: this.$route.params.comment_id,
				loading: true,

				// Comment //
				data: {},
				comment: {},

				// Error //
				error: '',
			}
		},

		components: {
			Alert,
			CommentCreate
		},

		methods: {
			async getPage() {
				try {
					this.data = await PageService.s_comment_reply(this.comment_id)
				}
				catch (err) { this.error = err }

				if (this.data.status) { this.comment = this.data.comment }
				else { this.error = this.data.message }
			},

			async submit(editorText) {
				if (localStorage.usertoken) {
					try {
						const comment = await CommentService.s_create(
							this.comment.post,
							editorText,
							this.comment._id,
						)

						if (comment.status) {
							// [REDIRECT] Post Page //
							router.push({
								name: 'post',
								params: {
									post_id: this.comment.post,
									limit: 20,
									page: 1,
								}
							})
						}
						else { this.error = comment.message }
					}
					catch (err) { this.error = err }
				}
				else { this.error = 'Error unable to update comment, no token passed' }
			},

			log() {
				console.log('%%% [PAGE] CommentReply %%%')
				console.log('comment:', this.comment)
				console.log('comment_id:', this.comment_id)
			},
		},

		async created() {
			// [REDIRECT] Log Needed //
			if (!localStorage.usertoken) { router.push({ name: 'user_login' }) }

			// Get Comment Details //
			await this.getPage()

			// Set Loaded //
			this.loading = false
			
			// [LOG] //
			this.log()
		},
	}
</script>