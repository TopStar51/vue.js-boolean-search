<template>
  <el-container>
    <el-header>
      <el-input
        v-model="searchInput"
        clearable
        placeholder="Please type to search"
        @input="inputChange">
      </el-input>
    </el-header>
    <el-main>
      <list-item
        v-for="(blog, i) in groupBlogs"
        :key="i"
        :title="blog.title"
        :url="blog.url"
        :contents="blog.contents"
      ></list-item>
    </el-main>
  </el-container>
</template>

<script>
// @ is an alias to /src
import ListItem from '@/components/ListItem.vue'
import notes from '@/assets/notes.json'

export default {
  name: 'Home',
  components: {
    ListItem
  },
  data () {
    return {
      searchInput: '',
      blogs: notes.blogs,
      filteredBlogs: notes.blogs
    }
  },
  computed: {
    groupBlogs () {
      var newArr = Object.values(this.filteredBlogs.reduce((result, {
        url,
        title,
        highlight,
        note
      }) => {
        // Create new group
        if (!result[url]) {
          result[url] = {
            url,
            title,
            contents: []
          }
        }
        // Append to group
        result[url].contents.push({
          highlight,
          note
        })
        return result
      }, {}))
      return newArr
    }
  },
  methods: {
    inputChange () {
      this.filteredBlogs = this.blogs.filter(item =>
        item.url.toLowerCase().includes(this.searchInput.toLowerCase()) ||
        item.title.toLowerCase().includes(this.searchInput.toLowerCase()) ||
        item.highlight.toLowerCase().includes(this.searchInput.toLowerCase()) ||
        item.note.toLowerCase().includes(this.searchInput.toLowerCase())
      )
    }
  }
}
</script>
<style lang="scss" scoped>
.el-container {
  width: 50%;
  margin: auto;
  padding: 50px 20px;
}
::v-deep .el-input__inner {
  border: 2px solid gray;
  border-radius: 4px;
  &:focus {
    border: 2px solid #409EFF;
    border-radius: 4px;
    outline: 0;
  }
}
</style>
