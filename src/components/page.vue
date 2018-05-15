<template>
  <div id="app">
    <aside>
      <ul>
        <li class="save">
          <button @click="onclickSave">save</button>
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
          <button @click="onClickLogOut" v-show="currentUser.id">logOut</button>
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
        <ul v-for="(skill,index) in information.skills">
          <li>
            <span class="name"> <editableSpan  :informationDetail="skill.name"   @typeInput="listenInput($event, `skills.${index}.name`)"></editableSpan></span>
            <span class="description"><editableSpan   :informationDetail="skill.description"  @typeInput="listenInput($event, `skills.${index}.description`)"></editableSpan></span>
          </li>
        </ul>
      </div>

      <div class="projects">
        <h2>项目经历</h2>
        <ul v-for="(work, index) in information.works">

          <li>
            <header>
              <div class="start">
                <h3 class="name"> <editableSpan  :informationDetail="work.name"   @typeInput="listenInput($event, `works.${index}.name`)"></editableSpan></h3>
                <span class="link"> <editableSpan  :informationDetail="work.url"   @typeInput="listenInput($event, `works.${index}.url`)"></editableSpan></span>
              </div>
              <div class="end">
                <span class="keywords"> <editableSpan  :informationDetail="work.needSkills"   @typeInput="listenInput($event, `works.${index}.needSkills`)"></editableSpan></span>
              </div>
            </header>
            <p class="description"> <editableSpan  :informationDetail="work.description"   @typeInput="listenInput($event, `works.${index}.description`)"></editableSpan></p>
          </li>
        </ul>
      </div>
    </main>
    <div class="register" v-show="showRegister" @submit="submitRegister">
      <h2>register</h2>
      <form>
        <p>email: <input type="text" ref="registerEmail"></p>
        <p>password: <input type="password" ref="registerPassword"></p>
        <button @click="onClickRegisterComfirm">comfirm</button>
      </form>
    </div>
    <div class="logIn" v-show="showLogIn">
      <h2>logIn</h2>
      <form>
        <p>email: <input type="text" ref="logInEmail"></p>
        <p>password: <input type="password" ref="logInPassword"></p>
        <button @click="onClicklogInComfirm">comfirm</button>
      </form>
      <button @click="onClickRegister">register</button>
    </div>
  </div>

</template>

