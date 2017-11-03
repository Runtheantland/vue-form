# vue表单校验
遇到了form表单提交的需求，找了vue的组件觉得不够灵活，有时间自己写了一个。
调用方法
全局引入注册：
import va from 'global/js/va'
va.install(Vue);

// 注册一个全局自定义指令 v-va
Vue.directive('va', {})

在每个需要校验的input加上 例如：
   <label>产品描述:</label>
   <input v-va:describe="[{'NonEmpty':''}]" placeholder="产品描述" v-model="data.describe"  tag="产品描述" type="textarea"/>
   <p class="red">{{va.story.message}}</p>
   校验类型:v-va:describe="[{'NonEmpty':''}]"具体错误类型请看下文  tag:错误提示信息描述 p:展示错误信息
   data绑定:  va  : {
                    stock          : {}
                },
                //sub提交时所用到
                msg : {
                    describe       : '描述'
                },
                data:{
                    describe:''
                }
   当点击but提交form是 触发所有校验       
                var va = self.va;

                for (var i in va) {
                    
                    if (!va[i].isPass) {

                        Vue.set(self.va[i], 'message', self.msg[i] + '不能为空');
                        
                    }
                }
组件里有一些常用正则：
//常用正则表
var regList = {
    tel     : /^1[34578]\d{9}$/,
    idCard  : /^(\d{15}$|^\d{18}$|^\d{17}(\d|X|x))$/,
    ImgCode : /^[0-9a-zA-Z]{4}$/,
    SmsCode : /^\d{4}$/,
    MailCode: /^\d{4}$/,
    UserName: /^[\w|\d]{4,16}$/,
    Password: /^[\w!@#$%^&*.]{6,16}$/,
    Mobile  : /^1[3|4|5|7|8]\d{9}$/,
    RealName: /^[\u4e00-\u9fa5|·]{2,16}$|^[a-zA-Z|\s]{2,20}$/,
    BankNum : /^\d{10,19}$/,
    Money   : /^([1-9]\d*|[0-9]\d*\.\d{1,2}|0)$/,
    Answer  : /^\S+$/,
    Mail    : /^([a-zA-Z0-9_\.\-])+\@(([a-zA-Z0-9\-])+\.)+([a-zA-Z0-9]{2,4})+$/
}
以及报错信息：// 获得不同的报错信息
function getErrMsg(vaForm, ruleType, ruleValue) {
    var tag     = vaForm.tag
    var errMsgs = {
        NonEmpty: `${tag}不能为空`,
        reg     : `${tag}格式错误`,
        limit   : `${tag}必须在${ruleValue[0]}与${ruleValue[1]}之间`,
        equal   : `两次${tag}不相同`,
        length  : `${tag}长度必须在${ruleValue[0]}与${ruleValue[1]}之间`,
        unique  : `${tag}不能相同`
    }
    return errMsgs[ruleType]
}

都可以自定义添加

具体调用校验方法 例如：v-va:stock="[{'NonEmpty':'Money'}]"  stock  在data中va对象校验字段 数组中是在va.js中的校验方法子段，可重复。优先级左到右。

       
   
       
