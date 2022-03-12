<template>
  <div class="container">
    <section class="hero is-success">
      <div class="hero-body">
        <p class="title">Italiano Practice</p>
        <p class="subtitle">
          Practice your italian with more than 1000 sentences
        </p>
        <div class="box">
          <p>Word Category: {{ categoryName }}</p>
          <p>{{ inEnglish[sentenceIndex] }}</p>
          <p>{{ inItalian[sentenceIndex] }}</p>
          <p>True count: {{ trueCount }}</p>
          <p>False count: {{ falseCount }}</p>
        </div>
        <div class="box">
          <div class="field is-grouped">
            <p class="control is-expanded">
              <input
                class="input"
                type="text"
                v-model="written"
                v-on:keyup.enter="nextSentence"
              />
            </p>
            <p class="control">
              <a class="button is-static">{{ timer }}sn.</a>
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
      categoryIndex: 1,
      sentenceIndex: 0,
      written: null,
      trueCount: 0,
      falseCount: 0,
      categoryName: null,
      writtenSentence: null,
      timer: 0,
      italianSentences: italianSentences,
      inItalian: [],
      inEnglish: [],
      isTrue: true,
    }
  },
  watch: {
    written(val) {
      if (!val || val == ' ') {
        this.written = ''
        return
      }
      const sentence = this.inItalian[this.sentenceIndex]
        .slice(0, val.length)
        .toLowerCase()
        .replace('è', 'e')
      const userSentence = val.toLowerCase()
      console.log(sentence)
      console.log(userSentence)
      this.isTrue = sentence == userSentence
    },
  },
  mounted() {
    this.getSentences()
  },
  methods: {
    getSentences() {
      this.sentences = this.italianSentences
      this.getCategory(this.categoryIndex)
    },
    getCategory(index) {
      const category = this.sentences[index]
      this.categoryName = category[0]
      this.inItalian = category[1]['ITA']
      this.inEnglish = category[1]['EN']
    },
    nextSentence() {
      if (this.written) {
        this.isTrue ? (this.trueCount += 1) : (this.falseCount += 1)
        if (this.sentenceIndex < this.inEnglish.length) {
          this.sentenceIndex += 1
        } else {
          this.sentenceIndex = 0
          this.categoryIndex += 1
          this.getCategory(this.categoryIndex)
        }
        this.written = ''
        this.isTrue = true

      }
    },
  },
}
</script>