<script>
  import AV from "leancloud-storage/dist/av-min"
  import editableSpan from "./editableSpan"
  export default {
    name: 'page',
    created(){
      this._initAV()
    },
    mounted(){
      let currentUser = AV.User.current();
      console.log(currentUser)
      this.currentUser.id = currentUser.id
      var query = new AV.Query('User');
      query.get(this.currentUser.id).then(function (user) {
        let information = JSON.parse(user._hashedJSON.information)
        this.information = information
      }.bind(this), function (error) {
        // 异常处理
      });
    },
    data () {
      return {
        information:{
          name:"name",
          job: "job",
          birthday:"bir",
          gender:"gender",
          email:"email",
          phone:"phone",
          skills:[
            {
              name:"html",
              description:"还原静态页面"
            },
            {
              name:"css",
              description:"完成常见动画"
            },
            {
              name:"js",
              description:"熟悉常见的数据"
            },
            {
              name:"VUe",
              description:"熟练使用vue"
            },
          ],
          works:[
            {
              name:"vue简历",
              url:"www.baidu.com",
              needSkills:"js, vue, jQuery",
              description:"我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的\n"
            }
          ]
        },
        showRegister:false,
        showLogIn:false,
        currentUser:{
          id:"",
          userName:""
        }
      }
    },
    methods:{
      listenInput(e, key){
        let point = `this.information.${key}`
        let result = this.information
        console.log("point")
        console.log(point)
        console.log("result")
        console.log(result)
        let keys = point.split(".").slice(2)
        console.log("keys")
        console.log(keys)
        for (let i = 0; i < keys.length; i++) {
          if (i !== keys.length - 1) {
            console.log(2)
            result = result[keys[i]]
          } else {
            console.log(1)
            result[keys[i]] = e
          }
        }

        //
        // console.log(point)
        // point = point.replace(reg, (matchStr, matchIndex)=>{
        //   let frontEnd = point.split(matchStr)
        //   let pointNum = matchIndex - 1
        //   let middle = point[pointNum] + point[matchIndex]
        //   middle = middle.slice(1)
        //   middle = `[${middle}]`
        //   console.log(middle)
        //   frontEnd.splice(1, 0, middle)
        //   frontEnd[0] = frontEnd[0].slice(0, length - 1)
        //   let frontEndStr = frontEnd.join("")
        // })
      },
      onclickSave(){
        let currentUser = AV.User.current();
        if(currentUser){
          this.currentUser.id = currentUser.id
          var user = AV.Object.createWithoutData('User', this.currentUser.id);
          user.set('information',this.information);
          // 保存到云端
          user.save().then(()=>{
            alert("保存成功")
          });
        } else {
          this.showLogIn = true
        }


      },
      onClickRegister(){
        this.showLogIn = false
        this.showRegister = true
      },
      onClicklogInComfirm(){
        let logInEmail = this.$refs.logInEmail.value
        let logInPassword = this.$refs.logInPassword.value
        AV.User.logIn(logInEmail, logInPassword).then(function (loginedUser) {
          if(!loginedUser._hashedJSON.information){
            this.currentUser.id =loginedUser.id
          } else{
            let information = JSON.parse(loginedUser._hashedJSON.information)
            this.information = information
            this.currentUser.id =loginedUser.id
          }
          this.showLogIn = false
          this.showRegister = false

        }.bind(this), function (error) {
          console.log("error")
          alert("userName or password error")
        });
      },
      onClickRegisterComfirm(){
        this.showLogIn = true
        this.showRegister = false
      },
      onClickLogOut(){
        AV.User.logOut();
        this.currentUser.id=''
        this.information = {
          name:"name",
          job: "job",
          birthday:"bir",
          gender:"gender",
          email:"email",
          phone:"phone",
          skills:[
            {
              name:"html",
              description:"还原静态页面"
            },
            {
              name:"css",
              description:"完成常见动画"
            },
            {
              name:"js",
              description:"熟悉常见的数据"
            },
            {
              name:"VUe",
              description:"熟练使用vue"
            },
          ],
          works:[
            {
              name:"vue简历",
              url:"www.baidu.com",
              needSkills:"js, vue, jQuery",
              description:"我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的我是怎么做这个项目的\n"
            }
          ]
        }
        alert("you are already logOut")
      },


      submitRegister(ee){
        this.showRegister = false
        let registerEmail = this.$refs.registerEmail.value
        let registerPassword = this.$refs.registerPassword.value
        // 新建 AVUser 对象实例
        let user = new AV.User();
        // 设置用户名
        user.setUsername(registerEmail);
        // 设置密码
        user.setPassword(registerPassword);
        user.signUp().then(function (loginedUser) {
          console.log(loginedUser);
        }, function (error) {
          console.log("erroe")
          console.log(error)
        });
      },
      _initAV(){
        if(AV){
          console.log("no av")
          // init leanCloud
          var APP_ID = 'eqtNEtnaMt1BxxbMHdUKHQkR-gzGzoHsz';
          var APP_KEY = 'e99EIvtO7RE41XNDgye1AdwE';
          AV.init({
            appId: APP_ID,
            appKey: APP_KEY
          });
        } else {
          //
          console.log("have av")
        }

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
  #app .register{
    position: fixed;
    top: 0;
    left:0;
    background: grey;
    width:100%;
    height:100vh;
  }
  #app .logIn{
    position: fixed;
    top: 0;
    left:0;
    background: grey;
    width:100%;
    height:100vh;
  }
</style>

