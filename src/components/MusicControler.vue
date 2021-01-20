<template>
	
	<div class="music-controler">
		<audio :src="musicURL" ref='audio' id="audio" ></audio>
		<div class="cover">
			<div class="control">
				<svg @click="play" v-show="isPause" class="pause" xmlns="http://www.w3.org/2000/svg" version="1.1">
				  <circle cx="35" cy="35" r="15" stroke="white"
				  stroke-width="2" fill="transparent"/>
				  <path d="M29 27 L29 43 L44 35Z" stroke="white" fill="white"/>
				</svg>
				<svg @click="play" v-show="!isPause" class="play" xmlns="http://www.w3.org/2000/svg" version="1.1">
				  <circle cx="10" cy="10" r="8" stroke="white"
				  stroke-width="2" fill="transparent"/>
				  <path d="M8 6 L8 14 Z" stroke-width="2" stroke="white" fill="white"/>
				  <path d="M12 6 L12 14 Z" stroke-width="2" stroke="white" fill="white"/>
				</svg>
			</div>
			<img width="70px" src="http://p1.music.126.net/DSTg1dR7yKsyGq4IK3NL8A==/109951163046050093.jpg?param=130y130" alt="">
		</div>
		<div class="music-info">
			<div class="music-name">&nbsp;{{ musicName }} <span v-if="singer">- {{ singer }}</span></div>
			<div class="lyric">&nbsp; {{ lyric }}</div>
			<IconTest :pos="pos" id = "icontest" @changeCurrentTime="changeCurrentTime" @pause="pause"></IconTest>
			<Search class="search" @togleSearch="searchShow=!searchShow"></Search>
			<Music class="search-box" v-show="searchShow" @play="playFromUrl"></Music>
		</div>
	</div>
</template>

<script>
import IconTest from './IconTest.vue'
import Search from './Search.vue'
import Music from './Music.vue'


function test(){
	if (this.$refs.audio.currentTime === 0){
		this.pos = "0%";
		return;
	}
	this.pos = this.$refs.audio.currentTime / this.$refs.audio.duration * 100 + "%";
}

export default{
	name: "MusicControaler",
	components:{
		IconTest,
		Search,
		Music,
	},
	watch:{
		pos: function(val){
			if (val === "100%"){
				this.isPause = true;
			}
		}
	},
	data: function(){
		return {
			musicURL: "",
			isPause: true,
			pos: "0%",
			searchShow: false,
			musicName: "",
			singer: "",
			lyric: "",
		}
	},
	methods: {
		play: function(){
			if(this.isPause){
				this.$refs.audio.play();
				// 将this作为参数传进去时回创建this的copy,不能改变外部pos的值
				// 使用bind解决了这个问题
				this.a = setInterval(test.bind(this), 100)
			}else{
				this.$refs.audio.pause();
				clearInterval(this.a)
			}
			this.isPause = !this.isPause;
		},
		pause: function(){
			this.$refs.audio.pause();
		},
		changeCurrentTime: function(t){
			this.$refs.audio.pause();
			this.$refs.audio.currentTime = this.$refs.audio.duration * t;
			if (!this.isPause){
				this.$refs.audio.play();
			}
		},
		playFromUrl: function(url, name, singer){
			this.musicName = name;
			this.singer = singer;
			this.pause();
			this.isPause = true;
			this.musicURL = url;
			this.searchShow = false;
			setTimeout(this.play, 10);
			// this.play();
		}
	}
}
</script>

<style scoped>
svg{
	width: inherit;
	height: inherit;
	cursor: pointer;
}

.music-controler{
	width: 320px;
	height: 70px;
	border: 1px solid;
}
.cover{
	width: 70px;
	height: 70px;
	border-right: 1px solid;
	float: left;
	position: relative;
}
.control{
	width: inherit;
	height: inherit;
	background-color: rgba(0, 0, 0, 0.4);
	position: absolute;
}

.play{
	width: 20px;
	height: 20px;
	position: absolute;
	right: 2px;
	bottom: 2px;
}

.music-info{
	position: relative;
	width: 230px;
	height: 62px;
	padding-top: 8px;
	padding-left: 10px;
	float: left;
}
.music-name{
	font-size: 16px;
	font-weight: bold;
	text-align: left;
	/* border: 1px solid; */
}
.music-controler div{
	-webkit-user-select:none;
	-moz-user-select:none; 
	-ms-user-select:none; user-select:none;
}
.music-name span{
	font-size: 14px;
	font-weight: normal;
}
.lyric{
	margin-top: 2px;
	font-size: 14px;
	color: orangered;
}

.progress-bar{
	width: 200px;
}
.play-bar{
	width: 10px;
	position: absolute;
	bottom: 8px;
	border-top: 1px solid red;
	/* border-radius: 1px; */
}
.search{
	position: absolute;
	left: 220px;
	top: 4px;
}
.search-box{
	position: absolute;
	left: 50px;
	top: 4px;
}
</style>
