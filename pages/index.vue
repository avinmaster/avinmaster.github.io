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
        href="http://github.com"
        class="fadeIn wow"
        data-wow-delay=".4s"
        data-wow-duration="3s"
      >Github</a>
      <a
        href="http://dribbble.com"
        class="fadeIn wow"
        data-wow-delay=".6s"
        data-wow-duration="3s"
      >Dribbble</a>
      <a
        href="http://behance.net"
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

    <full-page id="main" :options="options">
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
        <div id="slide-1" class="slide active" />
      </div>

      <div class="section section-3">
        <h1>About life of a web developer...</h1>
        <p>
          I'm a web programmer, <span>UX/UI</span> designer and backend/frontend developer.
        </p>
        <a href="#" class="btn">Read More!</a>
      </div>
    </full-page>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  async asyncData ({ $content, params }) {
    const doc = await $content(params.slug || 'hello').fetch()

    return {
      doc
    }
  },

  data () {
    return {
      options: {
        continuousHorizontal: true,
        scrollBar: true,
        parallax: true,
        parallaxOptions: { type: 'reveal', percentage: 62, property: 'translate' },
        licenseKey: '11111111-11111111-11111111-11111111',
        anchors: ['start', 'my-work', 'about-me'],
        navigation: true,
        navigationPosition: 'left',
        navigationTooltips: ['Intro', 'My Work', 'About me'],

        responsiveWidth: 900,
        responsiveHeight: 600,

        controlArrows: false,

        css3: false,

        onLeave: this.modifyClasses
      },
      navHide: false,
      navCompact: false
    }
  },

  methods: {
    modifyClasses (origin, destination) {
      this.navHide = destination.index === 1
      this.navCompact = destination.index !== 0
    }
  }
}
</script>
