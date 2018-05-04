<template>
  <div id="app">
    <aside>
      <ul>
        <li class="save">
          <button>保存</button>
        </li>
        <li class="share">
          <button>分享</button>
        </li>
        <li class="print">
          <button>打印</button>
        </li>
        <li class="changeSkin">
          <button>换肤</button>
        </li>
      </ul>
      <span class="logOut">
          <button>登出</button>
      </span>
    </aside>
    <main>
      <div class="mainInformation">
        <h1>饶家俊</h1>
        <p>应聘职位: 前端工程师</p>
        <p>生日: 1995年12月 | 性别: 男 | 邮箱: 471412450@qq.com | 手机号码: 15702097950</p>
      </div>
      <editable-span></editable-span>

      <div class="skills">
        <h2>技能</h2>
        <ul>
          <li>
            <span class="name">静态页面制作</span>
            <span class="description">完美还原设计稿</span>
          </li>
          <li>
            <span class="name">静态页面制作</span>
            <span class="description">完美还原设计稿</span>
          </li>
          <li>
            <span class="name">静态页面制作</span>
            <span class="description">完美还原设计稿</span>
          </li>
          <li>
            <span class="name">静态页面制作</span>
            <span class="description">完美还原设计稿</span>
          </li>
        </ul>
      </div>

      <div class="projects">
        <h2>项目经历</h2>
        <ul>
          <li>
            <header>
              <div class="start">
                <h3 class="name">我的简历</h3>
                <span class="link">http://xxxx/xxx</span>
              </div>
              <div class="end">
                <span class="keywords">CSS3、jQuery、响应式</span>
              </div>
            </header>
            <p class="description">我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的</p>
          </li>

          <li>
            <header>
              <div class="start">
                <h3 class="name">我的简历</h3>
                <span class="link">http://xxxx/xxx</span>
              </div>
              <div class="end">
                <span class="keywords">CSS3、jQuery、响应式</span>
              </div>
            </header>
            <p class="description">我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的</p>
          </li>

          <li>
            <header>
              <div class="start">
                <h3 class="name">我的简历</h3>
                <span class="link">http://xxxx/xxx</span>
              </div>
              <div class="end">
                <span class="keywords">CSS3、jQuery、响应式</span>
              </div>
            </header>
            <p class="description">我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的</p>
          </li>

          <li>
            <header>
              <div class="start">
                <h3 class="name">我的简历</h3>
                <span class="link">http://xxxx/xxx</span>
              </div>
              <div class="end">
                <span class="keywords">CSS3、jQuery、响应式</span>
              </div>
            </header>
            <p class="description">我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的</p>
          </li>
        </ul>
      </div>
    </main>
  </div>

</template>

<script>

  import editableSpan from "./editableSpan"
  export default {
    name: 'page',
    data () {
      return {
        msg: 'Welcome to Your Vue.js App'
      }
    },
    components:{
      editableSpan
    }
  }
</script>

<style scoped>
  li{
    list-style: none;
  }
  *{
    margin: 0;
    padding:0;
  }
  #app{
    display:flex;
  }
  aside{
    width:100px;
    position: relative;
    height:100vh
  }
  main{
    flex:1;
  }
  main .mainInformation{
    display: flex;
    flex-direction: column;
    text-align: center;
  }
</style>
