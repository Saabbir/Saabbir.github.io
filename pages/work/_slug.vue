<template>
  <div class="l-main page-single-work">
    <div class="u-py-48">     
      <div class="l-container">
        <div class="c-portfolio">
          <div class="c-portfolio__header">
            <h1 class="c-portfolio__header-title display-1">{{ work.title }}</h1>
            <h3 v-if="work.highlight" class="u-mt-16 u-text-gray">{{ work.highlight }}</h3>
            <div class="l-container--sm">
              <p v-if="work.description" class="u-mt-32 t-18">{{ work.description }}</p>
            </div>
          </div>
          <div class="c-portfolio__stats">
            <dl>
              <dt>Type</dt>
              <dd>{{ work.type }}</dd>
            </dl>
            <dl>
              <dt>Tools used:</dt>
              <dd v-for="tool in work.tools" :key="tool">{{ tool }}</dd>
            </dl>
            <dl>
              <dt>Live</dt>
              <dd>
                <a :href="work.url" class="c-button" target="_blank">View Live</a>
              </dd>
            </dl>
          </div>
          <div>
            <picture>
              <source :srcset="require(`~/assets/images/work/${work.coverImg}.jpg?webp`)" type="image/webp">
              <img 
                :src="require(`~/assets/images/work/${work.coverImg}.jpg`)" 
                :alt="`${work.title}-cover`">
            </picture>
          </div>
        </div>
      </div><!-- /.l-container -->
      <div class="nuxt-content-wrapper">
        <nuxt-content :document="work"></nuxt-content>
      </div>
      <div class="u-my-48">
        <hr>
      </div>      
      <div class="">
        <div class="l-container">
          <Pagination routeName="work-slug" :prev="prev" :next="next" type="Work" />
        </div>
      </div>      
    </div>
  </div>
</template>

<script>
  export default {
    name: 'SingleWork',
    head() {
      return {
        title: this.work.title + ' - Case Study',
        meta: [
          {
            hid: 'description',
            name: 'description',
            content: 'Saabbir Hossain is a professional front-end developer based in Barisal, Bangladesh.'
          }
        ]
      }
    },
    async asyncData({ $content, params }) {
      const work = await $content('work', params.slug).fetch()

      const [prev, next] = await $content('work')
        .only(['title', 'slug'])
        .sortBy('createdAt', 'asc')
        .surround(params.slug)
        .fetch()

      return {
        work,
        prev,
        next
      }
    },    
  }
</script>

<style lang="scss" scoped>

</style>