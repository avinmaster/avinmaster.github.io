<template>
  <div>
    <!-- <nuxt-content :document="doc" /> -->
    <div id="bottom-nav" class="nav" :class="{ 'hide': navHide }">
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
        <a href="">En <span class="pe-7s-angle-down"/></a>
      </div>
    </div>

    <div id="right-nav" class="nav" :class="{ 'hide': navHide }">
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

    <full-page id="main" ref="fullpage" :options="options">
      <div class="section section-1 start active fp-auto-height-responsive">
        <theme-switcher :is-dark="isDark" @toggle="toggleTheme" />
        <div class="fp-bg" />
        <div class="content">
          <h1 class="fadeIn wow" data-wow-duration="1.4s">
            Hello! My name is<br>
            <span class="fadeIn wow" data-wow-duration="1.4s">Odilov Oybek.</span>
          </h1>
          <p class="fadeIn wow" data-wow-duration="2.1s">
            - a fullstack web-engineer.
          </p>
        </div>
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
  </div>
</template>

<script>
import ThemeSwitcher from '~/components/ThemeSwitcher.vue'

export default {
  name: 'IndexPage',
  components: {
    ThemeSwitcher
  },
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
        licenseKey: '11111111-11111111-11111111-11111111',
        anchors: ['start', 'my-work', 'about-me'],
        navigation: true,
        navigationPosition: 'left',
        navigationTooltips: ['Intro', 'My Work', 'About me'],

        responsiveWidth: 900,
        responsiveHeight: 600,

        controlArrows: false,
        css3: true,
        onLeave: this.onLeave,
        afterResponsive: this.onResponsive
      },
      navHide: false,
      sectionsWithScroll: [2],
      autoScrollDisabled: false,
      isDark: false
    }
  },

  computed: {
    fullpageApi () {
      return this.$refs.fullpage.api
    }
  },

  watch: {
    currentTheme (val) {
      document.body.classList.toggle('dark', val)
    }
  },

  mounted () {
    const initUserTheme = this.getTheme() || this.getMediaPreference()
    this.setTheme(Boolean(Number(initUserTheme)))

    const WOW = new (require('wow.js'))()

    WOW?.init()
  },

  methods: {
    getTheme () {
      return localStorage.getItem('dark-theme')
    },
    getMediaPreference () {
      return window.matchMedia('(prefers-color-scheme: dark)').matches
    },
    setTheme (isDark) {
      this.isDark = isDark
      localStorage.setItem('dark-theme', Number(isDark))
      document.documentElement.className = isDark ? 'dark' : ''
    },
    toggleTheme () {
      this.setTheme(!this.isDark)
    },
    onResponsive (val) {
      if (!this.fullpageApi) {
        return
      }
      const isScrollable = this.sectionsWithScroll.includes(this.fullpageApi.getActiveSection().index)

      if (!val) {
        if (isScrollable) {
          this.disableAutoScrolling()
        } else {
          this.enableAutoScrolling()
        }
      } else if (isScrollable) {
        this.disableAutoScrolling()
      }
    },
    scroll () {
      const rect = document.querySelector('[data-anchor=about-me]')?.getBoundingClientRect()?.top
      if (!rect) {
        return
      }

      if (this.fullpageApi.getActiveSection().index === 2 && rect > 1 && this.autoScrollDisabled) {
        this.enableAutoScrolling()
        this.fullpageApi.moveSectionUp()
      }
    },
    onLeave (orig, dest) {
      const destIndex = dest.index
      this.navHide = destIndex === 1
      document.querySelector('#fp-nav')?.classList.toggle('hide', destIndex === 1)

      if (document.body.offsetWidth < 900) {
        return
      }

      if (destIndex === 2) {
        setTimeout(this.disableAutoScrolling, 700)
      } else {
        this.enableAutoScrolling()
      }
    },
    enableAutoScrolling () {
      if (window.removeEventListener) {
        window.removeEventListener('scroll', this.scroll)
      } else {
        window.detachEvent('onscroll', this.scroll)
      }

      window.removeEventListener('scroll', this.scroll)
      this.fullpageApi.setAutoScrolling(true)
      this.fullpageApi.setFitToSection(true)
      this.autoScrollDisabled = false
    },
    disableAutoScrolling () {
      if (window.addEventListener) {
        window.addEventListener('scroll', this.scroll)
      } else {
        window.attachEvent('onscroll', this.scroll)
      }

      this.fullpageApi.setAutoScrolling(false)
      this.fullpageApi.setFitToSection(false)
      this.autoScrollDisabled = true
    }
  }
}
</script>
