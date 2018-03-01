<template>
  <div class="c-element-profile-options">
  <div class="btn-group w-100" role="group" aria-label="Navigation">
    <router-link
      :to="'/element/' + previousElementId"
      tag="button"
      type="button"
      class="btn btn-primary w-100"
      :disabled="previousElementId === 0">
      <i class="material-icons">chevron_left</i>
    </router-link>
    <router-link
      :to="'/element/' + nextElementId"
      tag="button"
      type="button"
      class="btn btn-primary w-100"
      :disabled="nextElementId === numberOfElements + 1">
        <i class="material-icons">chevron_right</i>
    </router-link>
  </div>

    <p class="text-muted mt-3">Debug</p>
    <mdc-switch v-model="debug"></mdc-switch>
  </div>
</template>

<script>
  import { mapState } from 'vuex'
  import * as types from '@/store/mutation-types'
  import MdcSwitch from '../shared/MaterialComponents/MdcSwitch'
  import router from '../../router'

  export default {
    name: 'element-profile-options',
    components: {
      MdcSwitch
    },
    methods: {
      performRedirect: function (e) {
        switch (e.keyCode) {
          case 37: // left
            router.push('/element/' + this.previousElementId)
            break
          case 39: // right
            router.push('/element/' + this.nextElementId)
            break
          default:
          // nothing
        }
      }
    },
    created: function () {
      document.addEventListener('keydown', this.performRedirect)
    },
    beforeDestroy: function () {
      document.removeEventListener('keydown', this.performRedirect)
    },
    computed: {
      ...mapState({
        numberOfElements: state => Object.keys(state.elements.default).length
      }),
      previousElementId () {
        return +this.$route.params.id - 1
      },
      nextElementId () {
        return +this.$route.params.id + 1
      },
      debug: {
        get () {
          return this.$store.state.configuration.debug
        },
        set (value) {
          this.$store.commit(types.TOGGLE_DEBUG, value)
        }
      }
    }
  }
</script>

<style scoped>
  .btn-group > .btn {
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: hidden;
  }
</style>
