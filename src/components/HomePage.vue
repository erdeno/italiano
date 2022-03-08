<template>
  <div class="container">
    <h1>Hello world</h1>
  </div>
</template>

<script>
import italianSentences from '@/assets/italiano.json'
export default {
  name: 'HomePage',
  data() {
    return {
      sentences: [],
      italianSentences: italianSentences,
    }
  },
  mounted() {
    this.getSentences()
    console.log(this.sentences)
  },
  methods: {
    getSentences() {
      this.sentences = this.italianSentences
    },
  },
}
</script>
