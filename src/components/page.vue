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
        <h1>
          <editableSpan :informationDetail="information.name" @typeInput="listenInput($event,'name')"></editableSpan>
        </h1>
        <p>应聘职位: <editableSpan :informationDetail="information.job"  @typeInput="listenInput($event, 'job')"></editableSpan></p>
        <p>生日:<editableSpan :informationDetail="information.birthday"  @typeInput="listenInput($event, 'birthday')"></editableSpan>
          | 性别:<editableSpan :informationDetail="information.gender"  @typeInput="listenInput($event, 'gender')"></editableSpan>
          | 邮箱: <editableSpan :informationDetail="information.email"   @typeInput="listenInput($event, 'email')"></editableSpan>
          | 手机号码: <editableSpan :informationDetail="information.phone"   @typeInput="listenInput($event, 'phone')"></editableSpan>
        </p>
      </div>
      <div class="skills">
        <h2>技能</h2>
        {{information}}
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
        information:{
          name:"rjj",
          job: "打杂的",
          birthday:"1895年12月",
          gender:"女",
          email:"11111@qq.com",
          phone:"110"
        }
      }
    },
    methods:{
      listenInput(e, key){
        this.information[key] = e
      },
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
