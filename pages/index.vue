<template>
  <div>
    <!-- <nuxt-content :document="doc" /> -->
    <div id="bottom-nav" class="nav" :class="{ 'hide': navHide, 'compact': navCompact }">
      <div class="menu fadeIn wow" data-wow-delay=".5s" data-wow-duration=".5s">
        <a href="#" class="portfolio-lnk">My work</a>
        <a href="#" class="about-lnk">About me</a>
      </div>

      <div
        class="lang fadeIn wow"
        data-wow-delay="1.5s"
        data-wow-duration=".5s"
      >
        <a href="">FAQ</a>
        <a href="">Contact</a>
        <a href="">En <span class="pe-7s-angle-down" /></a>
      </div>
    </div>

    <div id="right-nav" class="nav" :class="{ 'hide': navHide, 'compact': navCompact }">
      <a
        href="https://github.com"
        class="fadeIn wow"
        data-wow-delay=".4s"
        data-wow-duration="3s"
      >Github</a>
      <a
        href="https://dribbble.com"
        class="fadeIn wow"
        data-wow-delay=".6s"
        data-wow-duration="3s"
      >Dribbble</a>
      <a
        href="https://behance.net"
        class="fadeIn wow"
        data-wow-delay=".8s"
        data-wow-duration="3s"
      >Behance</a>
      <a
        href="mailto:oybek.odili@gmail.com"
        class="fadeIn wow"
        data-wow-delay="1s"
        data-wow-duration="3s"
      >Email</a>
    </div>

    <client-only>
      <full-page id="main" ref="fullpage" :options="options">
        <div class="section section-1 start active fp-auto-height-responsive">
          <h1 class="fadeIn wow" data-wow-duration="1.4s">
            Hello! My name is<br>
            <span class="fadeIn wow" data-wow-duration="1.4s">Odilov Oybek.</span>
          </h1>
          <p class="fadeIn wow" data-wow-duration="2.1s">
            - a fullstack web-engineer.
          </p>
        </div>

        <div class="section section-2 project">
          <div class="arrows">
            <button id="left-arrow">
              ←
            </button>
            <button id="right-arrow">
              →
            </button>
          </div>
          <div id="slide-1" class="slide active">
            <h1>Hello</h1>
          </div>
        </div>

        <div class="section section-3">
          <h1>About life of a web developer...</h1>
          <p>
            I'm a web programmer, <span>UX/UI</span> designer and backend/frontend developer.
          </p>
          <a href="#" class="btn">Read More!</a>
        </div>
      </full-page>
    </client-only>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  async asyncData ({
    $content,
    params
  }) {
    const doc = await $content(params.slug || 'hello').fetch()

    return {
      doc
    }
  },

  data () {
    return {
      options: {
        continuousHorizontal: true,
        parallax: true,
        parallaxOptions: {
          type: 'reveal',
          percentage: 62,
          property: 'translate'
        },
        licenseKey: '11111111-11111111-11111111-11111111',
        anchors: ['start', 'my-work', 'about-me'],
        navigation: true,
        navigationPosition: 'left',
        navigationTooltips: ['Intro', 'My Work', 'About me'],

        responsiveWidth: 900,
        responsiveHeight: 600,

        controlArrows: false,

        css3: true,

        onLeave: this.onLeave
      },
      navHide: false,
      navCompact: false,
      scrollbarEnabled: false,
      autoScrollDisabled: false
    }
  },

  mounted () {
    window.WOW = require('wow.js')

    new window.WOW().init()
  },

  methods: {
    scroll () {
      const rect = document.querySelector('[data-anchor=about-me]').getBoundingClientRect().top
      if (this.$refs.fullpage.api.getActiveSection().index === 2 && rect > 1) {
        window.removeEventListener('scroll', this.scroll)

        this.$refs.fullpage.api.setAutoScrolling(true)
        this.$refs.fullpage.api.moveSectionUp()
        this.$refs.fullpage.api.setFitToSection(true)
      }
    },
    onLeave (origin, destination) {
      if (destination.index === 2) {
        setTimeout(() => {
          this.$refs.fullpage.api.setAutoScrolling(false)
          this.$refs.fullpage.api.setFitToSection(false)
          window.addEventListener('scroll', this.scroll)
        }, 700)
      }

      this.navHide = destination.index === 1
      this.navCompact = destination.index !== 0
    }
  }
}
</script>
