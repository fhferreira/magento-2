<template>
  <div class="confirm-page">
    <div class="confirm-page__banner">
      <div class="confirm-page__banner_image">
        <img
          :src="require('~/assets/img/confirm-email.svg')"
          alt=""
        >
      </div>
      <div class="confirm-page__banner_title">
        {{ $t('textTitle') }}
      </div>
      <div class="confirm-page__banner_description">
        {{ $t('textDescription') }}
      </div>
    </div>
    <div class="text-center">
      <b-row
        align-v="center"
        align-h="center"
      >
        <b-col md="auto">
          <b-button
            :disabled="refreshLoading"
            variant="success"
            size="lg"
            @click="handleRefresh"
          >
            <b-spinner
              v-if="refreshLoading"
              type="grow"
            />
            {{ $t('buttonRefresh') }}
          </b-button>
        </b-col>
        <b-col md="auto">
          <a
            class="confirm-page__button_resend"
            @click="handleResend"
          >
            {{ $t('buttonResend') }}
          </a>
        </b-col>
      </b-row>
    </div>
    <div class="text-center">
      <a
        class="confirm-page__button_logout"
        @click="handleLogout"
      >
        {{ $t('buttonLogout') }}
      </a>
    </div>
  </div>
</template>
<script>
import {mapGetters} from 'vuex'
export default {
  data() {
    return {
      refreshLoading: false
    }
  },
  layout: 'auth',
  middleware: ['authenticated', 'notConfirmed'],
  computed: {
    ...mapGetters({
      account: 'account/get'
    })
  },
  methods: {
    handleLogout() {
      this.$store.dispatch('auth/logout')
      this.$router.push('/check')
    },
    async handleRefresh() {
      this.refreshLoading = true
      await this.$store.dispatch('account/load')
      if(this.account.confirmed) {
        this.$router.push('/')
      }
      this.refreshLoading = false
    },
    async handleResend() {
      await this.$store.dispatch('auth/resend')
    }
  }
}
</script>
<i18n locale="en">
{
  "textTitle": "Please, confirm your email.",
  "textDescription": "Visit your email client and click the verification link. After return here and click refresh.",
  "buttonRefresh": "Refresh",
  "buttonResend": "Resend registration",
  "buttonLogout": "Try another email"
}
</i18n>
<style lang="scss">
  .confirm-page {
    margin-top: 60px;
    &__button_logout {
      display: block;
      cursor: pointer;
      margin-top: 55px;
      font-family: 'Open Sans', sans-serif;
      font-size: 18px;
      font-weight: 600;
      font-stretch: normal;
      font-style: normal;
      line-height: 1.11;
      letter-spacing: 0.18px;
      text-align: center;
    }
    &__button_resend {
      display: block;
      cursor: pointer;
      font-family: 'Open Sans', sans-serif;
      font-size: 18px;
      font-weight: 600;
      font-stretch: normal;
      font-style: normal;
      line-height: 1.11;
      letter-spacing: 0.18px;
      text-align: center;
    }
    &__banner {
      background-color: #f9f9f9;
      padding: 50px;
      margin-bottom: 40px;
      &_image {
        padding: 0 40px;
        margin-bottom: 20px;
        img {
          width: 100;
          max-width: 100%;
          height: auto;
        }
      }
      &_title {
          font-family: 'Open Sans', sans-serif;
          font-size: 28px;
          font-weight: normal;
          font-stretch: normal;
          font-style: normal;
          line-height: 1.36;
          letter-spacing: normal;
          text-align: center;
          color: $black;
      }
      &_description {
        font-family: 'Open Sans', sans-serif;
        font-size: 16px;
        font-weight: normal;
        font-stretch: normal;
        font-style: normal;
        line-height: 1.38;
        letter-spacing: normal;
        text-align: center;
        color: $black;
        margin-top: 5px;
      }
    }
  }
</style>
