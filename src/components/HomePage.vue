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
          <p>{{ inEnglish[0] }}</p>
        </div>
        <div class="box">
          <div class="field is-grouped">
            <p class="control is-expanded">
              <input class="input" type="text" />
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
      index: 1,
      categoryName: null,
      writtenSentence: null,
      timer: 0,
      italianSentences: italianSentences,
      inItalian: [],
      inEnglish: [],
    }
  },
  mounted() {
    this.getSentences()
    this.getCategory(this.index)
  },
  methods: {
    getSentences() {
      this.sentences = this.italianSentences
    },
    getCategory(index) {
      const category = this.sentences[index]
      this.categoryName = category[0]
      this.inItalian = category[1]['ITA']
      this.inEnglish = category[1]['EN']
      console.log(this.categoryName)
    },
  },
}
</script>
