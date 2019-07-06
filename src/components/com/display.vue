<template>
  <div ref="display">121323243</div>
</template>

<script>
import Vue from 'vue'
import RandomStr from '../../utils/random_str'
export default {
  props:{
    code:{
      type:String,
      default:''
    }
  },
  data() {
    return {
      html:'',
      js:'',
      css:'',
      component:null,
      id:RandomStr()
    }
  },
  methods: {
    //source: .vue 文件代码 即props : code
    //type: 分割的部分 也就是template script style
    getSource(source,type){
      const regex = new RegExp(`<${type}>[^>]*>`)
      let openingTag = source.match(regex)  //使用正则表达式模式对字符串执行查找，并将包含查找的结果作为数组返回。
      if(!openingTag){
        return ""
      }else{
        openingTag = openingTag[0]
      }
      return source.slice(
        source.indexOf(openingTag)+openingTag.length,
        source.lastIndexOf(`<${type}>`)
      )
    },
    // splitCode(){}  //split 用于把字符串分割成字符串数组String.split() 执行的操作与 Array.join 执行的操作是相反的
    splitCode(){
      const script = this.getSource(this.code,"script").replace(
        /export default/,
        "return"
      )
      const style = this.getSource(this.code,"style")
      const template = `<div id="app">${this.getSource(this.code,template)}</div>`
      this.html = template
      this.js = script
      this.css = style
    },
    renderCode(){
      this.splitCode()
      if(this.html !== "" && this.js !==""){
        const parseStrToFunction = new Function(this.js)()
        parseStrToFunction.template = this.html
        const Component = Vue.extend(parseStrToFunction)
        this.component = new Component().$mount()
        this.$refs.display.appendChild(this.component.$el)
        if(this.css !== ""){
          const style = document.createElement('style')
          style.type = 'text/css'
          style.id = this.id
          style.innerHTML = this.css
          document.getElementsByTagName('head')[0].appendChild(style)
        }
      }
    },
    destroyedCode() {
      const $target = document.getElementById(this.id)
      if($target){
        $target.parentNode.removeChild($target)
      }
      if(this.component){
        this.$refs.display.removeChild(this.component.$el)
        this.component.$destroyed()
        this.component = null;
      }
    },

  },
  mounted() {
    this.renderCode()
  },
  watch: {
    code(){
      this.destroyedCode()
      this.renderCode()
    }
  },
  Beforedestroyed() {
    this.destroyedCode()
  },
}
</script>

<style>

</style>
