<template>
  <v-app>
   
    
    <v-container>
      <h2>网易云音乐解析</h2>
      <v-text-field v-model="n" label="请输入歌曲id"></v-text-field>
      <p>例如：https://music.163.com/#/song?id=1855080368 后面的“1855080368”就是歌曲id，只支持非会员歌曲，不提供破解服务</p>
      <v-btn @click="getLyric">解析</v-btn><a>  </a>
      <br /><br />
      <audio :src="url" controls></audio>
      <p v-if="lyric">{{lyric}}</p>
    </v-container>
  </v-app>
</template>





<script>

export default {
  data () {
    return {
      lyric: '',
      n: "",
      url: ""
    }
  },
  methods: {
    getLyric() {
      this.url = `http://music.163.com/song/media/outer/url?id=${this.n}`;
      const xhr = new XMLHttpRequest()
      xhr.open('GET', `https://cors-anywhere.wuyuan.dev/music.163.com/api/song/lyric?os=pc&id=${this.n}&lv=-1&kv=-1&tv=-1`, true)
      xhr.onreadystatechange = function() {
        if (xhr.readyState === 4 && xhr.status === 200) {
          const res = JSON.parse(xhr.responseText)
       
          this.lyric =  res.lrc.lyric.replace(/\n/g, "\n")
          this.lyric =  this.lyric.replace(/\[.*?\]/g, '')
        }
      }.bind(this)
      xhr.send()
    }
  }
}





</script>
