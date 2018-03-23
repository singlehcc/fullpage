<template>
	<div class="full-page" v-if="option" :class="{'page-before': option.index < currentPage,
        'page-after': option.index > currentPage,
        'page-current': option.index === currentPage}">
		<slot></slot>
	</div>
</template>

<script>
	export default {
		data () {
			return {
				option: null
			}
		},
		created () {
			console.log(this.option)
		},
		props: ['currentPage']
	}
</script>

<style scoped>
	.full-page {
		overflow: hidden;
	    position: fixed;
	    top: 0;
	    bottom: 0;
	    left: 0;
	    right: 0;
	    transition: all 0.5s ease 0s;
	    z-index: 1;
	}

	.page-before {
	    z-index: 0;
	    transform: translate3d(0, -100%, 0);
	}
	.page-after {
	    z-index: 0;
	    transform: translate3d(0, 100%, 0);
	}
</style>