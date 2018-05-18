<template>
    <span class="editbleSpan">
      <span class="show" @click="changeEditOn">{{informationDetail}}</span>
      <span class="input-button-ct">
        <input class="inputArea" v-show="edit">
        <button class="inputButton" type="primary" v-show="edit" @click="changeEditOffAndListenInputValue">编辑</button>
      </span>
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
            console.log(111)
            this.edit = false
            let inputValue = e.target.parentNode.querySelector("input").value
            this.$emit("typeInput", inputValue)
          }
        }
    }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .input-button-ct
    position: relative
    .inputArea
      -webkit-appearance: none;
      background-color: #fff;
      background-image: none;
      border-radius: 4px;
      border: 1px solid #dcdfe6;
      -webkit-box-sizing: border-box;
      box-sizing: border-box;
      color: #606266;
      display: inline-block;
      font-size: inherit;
      height: 40px;
      line-height: 40px;
      outline: 0;
      padding: 0 15px;
      -webkit-transition: border-color .2s cubic-bezier(.645,.045,.355,1);
      transition: border-color .2s cubic-bezier(.645,.045,.355,1);
      width: 20%;
    .inputButton
      position: absolute;
      display: inline-block;
      line-height: 1;
      white-space: nowrap;
      cursor: pointer;
      background: #fff;
      border: 1px solid #dcdfe6;
      color: #606266;
      -webkit-appearance: none;
      text-align: center;
      -webkit-box-sizing: border-box;
      box-sizing: border-box;
      outline: 0;
      margin: 0;
      -webkit-transition: .1s;
      transition: .1s;
      padding: 12px 20px;
      font-size: 14px;
      border-radius: 4px;
      color: #fff;
      background-color: #409EFF;
      border-color: #409EFF;
</style>
