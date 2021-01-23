<template>
	<div>
		<IconBase width="20px" height="20px" @click="$emit('togleSearch')"><SearchIcon></SearchIcon></IconBase>
	</div>
	
</template>

<script>
import IconBase from './icons/IconBase.vue'
import SearchIcon from './icons/SearchIcon.vue'
export default{
	name: "Search",
	components:{
		IconBase,
		SearchIcon,
	},
}
</script>

<style>
</style>
