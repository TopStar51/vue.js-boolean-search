<template>
  <div class="list-item">
    <h3 class="title">
      {{ title }}
    </h3>
    <div class="url">
      {{ url }}
    </div>
    <div v-for="(content, i) in contents" :key="i" class="content">
      <span v-if="content.highlight" :href="url" class="highlight" @click="goToUrl()">
        {{ content.highlight }}
      </span>
      <div v-if="content.highlight" class="empty-space"></div>
      <div v-if="content.note" class="note">
        {{ content.note }}
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'ListItem',
  props: {
    title: {
      type: String,
      default: ''
    },
    url: {
      type: String,
      default: ''
    },
    contents: {
      type: Array,
      default () {
        return []
      }
    }
  },
  methods: {
    goToUrl () {
      window.open(this.url, '_blank')
    }
  }
}
</script>
<style lang="scss" scoped>
.list-item {
  margin-bottom: 40px;
  .title {
    margin-bottom: 5px;
  }
  .url {
    font-size: 14px;
    margin-bottom: 10px;
    color: #7d7d7d;
    font-weight: 600;
  }
  .content {
    padding: 8px;
    border: 2px solid gray;
    border-radius: 4px;
    .highlight {
      cursor: pointer;
      background: #f7f7ab;
    }
    .empty-space {
      height: 30px;
    }
  }
}
</style>
