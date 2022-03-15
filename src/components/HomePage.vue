<template>
  <div class="container">
    <section class="hero" :class="isTrue ? 'is-primary' : 'is-danger'">
      <div class="hero-body">
        <p class="title">Italiano Practice</p>
        <p class="subtitle">
          Practice your italian with more than 1000 sentences
        </p>
        <div class="columns">
          <div class="column">
            <div class="box">
              <div class="control has-icons-left">
                <div class="select is-rounded">
                  <select @change="selectCategory">
                    <option
                      v-for="(sentence, key) in sentences"
                      :key="key"
                      :value="key"
                      :selected="key == categoryIndex"
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
        </div>
        <div class="buttons">
          <button class="button is-success is-light is-static">
            True: {{ trueCount }}
          </button>
          <button class="button is-danger is-light is-static">
            False: {{ falseCount }}
          </button>
        </div>
        <progress
          class="progress is-link"
          :value="sentenceIndex"
          :max="inItalian.length"
        >
          15%
        </progress>
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
      this.isTrue = sentence == userSentence
    },
  },
  mounted() {
    this.getSentences()
  },
  methods: {
    getSentences() {
      this.sentences = this.italianSentences
      this.getCategory()
    },
    getCategory() {
      const category = this.sentences[this.categoryIndex]
      this.categoryName = category[0]
      this.inItalian = category[1]['ITA']
      this.inEnglish = category[1]['EN']
      this.sentenceIndex = 0
    },
    nextSentence() {
      if (this.written) {
        if (
          this.isTrue &&
          this.written.length == this.inItalian[this.sentenceIndex].length
        ) {
          this.trueCount += 1
          this.showSentence = false

          if (this.sentenceIndex + 1 < this.inEnglish.length) {
            this.sentenceIndex += 1
          } else {
            this.categoryIndex += 1
            this.getCategory()
          }
          this.written = ''
          this.isTrue = true
        } else {
          this.falseCount += 1
          this.showSentence = true
          return
        }
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
      const index = Math.floor(e.target.value)
      this.categoryIndex = index
      this.getCategory()
      this.trueCount = 0
      this.falseCount = 0
    },
  },
}
</script>
