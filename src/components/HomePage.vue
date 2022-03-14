<template>
  <div class="container">
    <section class="hero" :class="isTrue ? 'is-primary' : 'is-danger'">
      <div class="hero-body">
        <p class="title">Italiano Practice</p>
        <p class="subtitle">
          Practice your italian with more than 1000 sentences
        </p>
        <progress
          class="progress is-link"
          :value="sentenceIndex"
          :max="inItalian.length"
        >
          15%
        </progress>
        <div class="columns is-mobile">
          <div class="column">
            <div class="box is-danger">
              <p>Word Category:</p>
              <div class="control has-icons-left">
                <div class="select">
                  <select @change="selectCategory">
                    <option
                      v-for="(sentence, key) in sentences"
                      :key="key"
                      :value="key"
                    >
                      {{ sentence[0] }}
                    </option>
                  </select>
                </div>
                <div class="icon is-small is-left">
                  <i class="fas fa-pizza-slice"></i>
                </div>
              </div>
            </div>
          </div>
          <div class="column">
            <p class="box is-danger">True count: {{ trueCount }}</p>
          </div>
          <div class="column">
            <p class="box is-danger">False count: {{ falseCount }}</p>
          </div>
        </div>
        <p class="box is-danger">{{ inEnglish[sentenceIndex] }}</p>
        <p class="box is-danger" v-if="showSentence">
          {{ inItalian[sentenceIndex] }}
        </p>
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
      showSentence: false,
    }
  },
  watch: {
    written(val) {
      if (!val || val == ' ') {
        this.written = ''
        return
      }
      const sentence = this.normalize(
        this.inItalian[this.sentenceIndex].slice(0, val.length).toLowerCase()
      )
      const userSentence = val.toLowerCase()
      // console.log(sentence)
      // console.log(userSentence)
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
      // console.log(this.inItalian)
    },
    nextSentence() {
      if (this.written) {
        if (
          this.isTrue &&
          this.written.length == this.inItalian[this.sentenceIndex].length
        ) {
          this.trueCount += 1
          this.showSentence = false
        } else {
          this.falseCount += 1
          this.showSentence = true
          return
        }
        if (this.sentenceIndex + 1 < this.inEnglish.length) {
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
    normalize(text) {
      text = text.replace('ì', 'i')
      text = text.replace('è', 'e')
      text = text.replace('à', 'a')
      text = text.replace('ò', 'o')
      return text
    },
    selectCategory(e) {
      const index = e.target.value
      this.getCategory(index)
    },
  },
}
</script>
