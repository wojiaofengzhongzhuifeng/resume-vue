<template>
    <span class="editbleSpan">
      <span class="show" @click="changeEditOn">{{informationDetail}}</span>
      <input type="text" v-show="edit">
      <button  v-show="edit" @click="changeEditOffAndListenInputValue">编辑</button>
    </span>
</template>

<script>
    export default {
        props:["informationDetail"],
        name: "editableSpan",
        data:function(){
          return {
            edit: false
          }
        },
        methods:{
          changeEditOn(){
            this.edit = true
          },
          changeEditOffAndListenInputValue(e){
            this.edit = false
            let inputValue = e.target.parentNode.querySelector("input").value
            this.$emit("typeInput", inputValue)
          }
        }
    }
</script>

<style lang="stylus" rel="stylesheet/stylus">

</style>
