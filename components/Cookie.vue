<template>
  <div>
    <no-ssr>
      <div v-show="show" role="dialog">
        <div id="cookie-notice" class="cookie-notice">
          <div class="cookie-message">
            <div>
              <h4 class="cookie-title">{{ cookieTitle }}</h4>
            </div>
            <div>
              <p>
                {{ cookieMessage }}

                <a v-bind="{ href, target }"> {{ info }}</a>
              </p>
            </div>
          </div>
          <div class="button-area">
            <div>
              <button @click="AcceptCookieConsent" class="cookie-button accept">
                {{ AcceptButton }}
              </button>
            </div>
            <div>
              <button @click="DeclineCookieConsent" class="cookie-button decline">
                {{ DeclineButton }}
              </button>
            </div>
          </div>
        </div>
      </div>
    </no-ssr>
  </div>
</template>

<script>
export default {
  name: 'CookieConsentNuxt',

  props: {
    cookieTitle: {
      type: String,
      default: 'Cookie Consent'
    },

    cookieMessage: {
      type: String,
      default:
        'To optimize your browsing experience, this site uses cookies or similar technologies... For more information:'
    },

    href: {
      type: String,
      default: 'Click Here'
    },

    target: {
      type: String,
      default: '_blank'
    },

    info: {
      type: String,
      default: 'More Info'
    },

    AcceptButton: {
      type: String,
      default: 'Accept'
    },

    DeclineButton: {
      type: String,
      default: 'Dimiss'
    },

    cookieName: {
      type: String,
      default: 'user-cookies'
    },

    cookieValue: {
      type: Number,
      default: 1
    },

    cookieExpires: {
      type: Number,
      default: 30
    },

    cookiePath: {
      type: String,
      default: '/'
    }
  },

  data() {
    return {
      show: true
    }
  },

  computed: {
    cookie() {
      return !this.getCookie(this.cookieName)
    }
  },

  beforeMount() {
    this.show = this.cookie
  },

  mounted() {
    function showPopup() {
      document.getElementById('cookie-notice').style.display = 'flex'
    }

    setTimeout(() => showPopup(), 9500)
  },

  methods: {
    AcceptCookieConsent() {
      this.show = false
      this.setCookie(this.cookieName, this.cookieValue, this.cookieExpires, this.cookiePath)
    },

    DeclineCookieConsent() {
      this.show = false
      this.setCookie('', '', '', '')
    },

    getCookie(cookieName) {
      const value = `; ${document.cookie}`
      const parts = value.split(`; ${cookieName}=`)
      return parts.length !== 2 ? undefined : parts.pop().split(';').shift()
    },

    setCookie(name, value, expiresDays, path) {
      if (name != '') {
        const exdate = new Date()
        exdate.setDate(exdate.getDate() + (expiresDays || 30))

        const cookie = [
          `${name}=${value}`,
          `expires=${exdate.toUTCString()}`,
          `path=${path || '/'}`
        ]

        document.cookie = cookie.join(';')
      }
    }
  }
}
</script>

<style lang="css">
@import '~/static/css/cookie.css';
</style>
