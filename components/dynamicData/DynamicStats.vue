<template>
  <DynamicSearchScreenSqueleton 
    :filtersConfig="filtersConfig"
    mainClass="stat"
    >
    
    <!-- TEMPORARILY COMMENTED -->
    <SearchResultsStats
      :routeConfig="routeConfig"
    />

  </DynamicSearchScreenSqueleton>
</template>

<script>
import { mapState } from 'vuex'

import DynamicSearchScreenSqueleton from './DynamicSearchScreenSqueleton.vue'
import SearchResultsStats from './SearchResultsStats.vue';

export default {

  name: 'DynamicStats',

  props:[
    'routeConfig',
    'endPointConfig',
    'filtersConfig'
  ],

  beforeMount : function(){
    this.log && console.log('\nC-DynamicStats / beforeMount...')
  },

  data: () => {
    return   {
    }
  },

  components: {
    DynamicSearchScreenSqueleton, 
    SearchResultsStats
  },

  computed: {

    ...mapState({
      log : state => state.log, 
      locale : state => state.locale,
      breakpoint : state => state.breakpoint,
      user: state => state.user.user
    }),

  },
}
</script>

<style>

  .list .filter-feedback{
    padding-bottom: 0;
  }

</style>
