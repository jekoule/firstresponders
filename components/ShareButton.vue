<template>
  <a :href="shareURL" @click.prevent="share()" target="_blank"
     :class="[ isButton ? `btn btn-share btn-${network.toLowerCase()}` : '' ]">
    <slot>Share on {{ network }}</slot>
  </a>
</template>

<script>
import config from '~/config'
import { openPopup } from '~/assets/js/helpers'

export default {
  props: {
    text: {
      type: String,
      required: false,
      default: null
    },
    url: {
      type: String,
      required: false,
      default: null
    },
    network: {
      type: String,
      required: true,
      default: null
    },
    isButton: {
      type: Boolean,
      required: false,
      default: true
    }
  },

  computed: {
    shareURL() {
      const network = this.network.toLowerCase()
      let url = this.url || config.sharing.url
      let text = this.text || config.sharing.defaultTweetText

      if (network === 'facebook' && !url.match(/facebook\.com\/sharer/)) {
        url = `https://www.facebook.com/sharer/sharer.php?u=${encodeURIComponent(url)}`
      }
      else if (network === 'twitter' && !url.match(/twitter\.com\/intent\/tweet/)) {
        url = `https://twitter.com/intent/tweet?text=${encodeURIComponent(text.trim())}&url=${encodeURIComponent(url)}`
      }
      return url
    }
  },

  methods: {
    share() {
      openPopup(this.shareURL, 'share')
    }
  }
}
</script>
