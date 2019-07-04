<template>
  <button :class="'i-botton-size' + size" :disabled="disabled" @click="handleClick">
    <slot></slot>
  </button>
</template>

<script>
function oneOf(value,validList){
    for(let i = 0;i<validList.length;i++){
      if(value === validList[i]){
        return true
      }
    }
    return false   //结束
}
import bus from '@/bus/bus'
export default {
  props:{
    disabled:{
      type:Boolean,
      default:false
    },
    size:{
      validator(value){  //校验
        return oneOf(value,['small','middle','large'])
      },
      default:'large'
    }
  },
  data() {
    return {
      msg:'我是button'
    }
  },
  methods: {
    handleClick(event){
      // console.log(event)
      // console.log(123)
      // this.$emit('on-click',this.msg)
      bus.$emit('todo',this.msg)
    }
  },
}
</script>

<style scoped>
button{
  border-radius: 5px;
  cursor: pointer;
  outline: none;
}
.i-botton-sizelarge{
  padding: 12px;
}
.i-botton-sizemiddle{
  padding: 8px;
}
.i-botton-sizesmall{
  padding: 4px;
}
</style>
