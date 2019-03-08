<template>
  <div>
    <article v-for="(location, idx) in locations" :key="idx">
      <img :src="location.image" >
      <h1>{{ location.name }}</h1>
    </article>
    <div>
      <p>Chat box</p>
      <div>
        <p>Starts here ...</p>
        <span v-for="message in chat">
          <p>{{message.message}} details: {{message.createdAt}}</p>
        </span>
      </div>
      <p>Say Something</p>
      <textarea v-model="message" cols="30" rows="10" ></textarea><br/>
      <button @click="sendMessage()">Send</button>
    </div>
  </div>
</template>

<script>
  import { db } from '../main'
  export default {
    name: 'HelloWorld',
    data () {
      return {
        locations: [],
        chat: [],
        message: ''
      }
    },
    methods: {
      sendMessage() {      // <-- and here

        const data = {
          message: this.message,
          createdAt: new Date()
        };
        db.collection('chat').add(data)
      }
    },
    firestore () {
      return {
        locations: db.collection('locations').orderBy('createdAt'),
        chat: db.collection('chat').orderBy('createdAt')
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
</style>
