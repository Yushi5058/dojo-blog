<template>
  <div class="home">
      <h1>home</h1>
      <div v-if="posts.length">
          <PostList v-if="showPosts"  :posts="posts" />
          <button @click="showPosts = !showPosts">toggle posts</button>
          <button @click="posts.pop()">delete a post</button>
      </div>
      <div v-if="error">{{ error  }}</div>
      <div v-else>Loading ...</div>
    </div>
</template>

<script>
import PostList from '../components/PostList.vue';
import { ref } from 'vue';
export default {

  name: 'Home',
    components: { PostList },
    setup() {
        const posts = ref([  ])
        const error = null

        const load = async() => {
            try {
                let data = await fetch('http://localhost:3000/posts')
                if (!data.ok) {
                    throw Error('no data available')
                }
                posts.value = await data.json()
            }
            catch(err) {
                error.value = err.message
            }
        }
        load()
        const showPosts = ref(true)

        return { posts, showPosts, error }
    }
}
</script>
