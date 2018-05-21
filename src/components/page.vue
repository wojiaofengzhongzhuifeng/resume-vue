<template>
  <div id="app" ref="xxxxx">

    <aside v-show="previewModel">
      <div class="userInformation">
          <div ref="imgCt" class="imgCt" v-show="showLogInUser">
          </div>
          <p v-show="showLogInUser">{{logInUser.email}}</p>
      </div>
      <ul>
        <li class="save">
          <el-button type="primary" @click="onclickSave">保存</el-button>
        </li>
        <li class="share">
          <el-button type="primary" @click="dialogVisible = true" v-show="currentUser.id">分享</el-button>
        </li>
        <li class="print">
          <el-button type="primary" @click="onClickPrint" v-show="currentUser.id">打印</el-button>
        </li>

        <li class="message" v-show="this.currentUser.id">
          <el-button type="primary" @click="dialogMessage=true">消息</el-button>
          <span class="number" v-show="this.allUserHrInformationArr.length > 0">{{this.allUserHrInformationArr.length}}</span>
        </li>
      </ul>
      <el-button type="danger" @click="onClickLogOut" v-show="currentUser.id">登出</el-button>
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
        <ul>
          <li v-for="(skill,index) in information.skills" class="skill">
            <div class="skill-h3-p-ct" >
              <i class="el-icon-close" @click="onClickCloseSkill(index)"  v-show="previewModel"></i>
              <h3 class="name"> <editableSpan  :informationDetail="skill.name"   @typeInput="listenInput($event, `skills.${index}.name`)"></editableSpan></h3>
              <p class="description"><editableSpan   :informationDetail="skill.description"  @typeInput="listenInput($event, `skills.${index}.description`)"></editableSpan></p>
            </div>
          </li>
        </ul>
        <div class="addSkills" @click="addExtraSkill"  v-show="previewModel">
          <i class="el-icon-circle-plus"></i>
        </div>
      </div>

      <div class="projects">
        <h2>项目经历</h2>
        <ul v-for="(work, index) in information.works">
          <li class="project">
            <i class="el-icon-close projectLi" @click="onClickCloseProject(index)"  v-show="previewModel"></i>
            <h3 class="projectName"> <editableSpan  :informationDetail="work.name"   @typeInput="listenInput($event, `works.${index}.name`)"></editableSpan></h3>
            <p class="link"> <editableSpan  :informationDetail="work.url"   @typeInput="listenInput($event, `works.${index}.url`)"></editableSpan></p>
            <p class="keywords"> <editableSpan  :informationDetail="work.needSkills"   @typeInput="listenInput($event, `works.${index}.needSkills`)"></editableSpan></p>
            <p class="description"> <editableSpan  :informationDetail="work.description"   @typeInput="listenInput($event, `works.${index}.description`)"></editableSpan></p>
          </li>
        </ul>
      </div>
      <div class="addWork" @click="addExtraWork">
        <div class="addSkills" v-show="previewModel">
          <i class="el-icon-circle-plus" ></i>
        </div>
      </div>


      <div class="contact-ct" v-show="!previewModel">
        <div class="contact">
          <h4>请联系我!</h4>
          <p>回车即可提交</p>
          <el-input placeholder="请输入联系方式" v-model="hrInformation.email"></el-input>
          <el-input placeholder="请输入内容" v-model="hrInformation.message" @keyup.enter.native="saveHrInformation"></el-input>

        </div>
      </div>
    </main>


    <el-dialog title="用户登录" :visible.sync="dialogFormVisible">
      <el-form >
        <el-form-item label="用户名" :label-width="formLabelWidth">
          <el-input auto-complete="off" v-model="logInUser.email"  ></el-input>
        </el-form-item>
        <el-form-item label="密码" :label-width="formLabelWidth">
          <el-input auto-complete="off" type="password"   v-model="logInUser.password" @keyup.enter.native="logInEnter"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="success" @click="onClickRegister">注 册</el-button>
        <el-button class="cancel" @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onClicklogInComfirm" ref="logInComfirm" id="asdf">确 定</el-button>
      </div>
    </el-dialog>



    <el-dialog title="用户注册" :visible.sync="dialogFormVisibleRegister">
      <el-form :model="form">
        <el-form-item label="用户名" :label-width="formLabelWidth">
          <el-input auto-complete="off" v-model="resigterUser.email"></el-input>
        </el-form-item>
        <el-form-item label="密码" :label-width="formLabelWidth">
          <el-input auto-complete="off"  type="password" v-model="resigterUser.password" ref="registerPassword"></el-input>
        </el-form-item>
        <el-form-item label="确认密码" :label-width="formLabelWidth">
          <el-input auto-complete="off"  type="password" v-model="resigterUser.comfirmPassword"  ref="registerPasswordComfirm" @keyup.enter.native="registerComfirm"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="primary" @click="onClickResigterComfirm"  id="asdfg" >确 定</el-button>
      </div>
    </el-dialog>


    <el-dialog title="用户消息" :visible.sync="dialogMessage">
      <ul>
        <li v-for="message in allUserHrInformationArr">
          <span>{{message.email}}</span> : <span>{{message.message}}</span>
        </li>
      </ul>
    </el-dialog>

    <el-dialog
      title="提示"
      :visible.sync="dialogVisible"
      width="50%">
      <p>请将下面内容分享</p>
      <el-input v-model="shareUrl"></el-input>
      <span slot="footer" class="dialog-footer">
    <el-button type="primary" @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
  </span>
    </el-dialog>


  </div>





