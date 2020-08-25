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
import notes from '@/notes.json'

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
      /** Single search */
      if (this.searchInput.includes(':')) {
        const splits = this.searchInput.split(':')
        const key = splits[0].trim()
        const value = splits[1].trim()
        let searchValue
        if (value.charAt(0) === '"') {
          searchValue = value.substring(1)
          const lastIndex = value.lastIndexOf('"')
          if (lastIndex > 0) {
            searchValue = value.substring(1, lastIndex)
          }
        } else if (value.includes(' ')) {
          const splits = value.split(' ')
          searchValue = splits[0].trim()
        } else {
          searchValue = value
        }
        this.filteredBlogs = this.blogs.filter(item =>
          item[key] && item[key].toLowerCase().includes(searchValue.toLowerCase())
        )
      } else {
        this.filteredBlogs = this.blogs
      }
      /* if (this.searchInput.includes('&')) {
        const splits = this.searchInput.split('&')
        const search1 = splits[0].trim()
        const search2 = splits[1].trim()
        if (search1.includes(':')) {
          const splits = search1.split(':')
          const key = splits[0].trim()
          const value = splits[1].replace(/["]/g, '').trim()
          this.filteredBlogs = this.blogs.filter(item =>
            item[key] && item[key].toLowerCase().includes(value.toLowerCase())
          )
          if (search2.includes(':')) {
            const splits2 = search2.split(':')
            const key2 = splits2[0].trim()
            const value2 = splits2[1].replace(/["]/g, '').trim()
            this.filteredBlogs = this.blogs.filter(item =>
              item[key] && item[key].toLowerCase().includes(value.toLowerCase()) && item[key2] && item[key2].toLowerCase().includes(value2.toLowerCase())
            )
          }
        } else {
          this.filteredBlogs = this.blogs
        }
      } else {
        if (this.searchInput.includes(':')) {
          const splits = this.searchInput.split(':')
          const key = splits[0].trim()
          const value = splits[1].replace(/["]/g, '').trim()
          this.filteredBlogs = this.blogs.filter(item =>
            item[key] && item[key].toLowerCase().includes(value.toLowerCase())
          )
        } else {
          this.filteredBlogs = this.blogs
        }
      } */
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
