<template>
  <div class="container">
    <section class="hero is-success">
      <div class="hero-body">
        <p class="title">Italiano Practice</p>
        <p class="subtitle">
          Practice your italian with more than 1000 sentences
        </p>
        <div class="box">
          <p>Word Category: È</p>
          <p>Questo è mio!</p>
        </div>
        <div class="box">
          <div class="field is-grouped">
            <p class="control is-expanded">
              <input class="input" type="text" />
            </p>
          </div>
        </div>
      </div>
    </section>
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
