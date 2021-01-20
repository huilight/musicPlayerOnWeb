<template>
	<svg xmlns="http://www.w3.org/2000/svg" version="1.1"
		:width="width"
		:height="height"
		@mousedown="$emit('mousedown', $event)"
		@mousemove="$emit('mousemove', $event)"
		@mouseup="$emit('mouseup', $event)"
		@mouseleave="$emit('mouseleave', $event)"
		@click="$emit('click')"
	>
		<g :stroke="stroke" :fill="fill">
			<slot />
		</g>
	</svg>
</template>

<script>
export default{
	name: "IconBase",
	props:{
		width: String,
		height: String,
		stroke: String,
		fill: {
			type: String,
			default: "black"
		},
	},
}
</script>

<style scoped>
	svg{
/* 		border: 1px solid; */
	}
</style>