</template>

<script>
  import AV from "leancloud-storage/dist/av-min"
  import editableSpan from "./editableSpan"
  import Identicon from "../../node_modules/identicon.js/identicon.js"
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
          // 预览模式
          return false
        } else {
          // 编辑模式
          return true
        }
      },
      showLogInUser:function(){
        if(this.currentUser.id === ""){
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
        dialogVisible:false,
        dialogMessage:false,
        hrInformation:{
          email:"",
          message:""
        },
        logInUser:{
          email:"",
          password:""
        },
        resigterUser:{
          email:"",
          password:"",
          comfirmPassword:""
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
          ],
          avatarNumber:""
        },
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
        allUserHrInformation:[],
        allUserHrInformationArr:[]
      }
    },
    methods:{
      registerComfirm(){
        let asdfg = document.querySelector("#asdfg")
        asdfg.click()
      },
      logInEnter(e){
        // this.$refs.edit.click()
        let asdf = document.querySelector("#asdf")
        console.log(asdf)
        asdf.click()
      },
      saveHrInformation(e){

        let previewUserId = this.previewUser.id


        var Information = AV.Object.extend('userAndHrContact');
        var information = new Information();
        information.set('userId',previewUserId);
        information.set('hrInformation',JSON.stringify(this.hrInformation));
        information.save().then(function (todo) {
          console.log('objectId is ' + todo.id);
          alert("成功向应聘者提交你的信息")
        }, function (error) {
          console.error(error);
        });

      },
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
      onClickCloseProject(index){
        // this.information.skills
        console.log(index)
        this.information.works.splice(index, 1)
      },
      onClickRegister(){
        this.dialogFormVisible = false
        this.dialogFormVisibleRegister = true
      },
      onClicklogInComfirm(){
        let logInEmail = this.logInUser.email
        let logInPassword = this.logInUser.password
        AV.User.logIn(logInEmail, logInPassword).then(function (loginedUser) {
          // 登录成功
          if(!loginedUser.attributes.information){
            this.currentUser.id =loginedUser.id
          } else{
            let information = JSON.parse(loginedUser.attributes.information)
            this.information = information
            this.currentUser.id =loginedUser.id
          }

          let shareUrl = location.origin + "?user_id=" + this.currentUser.id
          this.shareUrl = shareUrl
          this.dialogFormVisible = false
          this.createAvatar(this.currentUser.id)

          let _this = this

          var query = new AV.Query('userAndHrContact');
          query.find().then(function (todos) {
            let obj = {}
            let obj1 = {}
            for(let i=0;i<todos.length;i++){
              let userId = todos[i].attributes.userId
              let information = todos[i].attributes.hrInformation
              if(!obj[userId]){
                obj[userId] = []
                obj[userId].push(information)
              } else {
                obj[userId].push(information)
              }
            }

            for(let key in obj){
              if(key === _this.currentUser.id){
                _this.allUserHrInformation = obj[key]
              }
            }
            console.log("_this.allUserHrInformation")
            console.log(_this.allUserHrInformation)


            let arr = []
            // 将每项3message由json变成对象
            for(let i=0;i<_this.allUserHrInformation.length;i++){
              console.log(1)
              console.log(JSON.parse(_this.allUserHrInformation[i]))
              arr.push(JSON.parse(_this.allUserHrInformation[i]))
              console.log(arr)
              _this.allUserHrInformationArr = arr
            }



          })

        }.bind(this), function (error) {
          alert("账号或者密码错误")
        });
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



      onClickResigterComfirm(){
        if(this.resigterUser.password !== this.resigterUser.comfirmPassword){
          alert("密码不一致")
          this.resigterUser.password = ""
          this.resigterUser.comfirmPassword = ""
        } else {
          let registerEmail = this.resigterUser.email
          let registerPassword = this.resigterUser.password
          // 新建 AVUser 对象实例
          let user = new AV.User();
          // 设置用户名
          user.setUsername(registerEmail);
          // 设置密码
          user.setPassword(registerPassword);
          user.signUp().then(function (loginedUser) {
            this.dialogFormVisibleRegister = false
            this.dialogFormVisible = true
            // 保存到云端


          }.bind(this), function (error) {
            console.log(error)
          });
        }

      },
      _initAV(){
        var APP_ID = 'eqtNEtnaMt1BxxbMHdUKHQkR-gzGzoHsz';
        var APP_KEY = 'e99EIvtO7RE41XNDgye1AdwE';
        AV.init({
          appId: APP_ID,
          appKey: APP_KEY
        });
      },
      createAvatar(hash){
        var data = new Identicon(hash, 50).toString();
        var objE = document.createElement("div");
        objE.innerHTML = '<img width=50 height=50 src="data:image/png;base64,' + data + '">'
        if(this.$refs.imgCt.querySelector("div") === null){
          this.$refs.imgCt.appendChild(objE)
        } else {
          // 不做任何事情
        }

      },
      ContactMe(e){
        console.log(1)
        console.log(e)
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
    font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
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
    margin: 20px 0;
    /* box-shadow: 1px 1px 3px grey; */
    padding: 20px 0;
    box-shadow: 1px 1px 3px grey;
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
    .el-icon-close{
      display: none;
    }
    .addSkills{
      display: none;
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
    margin-top: 5px;
    text-align: center;
  }


  .projects{
    text-align: center;
    margin-top:20px;
  }
  .projects>ul{
    position: relative;
  }
  .project{
    box-shadow: 1px 1px 3px grey;
    border-radius: 5px;
    margin:5px 0;
    padding:5px;
    text-align: left;
    padding:10px;
  }
  .project p, .project h3{
    margin-bottom:10px;
    border-left:1px solid grey;
    padding-left:5px;
  }
  .project .start{
    display:flex;
  }
  .el-icon-close.projectLi{
    position: absolute;
    top:6px;
  }

  .cancel{
    color: #606266;
    width: 100px;
    height: 50px;
    padding: 20px;
    margin: 20px 0;
    background: #fff;
    border: 1px solid #dcdfe6;
  }


  .el-button--success {
    width: 100px;
    height: 50px;
    padding: 20px;
    margin: 20px 0;
    color: #fff;
    background-color: #67c23a;
    border-color: #67c23a;
  }
  .errorInformation{
    text-align: right;
    padding-top: 10px;
    color: #f56c6c;
    font-size: 16px;
  }
  .imgCt{
    padding-top:20px;
  }
  .contact-ct{
    display: flex;
    justify-content: center;

    color: #fff;
    padding-top:10px
  }
  .contact{
    background:#67c23a;
    box-shadow: 0 0 1px darkslategrey;
    text-align: center;
  }



  .message{
    position: relative;
  }
  .number{
    position:absolute;
    top: 17px;
    right: 48px;
    background: red;
    color: white;
    width: 20px;
    border-radius: 50%;
  }
  h1  span.editbleSpan button{
    border:1px solid red
  }
</style>

