<template>
  <section class="main">
    <div class="container main-container is-white left-main">
      <div class="left-container">
        <topics-nav />
        <topic-list :topics="topicsPage.results" :show-ad="false" />
        <pagination :page="topicsPage.page" url-prefix="/topics?p=" />
      </div>
      <topic-side />
    </div>
  </section>
</template>

<script>
import TopicSide from '~/components/TopicSide'
import TopicsNav from '~/components/TopicsNav'
import TopicList from '~/components/TopicList'
import Pagination from '~/components/Pagination'

export default {
  components: {
    TopicSide,
    TopicsNav,
    TopicList,
    Pagination
  },
  async asyncData({ $axios, params }) {
    try {
      const [user, topicsPage] = await Promise.all([
        $axios.get('/api/user/current'),
        $axios.get('/api/topic/topics')
      ])
      return { user, topicsPage }
    } catch (e) {
      console.error(e)
    }
  },
  head() {
    return {
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.$siteDescription()
        },
        { hid: 'keywords', name: 'keywords', content: this.$siteKeywords() }
      ]
    }
  }
}
</script>

<style lang="scss" scoped></style>
