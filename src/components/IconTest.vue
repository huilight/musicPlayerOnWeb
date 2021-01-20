<template>
	<div class="progress-bar" @mouseleave="mouseups">
		<IconBase class="o" width="200px" height="4px" stroke="#8f8f8f"  @mousemove="mousemoves($event)" @mouseup="mouseups" ><Lines></Lines></IconBase>
		<IconBase class="o" :width="width" height="4px" stroke="red" fill="black" @mousemove="mousemoves($event)" @mouseup="mouseups"><Lines></Lines></IconBase>
		<IconBase class="pointer" :style="position" width="8px" height="8px" fill="white" @mousedown="changePos($event)" @mouseup="mouseups"><Pointer></Pointer></IconBase>
	</div>
	
</template>

<script>
	
import IconBase from './icons/IconBase.vue'
import Lines from './icons/Line.vue'
import Pointer from './icons/Pointer.vue'

let startChange = false;

export default{
	name: "IconTest",
	props:{
		pos: {
			type: String,
			default: "0%",
		},
	},
	components: {
		IconBase,
		Lines,
		Pointer,
	},
	watch: {
		pos: function(val){
			this.width = this.pos;
			this.position = {left: this.pos};
		}
	},
	data: function(){
		return {
			width : this.pos,
			position: {left: this.pos},
		}
	},
	methods: {
		changePos: function(event){
			// console.log(this.pos)
			startChange = true;
			this.$emit("pause")
		},
		mousemoves: function(event){
			console.log(startChange)
		    if(startChange){
				// console.log(event.offsetX)
				this.width = event.offsetX + 'px';
				this.position = {left: event.offsetX + 'px'}
				// left = this.$refs.pos.style.left.split('px');
				// console.log(left)
			}
		},
		mouseups: function(){
			if (startChange){
				this.$emit('changeCurrentTime', parseInt(this.width)/200)
			}
			startChange = false;
			// console.log(parseInt(this.width))
			// console.log(parseInt(this.width)/200)
		},
	}
	
}
	
</script>

<style scoped>
.progress-bar{
	width: "200px";
	position: relative;
}
.o{
	position: absolute;
	top: 10px;
	left: 0px;
}
.pointer{
	cursor: pointer;
	position: absolute;
	top: 7px;
}
</style>
