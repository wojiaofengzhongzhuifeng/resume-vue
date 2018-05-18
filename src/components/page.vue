<template>
  <div id="app">

    <aside v-show="previewModel">
      <div class="userInformation">
          <div class="img-ct">
            <img
              src="http://upload-images.jianshu.io/upload_images/5529438-af8ac43908596335.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"
              alt="" width="50" height="50">
          </div>
          <p>userName</p>
      </div>
      <ul>
        <li class="save">
          <el-button type="primary" @click="onclickSave">保存</el-button>
        </li>
        <li class="share">
          <el-button type="primary" @click="ifShowShare = !ifShowShare">分享</el-button>
        </li>
        <li class="print">
          <el-button type="primary" @click="onClickPrint">打印</el-button>
        </li>
      </ul>
      <el-button type="danger" @click="onClickLogOut" v-show="currentUser.id">登出</el-button>
    </aside>

    <main>
      {{currentUser}}
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
        <ul>
          <li v-for="(skill,index) in information.skills" class="skill">
            <div class="skill-h3-p-ct" >
              <i class="el-icon-close" @click="onClickCloseSkill(index)"></i>
              <h3 class="name"> <editableSpan  :informationDetail="skill.name"   @typeInput="listenInput($event, `skills.${index}.name`)"></editableSpan></h3>
              <p class="description"><editableSpan   :informationDetail="skill.description"  @typeInput="listenInput($event, `skills.${index}.description`)"></editableSpan></p>
            </div>
          </li>
        </ul>
        <div class="addSkills" @click="addExtraSkill">
          <i class="el-icon-circle-plus"></i>
        </div>
      </div>

      <div class="projects">
        <h2>项目经历</h2>
        <ul v-for="(work, index) in information.works">
          <li class="project">
            <header>
                <h3 class="name"> <editableSpan  :informationDetail="work.name"   @typeInput="listenInput($event, `works.${index}.name`)"></editableSpan></h3>
                <span class="link"> <editableSpan  :informationDetail="work.url"   @typeInput="listenInput($event, `works.${index}.url`)"></editableSpan></span>
                <span class="keywords"> <editableSpan  :informationDetail="work.needSkills"   @typeInput="listenInput($event, `works.${index}.needSkills`)"></editableSpan></span>
            </header>
            <p class="description"> <editableSpan  :informationDetail="work.description"   @typeInput="listenInput($event, `works.${index}.description`)"></editableSpan></p>
          </li>
        </ul>
      </div>
      <div class="addWork" @click="addExtraWork">
        <div class="addSkills">
          <i class="el-icon-circle-plus"></i>
        </div>
      </div>
    </main>
    <!--<div class="register" v-show="showRegister" @submit="submitRegister">-->
      <!--<h2>register</h2>-->
      <!--<form>-->
        <!--<p>email: <input type="text" ref="registerEmail"></p>-->
        <!--<p>password: <input type="password" ref="registerPassword"></p>-->
        <!--<button @click="onClickRegisterComfirm">comfirm</button>-->
      <!--</form>-->
    <!--</div>-->



    <!--<div class="logIn" v-show="showLogIn">-->
      <!--<h2>logIn</h2>-->
      <!--<form>-->
        <!--<p>email: <input type="text" ref="logInEmail"></p>-->
        <!--<p>password: <input type="password" ref="logInPassword"></p>-->
        <!--<button @click="onClicklogInComfirm">comfirm</button>-->
      <!--</form>-->
      <!--<button @click="onClickRegister">register</button>-->
    <!--</div>-->


    <el-dialog title="用户登录" :visible.sync="dialogFormVisible" >
      <el-form >
        <el-form-item label="用户名" :label-width="formLabelWidth">
          <el-input auto-complete="off" ref="logInEmail" v-model="logInUser.email"  ></el-input>
        </el-form-item>
        <el-form-item label="密码" :label-width="formLabelWidth">
          <el-input auto-complete="off" type="password"  ref="logInPassword"  v-model="logInUser.password"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="success" @click="onClickRegister">注 册</el-button>
        <el-button type="primary" @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onClicklogInComfirm">确 定</el-button>
      </div>
    </el-dialog>



    <el-dialog title="用户注册" :visible.sync="dialogFormVisibleRegister"  @submit="submitRegister">
      <el-form :model="form">
        <el-form-item label="用户名" :label-width="formLabelWidth">
          <el-input auto-complete="off"   ref="registerEmail"></el-input>
        </el-form-item>
        <el-form-item label="密码" :label-width="formLabelWidth">
          <el-input auto-complete="off"  ref="registerPassword" type="password"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onClickRegisterComfirm">确 定</el-button>
      </div>
    </el-dialog>



    <div class="shareBoard" v-if="ifShowShare">
      <h2>分享栏</h2>
      <p>请将下面链接分享给别人</p>
      <input type="text" :value="shareUrl">
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
      let previewUser = window.location.search.substring(window.location.search.indexOf("=") + 1)
      this.previewUser.id = previewUser
      var query = new AV.Query('User');
      if(!previewUser){
        // 不做任何事情
      } else{
        query.get(this.previewUser.id).then(function (user) {  // bug
          let information = JSON.parse(user.attributes.information)
          this.information = information
        }.bind(this), function (error) {
          // 异常处理
        });
      }
    },
    computed: {
      previewModel: function () {
        if(this.previewUser.id !== "" && this.currentUser.id === ""){
          return false
        } else {
          return true
        }

      }
    },
    data () {
      return {
        dialogFormVisible: false,
        dialogFormVisibleRegister:false,
        logInUser:{
          email:"",
          password:""
        },
        form: {
          name: '',
          region: '',
          date1: '',
          date2: '',
          delivery: false,
          type: [],
          resource: '',
          desc: ''
        },
        formLabelWidth: '100px',
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
        },
        ifShowShare:false,
        shareUrl:"",
        previewUser:{
          id:"",
          userName:""
        },
      }
    },
    methods:{
      listenInput(e, key){
        if(this.currentUser.id === "" & this.previewUser !== ""){
          // 预览模式
          this.dialogFormVisible = true
        } else {
          let point = `this.information.${key}`
          let result = this.information
          let keys = point.split(".").slice(2)
          for (let i = 0; i < keys.length; i++) {
            if (i !== keys.length - 1) {
              console.log(2)
              result = result[keys[i]]
            } else {
              console.log(1)
              result[keys[i]] = e
            }
          }
        }

      },
      onclickSave(){
        if(!this.currentUser.id){
          // 需要登录
          this.dialogFormVisible = true
        } else {
          let currentUser = AV.User.current();
          this.currentUser.id = currentUser.id
          var user = AV.Object.createWithoutData('User', this.currentUser.id);
          let StringObject = JSON.stringify(this.information)
          user.set('information',StringObject);
          // 保存到云端
          user.save().then(()=>{
            alert("保存成功")
          });
        }
      },
      onClickCloseSkill(index){
        // this.information.skills
        console.log(index)
        this.information.skills.splice(index, 1)

      },
      onClickRegister(){
        this.dialogFormVisible = false
        this.dialogFormVisibleRegister = true
      },
      onClicklogInComfirm(){
        let logInEmail = this.logInUser.email
        let logInPassword = this.logInUser.password
        AV.User.logIn(logInEmail, logInPassword).then(function (loginedUser) {
          if(!loginedUser.attributes.information){
            this.currentUser.id =loginedUser.id
          } else{
            this.dialogFormVisible = false
            console.log(3)
            let information = JSON.parse(loginedUser.attributes.information)
            this.information = information
            this.currentUser.id =loginedUser.id
          }
          console.log(4)
          let shareUrl = location.origin + "?user_id=" + this.currentUser.id
          this.shareUrl = shareUrl
          this.showLogIn = false
          this.showRegister = false
        }.bind(this), function (error) {
        });
      },
      onClickRegisterComfirm(){
        this.showLogIn = true
        this.showRegister = false
      },
      onClickPrint(){
        window.print()
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
      addExtraSkill(){
        let skill = {
            name:"技能名称",
            description:"技能描述"
          }
        this.information.skills.push(skill)
      },
      addExtraWork(){
        let work = {
          name:"项目名称",
          url:"预览地址",
          needSkills:"项目需要技能",
          description:"项目描述\n"
        }
        this.information.works.push(work)
      },
      submitRegister(ee){
        console.log(11111)
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



        var APP_ID = 'eqtNEtnaMt1BxxbMHdUKHQkR-gzGzoHsz';
        var APP_KEY = 'e99EIvtO7RE41XNDgye1AdwE';
        AV.init({
          appId: APP_ID,
          appKey: APP_KEY
        });


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
    width:200px;
    position: relative;
    height:100vh;
    text-align:center;
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

  .shareBoard {
    position: fixed;
    width: 200px;
    height: 100px;
    -webkit-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    left: 50%;
    top: 50%;
    box-shadow: 0 0 2px grey;
    background: grey;
    text-align: center;
  }


  @media print {
    aside{
      display:none;
    }
  }

  .img-ct{
    border-radius:50%;

    margin-top:50px;
  }

  .el-button--primary {
    color: #fff;
    background-color: #409EFF;
    border-color: #409EFF;
    width: 100px;
    height: 50px;
    padding: 20px;
    margin: 20px 0;
  }
  .el-button--danger {
    position: absolute;
    width: 100px;
    height: 50px;
    bottom: 20px;
    left: 50px;
  }



  .skills{
    text-align: center;

  }
  .skills ul{
    display:flex;
    flex-wrap:wrap;
    justify-content: space-around;

  }
  .skills ul li{
    width:48%;
    height:80px;
    margin:5px;
    box-shadow: 1px 1px 3px grey;
    border-radius: 5px;
    display: flex;
    justify-content: center;
    align-items: center;

  }
  .skill{
    position: relative;
  }
  .el-icon-close{
    position: absolute;
    top: 5px;
    right: 5px;
  }
  .addSkills{
    height: 50px;
    line-height: 50px;
    box-shadow: 0 1px 5px grey;
    width: 99%;
    margin-top: 5px;
    text-align: center;
  }


  .projects{
    text-align: center;
  }
  .project{
    box-shadow: 1px 1px 3px grey;
    border-radius: 5px;
    margin:5px 0;
    padding:5px;
  }
  .project header{
    display: flex;
    justify-content:space-between;
    height: 46px;
    line-height: 46px;
  }
  .project .start{
    display:flex;
  }
  .project .end{}
</style>

