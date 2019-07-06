<template>
  <div class="menu">
    {{message}}
    {{name}}
    {{app.user}}
  </div>
</template> 

<script>
import bus from '@/bus/bus'
export default {
  inject:['name','app'],
  data() {
    return {
      message:'aaaaaa'
    }
  },
  created() {
    let self = this;
    bus.$on('todo',(e)=>{
      self.message = e
    })
  },
}
</script>

<style>

</style>
