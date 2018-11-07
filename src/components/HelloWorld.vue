<template>
  <main>
    <div class="chat-frame">
      <div class="content">
        <div class="contact-profile">
          <p>DEVCENTER FRONTEND TEST CHAT UI</p>
        </div>
        <div class="messages">
          <center v-show="!gifs.length" style="margin-top: 10rem; color: #aaaaaa;">
            <h1>Enter a command below</h1>
            <i class="fa fa-arrow-down"></i>
          </center>
          <ul>
            <li class="replies" v-for="(gif, index) in gifs" :key="index">
              <iframe :src="gif" frameborder="0"></iframe>
            </li>
          </ul>
        </div>
        <div class="message-input">
          <div class="wrap">
            <input type="text" v-model="string" placeholder="Type in a command prefixed with '/giphy' to get an awesome gif e.g '/giphy slack'" />
            <button class="button" @click="convertStringToGif" :disabled="string == ''">
              <i class="fa fa-paper-plane"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  data () {
    return {
      api_key: '9nB3eMXwFy8DQBwJ0Ln7U4EXBEY4N6Ao',
      string: '',
      gifs: []
    }
  },
  methods : {
    convertStringToGif() {
      let s = this.string.split(" ")
      if(s[0] == '/giphy') {
        s.shift()
        let input = s.join('+')
        this.fetchGif(input)
      }else if(s[0] != '/giphy') {
        alert("Kindly use '/giphy' prefix")
      }
    },

    async fetchGif(input) {
      try {
        let res = await axios.get(`http://api.giphy.com/v1/gifs/search?q=${input}&api_key=${this.api_key}&limit=1`)
        this.gifs.push(res.data.data[0].embed_url)
      }catch (e) {
        console.log(e)
      }
    }
  }
}
</script>

<style scoped src="./style.css"></style>
