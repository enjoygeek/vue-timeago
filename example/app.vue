<style>
  body {
    font-family: Helvetica;
    margin: 0;
  }
  .header {
    height: 40px;
    line-height: 40px;
    background-color: #f0f0f0;
    padding: 0 20px;
    & select {
      margin-right: 10px;
    }
  }
  .people {
    margin-top: 20px;
    padding: 0 20px;
  }
  .person {
    font-size: 1.4rem;
    & .name {
      width: 150px;
      display: inline-block;
    }
    & .meta {
      display: inline-block;
    }
  }
  .timeago {
    color: #999;
  }
</style>

<template>
  <div class="app">
    <header class="header">
      Choose your language:
      <select @change="handleChange">
        <option :value="lang" v-for="lang in langs" :selected="lang === currentLang">
          {{ lang }}
        </option>
      </select>
      <input type="button" :value="autoUpdate ? 'Stop update' : 'Resume update'" @click="toggleUpdate">
    </header>
    <div class="people">
      <div class="person" v-for="person in people">
        <span class="name">{{ person.name }}</span>
        <div class="meta">
          was born at
          <timeago
            :auto-update="autoUpdate"
            :max-time="86400 * 365"
            :locale="currentLang"
            class="timeago"
            :since="person.birth"></timeago>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import qs from './qs'
  import locales from './locales'

  const now = new Date().getTime()
  const people = [
    {
      name: 'egoist',
      birth: now - 60000
    },
    {
      name: 'chelly',
      birth: now - 60000 * 22
    },
    {
      name: 'aimer',
      birth: now - 60000 * 102
    },
    {
      name: 'young boy',
      birth: now - 1000
    },
    {
      name: 'old man',
      birth: new Date('2014-01-01')
    }
  ]

  export default {
    data() {
      return {
        autoUpdate: 1,
        people,
        currentLang: qs().lang || 'en-US',
        langs: Object.keys(locales)
      }
    },
    methods: {
      handleChange(e) {
        this.currentLang = e.target.value
      },
      format(val) {
        return ``
      },
      toggleUpdate() {
        this.autoUpdate = this.autoUpdate ? 0 : 1
      }
    }
  }
</script>
