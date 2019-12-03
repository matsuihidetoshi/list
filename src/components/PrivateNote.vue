<template>
  <div class="privateNote">
    <h1>PrivateNotes</h1>
    <div id="chat-form">
      <input v-model="content" name="content" class="form" placeholder="Content">
      <button v-on:click="createPrivateNote()">Post</button>
    </div>
  </div>
</template>
<script>
import { API, graphqlOperation} from "aws-amplify"
import { createPrivateNote } from "../graphql/mutations"
export default {
  name: 'PrivateNote',
  data () {
    return {
      content: "",
      privateNotes: []
    }
  },
  methods: {
    createPrivateNote: async function () {
      if (this.content === "") return
      const privateNote = {content: this.content}
      try {
        const privateNotes = [...this.privateNotes, privateNote]
        this.privateNotes = privateNotes
        this.content = ""
        await API.graphql(graphqlOperation(createPrivateNote, {input: privateNote}))
      } catch (error) {
        error
      }
    }
  }
}
</script>
<style scoped>

</style>