<template>
	<BContainer class="my-3">
		<BCard bg-variant="white" class="text-center">
			<BCaraousel :slideObjs="PageData.caraousel" class="mb-4" />

			<a href="/" class="m-auto text-center">
				<BButton variant="primary" class="m-auto text-center">
					Return Home
				</BButton>
			</a>
		</BCard>
	</BContainer>
</template>

<script>
	import PageData from '../../defaults/pages/components/bcarousel'
	import BCaraousel from '../../components/display/BCarousel'

	export default {
		data() {
			return {
				PageData: PageData,
			}
		},

		components: {
			BCaraousel,
		},
	}
</script>