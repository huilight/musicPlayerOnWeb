<template>
	
	<div class="music-controler">
		<audio :src="musicURL" ref='audio' id="audio" ></audio>
		<div class="cover">
			<div class="control">
				<svg @click="togglePlay" v-show="isPause" class="pause" xmlns="http://www.w3.org/2000/svg" version="1.1">
				  <circle cx="35" cy="35" r="15" stroke="white"
				  stroke-width="2" fill="transparent"/>
				  <path d="M29 27 L29 43 L44 35Z" stroke="white" fill="white"/>
				</svg>
				<svg @click="togglePlay" v-show="!isPause" class="play" xmlns="http://www.w3.org/2000/svg" version="1.1">
				  <circle cx="10" cy="10" r="8" stroke="white"
				  stroke-width="2" fill="transparent"/>
				  <path d="M8 6 L8 14 Z" stroke-width="2" stroke="white" fill="white"/>
				  <path d="M12 6 L12 14 Z" stroke-width="2" stroke="white" fill="white"/>
				</svg>
			</div>
			<img width="70px" height="70px" :src="picURL" alt="">
		</div>
		<div class="music-info">
			<div class="music-name">{{ musicName }}&nbsp;<span v-if="singer">- {{ singer }}</span></div>
			<div class="lyric">{{ lyric }}&nbsp;</div>
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
				clearInterval(this.a);
				clearInterval(this.b);
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
			lyrics: [],
			id: 0,
			picURL: "",
		}
	},
	methods: {
		setLyric: function(){
			for (let i in this.lyrics){
				let time = this.lyrics[i].time;
				let nextTime = i == this.lyrics.length-1 ? 9999 : this.lyrics[parseInt(i)+1].time;
				if (time < this.$refs.audio.currentTime && nextTime > this.$refs.audio.currentTime){
					this.lyric = this.lyrics[i].text;
					break;
				}
			}
		},
		togglePlay: function(){
			if(this.isPause){
				this.play()
			}else{
				this.pause()
			}
			// this.isPause = !this.isPause;
		},
		play: function(){
			this.$refs.audio.play();
			// 将this作为参数传进去时回创建this的copy,不能改变外部pos的值
			// 使用bind解决了这个问题
			this.a = setInterval(test.bind(this), 100);
			this.b = setInterval(this.setLyric, 10);
			this.isPause = false;
		},
		pause: function(){
			this.$refs.audio.pause();
			clearInterval(this.a);
			clearInterval(this.b);
			this.isPause = true;
		},
		changeCurrentTime: function(t){
			// this.$refs.audio.pause();
			this.$refs.audio.currentTime = this.$refs.audio.duration * t;
			this.play();
			// if (!this.isPause){
				// this.$refs.audio.play();
				// this.play();
			// }
		},
		playFromUrl: function(url, id, name, singer){
			this.musicName = name;
			this.singer = singer;
			this.pause();
			this.isPause = true;
			this.musicURL = url;
			this.searchShow = false;
			this.id = id;
			this.loadLyric();
			this.loadCover();
			setTimeout(this.play, 10);
			// this.play();
		},
		loadLyric: function(){
			this.$axios.get('https://autumnfish.cn/lyric', {
				params:{
					id: this.id,
				}
			}).then((response)=>{
				let lyricArray = response.data.lrc.lyric.trim().split('\n');
				this.lyrics = []
				for (let lrc of lyricArray){
				    let index = lrc.search(']');
				    let time = lrc.slice(1, index).split(':');
				    time = parseInt(time[0]) * 60 + parseFloat(time[1]);
				    time = time.toFixed(2);
				    let text = lrc.slice(index+1);
				    this.lyrics.push({'time': time ,'text': text});
				}
			})
		},
		loadCover: function(){
			this.$axios.get('https://autumnfish.cn//song/detail', {
				params: {
					ids: this.id,
				}
			}).then((response)=>{
				this.picURL = response.data.songs[0].al.picUrl;
			})
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
	width: 90%;
	height: 1.4em;
	font-size: 16px;
	font-weight: bold;
	text-align: left;
	overflow: hidden;
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
	width: inherit;
	height: 1.4em;
	overflow: hidden;
	text-overflow: clip;
	margin-top: 2px;
	font-size: 14px;
	text-align: left;
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
