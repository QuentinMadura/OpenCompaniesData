<template>

  <section class="hero has-background-white-ter is-fullheight skip-navbar">

    <div class="hero-body">
      <div class="container has-text-centered">
        <div class="columns is-mobile is-centered">
          
          <!-- USER NOT LOGGED -->
          <div class="column is-6" 
            v-if="!user.isLoggedin">
            <h3 class="title has-text-grey">
              <!-- YOU'VE BEEN DISCONNECTED -->
              <!-- {{ getText('disconnect_msg') }} -->
              {{ basicDict.disconnect_msg[locale] }}
            </h3>
            <div class="box">

              <!-- <nuxt-link 
                class="button is-block is-primary is-primary-b is-fullwidth" 
                :to="'/'"
                >
                home
              </nuxt-link> -->
        
              <button
                class="button is-block is-primary is-primary-b is-fullwidth" 
                @click="goBack"
                >
                <span class="icon">
                  <i class="fas fa-arrow-left"></i>
                </span>
                <span>
                  <!-- Back -->
                  {{ basicDict.bo_back[locale] }}
                </span>
              </button>


              <br>
        
              <nuxt-link 
                class="button is-block is-primary is-primary-b is-fullwidth" 
                type="submit" 
                :to="'/login'"
                >
                <!-- RECONNECT -->
                <span class="icon">
                  <i class="fas fa-sign-in-alt"></i>
                </span>
                <span>
                  <!-- {{ getText('reconnect') }} -->
                  {{ basicDict.reconnect[locale] }}
                </span>
              </nuxt-link>

            </div>
          </div>

          <!-- USER LOGGED -->
          <div class="column is-6" 
            v-if="user.isLoggedin">
            <h3 class="title has-text-grey">
              <!-- DO YOU WANT TO DISCONNECT ? -->
              <!-- {{ getText('want_disconnect') }} -->
              {{ basicDict.want_disconnect[locale] }}

            </h3>
            <div class="box">

              <button
                class="button is-block is-primary is-primary-b is-fullwidth" 
                @click="goBack"
                >
                <span class="icon">
                  <i class="fas fa-arrow-left"></i>
                </span>
                <span>
                  <!-- Back -->
                  {{ basicDict.bo_back[locale] }}
                </span>
              </button>

              <br>

              <button 
                class="button is-block is-primary is-primary-b is-fullwidth" 
                type="submit" 
                @click="sendLogout">
                <span class="icon">
                  <i class="fas fa-sign-out-alt"></i>
                </span>
                <span>
                  <!-- {{ getText('disconnect') }} -->
                  {{ basicDict.disconnect[locale] }}
                </span>
              </button>
            </div>
          </div>

        </div>
      </div>
    </div>

  </section>

</template>

<script>
  import { mapState, mapGetters } from 'vuex'

  import { BasicDictionnary } from "~/config/basicDict.js" 

  export default {
    
    components: {
    },

    props: [
    ],

    data : () => {
      return {
        basicDict : BasicDictionnary, 
      }
    },

    computed: {

      ...mapState({
        log : state => state.log, 
        locale : state => state.locale,
        user: state => state.user.user,
      }),

      ...mapGetters({
        isConfigComplete : 'config/getIsConfigComplete',
      })

    },

    mounted(){

      // hack to scroll top because vue-router scrollBehavior thing doesn't seem to work on Firefox on Linux at least
      const int = setInterval(() => {
        if(window.pageYOffset < 50){
          clearInterval(int)
        } else {
          window.scrollTo(0, 0)
        }
      }, 100);

    },

    methods: {

      // getText(textCode) {
      //   return this.$store.getters['config/defaultText']({txt:textCode})
      // },

      goBack(e){
        e.preventDefault()
        this.$router.back()
      },

      sendLogout(e){
        e.preventDefault()
        this.userEmail = ''
        this.userPassword = ''
        this.$store.dispatch('user/logout')
        .then(() => {
          this.log && console.log('C-LogoutScreen-sendLogout / after then...')
          let isConfigComplete = this.isConfigComplete
          if ( !isConfigComplete ) {
            // this.$$nuxt.$router.push('/login')
          }
        })
        .catch(err => {
          this.log && console.log('C-LogoutScreen-sendLogout / err :', err)
        })
      }
    }

  }
</script>
