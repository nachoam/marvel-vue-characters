<template>
  <div>
    <navbar :title="title" />
    <!-- Content -->
    <div id="app" style="padding-top:64px" v-cloak>
        <nuxt/>
    </div>
  </div>
</template>

<script>
import navbar from '../components/navbar.vue'


export default {
  components: { navbar },
  
  data() {
    return {
      title: 'Civil War',
    }
  },
  computed:{ },
  methods:{}
}
</script>
<style >


</style>
