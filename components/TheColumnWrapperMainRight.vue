<template>
  <aside class="column-wrapper-main-right">
    <div class="column-wrapper-main-right__tags-list-box">
      <h3 class="column-wrapper-main-right__caption">Popular Tags</h3>
      <AppLoading
        v-if="getIsLoadingTagsPopular"
        class="column-wrapper-main-right__loading"
        >Loading tags...</AppLoading
      >
      <AppButtonCaption
        v-if="getErrorsTagsPopular"
        :data-item="config.btn.refresh"
        class="column-wrapper-main-right__refresh"
        @clickBtn="fetchTags"
      />
      <Transition name="elements">
        <AppTagsList
          v-if="getTagsPopular"
          :data-item="getDataTagsPopular"
          class="column-wrapper-main-right__tags-list"
        />
      </Transition>
    </div>
  </aside>
</template>

<script>
import { mapState } from "vuex"

export default {
  transitions: {
    name: "elements",
    mode: "out-in",
  },

  data() {
    return {
      config: {
        btn: {
          refresh: {
            message: "Something went wrong",
            btnText: "Refresh",
            iconName: "arrow-clockwise",
            iconDesc: "refresh",
          },
        },
      },
    }
  },

  computed: {
    ...mapState({
      getTagsPopular: ({ tag }) => tag.tagsPopular,
      getIsLoadingTagsPopular: ({ tag }) => tag.isLoading,
      getErrorsTagsPopular: ({ tag }) => tag.errors,
    }),

    getDataTagsPopular() {
      return this.getTagsPopular.map((item) => {
        const path = `/tags/${item}`

        return {
          content: item,
          path,
        }
      })
    },
  },

  methods: {
    fetchTags() {
      this.$nuxt.refresh()
    },
  },
}
</script>

<style lang="scss">
.column-wrapper-main-right {
  @media (min-width: $min-width-tablet) {
    position: relative;
  }
}

.column-wrapper-main-right__tags-list-box {
  padding: $space-m;

  background-color: $var-color-light-accent;
  border-radius: 5px;

  @media (min-width: $min-width-tablet) {
    position: sticky;
    top: $space-l;
    left: 0;
  }
}

.column-wrapper-main-right__caption {
  @include text-default;

  margin-bottom: $space-s;
}
</style>
