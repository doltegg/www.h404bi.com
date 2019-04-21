<template>
  <div class="stie-container">
    <Header />
    <main id="site-main" class="mina-wrapper">
      <div id="post" class="post-content">
        <article itemscope itemtype="http://schema.org/Article">
          <header class="post-header">
            <h1 class="page-title">{{ page.attributes.title }}</h1>
            <!-- <div class="page-meta">
              <div v-if="page.attributes.tags">
                <i class="icon ion-md-pricetags"></i>
                <span v-for="(item, index) in page.attributes.tags" :key="index">
                  {{ item }}
                </span>
              </div>
            </div> -->
          </header>
          <section v-if="isPostOutdated" class="post-alert outdated-alert notification">
            本文最后更新于 {{ days }} 天前（{{ humanDate }}），其中的信息可能已经有所发展或者不再适合现阶段。
          </section>
          <section class="post-body">
            <slot name="default" />
          </section>
          <Disqus v-if="page.attributes.comments !== false && $siteConfig.disqusjs" :page="page" />
          <footer class="post-footer"></footer>
        </article>
      </div>
    </main>
    <Footer />
  </div>
</template>

<script>
import Header from '../components/Header.vue'
import Footer from '../components/Footer.vue'
import Disqus from '../components/Disqus.vue'
import lozad from 'lozad'
import { date } from '../utils'

export default {
  props: ['page'],
  components: {
    Header,
    Footer,
    Disqus
  },
  mounted () {
    lozad(document.querySelectorAll('.post-content img')).observe()
  },
  methods: {
    date
  },
  computed: {
    days () {
      const nowDate = new Date()
      const postDate = new Date(this.page.attributes.updatedAt || this.page.attributes.createdAt)
      return Math.floor((nowDate - postDate) / 86400000)
    },
    humanDate () {
      return date(this.page.attributes.updatedAt || this.page.attributes.createdAt, '{YYYY}-{MM}-{DD}')
    },
    isPostOutdated () {
      return this.days > 365
    }
  },
  head () {
    return {
      title: `${this.page.attributes.title} - ${this.$siteConfig.title}`,
      meta: [
        {
          name: 'description',
          content: this.page.attributes.description
        },
        {
          name: 'keywords',
          content: this.page.attributes.keywords
        },
        {
          property: 'og:title',
          content: this.page.attributes.title
        },
        {
          property: 'og:description',
          content: this.page.attributes.description
        },
        {
          property: 'og:image',
          content: `${this.$siteConfig.url}${this.page.attributes.cover}`
        },
        {
          property: 'og:type',
          content: 'website'
        },
        {
          name: 'twitter:card',
          content: 'summary'
        },
        {
          name: 'twitter:creator',
          content: this.$siteConfig.author
        },
        {
          name: 'twitter:title',
          content: this.page.attributes.title
        },
        {
          property: 'twitter:image:src',
          content: `${this.$siteConfig.url}${this.page.attributes.cover}`
        }
      ]
    }
  }
}
</script>