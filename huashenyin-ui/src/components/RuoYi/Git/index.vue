<template>
  <div>
    <svg-icon icon-class="gitee" @click="goto" />
  </div>
</template>

<script>
export default {
  name: 'huashenyinGit',
  data() {
    return {
      url: 'https://gitee.com/huashenyin/RuoYi-Vue'
    }
  },
  methods: {
    goto() {
      window.open(this.url)
    }
  }
}
</script>
