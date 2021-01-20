<template>
	<div class="music-search">
<!-- 		<div class="music-name">
		{{ name }}
		</div> -->
		<!-- <audio :src="musicURL" autoplay controls></audio> -->
		<div class="search-box">
			<!-- <label for="">搜索： -->
			<input type="text" @input="value=$event.target.value" v-on:keydown.enter="search">
			</label>
		</div>
		<div>
			<ul>
				<li v-for="music in musics" v-bind:id="music.id" @click="changeSrc(music.id, music.name, music.artists[0].name)">{{ music.name }}</li>
			</ul>
		</div>
	</div>
</template>

<script>

export default{

	name: "Music",
	data: function(){
		return {
			musicURL: "",
			name: "",
			musics: [],
		};
	},
	methods: {
		changeValue: function(value){
			console.log(value)
			this.value = value;
		},
		search: function(){
			let that = this;
			this.$axios.get("https://autumnfish.cn/search", {
				params: {
					keywords: this.value,
				}}).then(function(response){
					that.musics = response.data.result.songs;
			});
		},
		changeSrc: function(id, name, singer){
			let that = this;
			this.$axios.get("https://autumnfish.cn/song/url", {
				params:{
					id: id,
				}
			}).then(function(response){
				console.log(response)
				that.musicURL = response.data.data[0].url;
				that.name = name;
				that.singer = singer;
				console.log(that.musicURL);
				that.$emit('play', that.musicURL, that.name, that.singer)
			});
		}
	}
	
}
</script>

<style scoped>
ul{
	padding-left: 20px;
}
li{
	list-style: none;
	text-align: left;
	cursor: pointer;
}
li:hover{
	color: orangered;
}
.music-search{
	background-color: rgba(0, 0, 0, 0.8);
	color: white;
}
.search-box{
	text-align: left;
}
</style>
