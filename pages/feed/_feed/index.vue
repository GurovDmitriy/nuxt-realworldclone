<template>
  <main class="main-feed">
    <AppHero class="main-feed__hero">
      <TheTitleFeed class="main-feed__title-feed" />
    </AppHero>
    <h2 class="main-feed__caption visually-hidden">Main Content</h2>
    <TheColumnWrapperFeed class="main-feed__column-wrapper-feed" />
  </main>
</template>

<script>
import { actionTypes as actionTypesFeed } from "~/store/feed"
import { actionTypes as actionTypesUser } from "~/store/user"
import { getStrFromKebabCase } from "~/helpers/utils"
import { getIsValidParamsFeed } from "~/helpers/validateHook"

export default {
  async validate({ params }) {
    const feed = getStrFromKebabCase(params.feed)
    const res = await fetch(`http://localhost:3005/feeds?title_like=${feed}`, {
      method: "GET",
    })
    const data = await res.json()

    return getIsValidParamsFeed(feed, data)
  },

  async asyncData({ params, store, error }) {
    try {
      const feedTitle = getStrFromKebabCase(params.feed)

      const feedPayload = `title=${feedTitle}`
      await store.dispatch(actionTypesFeed.fetchFeed, feedPayload)

      const userPayload = `id=${store.state.feed.feed.userId}`
      await store.dispatch(actionTypesUser.fetchUser, userPayload)

      return {
        feedTitle,
      }
    } catch (err) {
      error(err)
    }
  },

  head() {
    return {
      title: `Feed ${this.feedTitle}`,
      meta: [
        {
          name: "description",
          content: `Template for learn Nuxt 2: Feed ${this.feedTitle}`,
          hid: "description",
        },
      ],
    }
  },
}
</script>

<style lang="scss">
.main-feed {
  @include container;

  padding-bottom: $space-l;
}
</style>
