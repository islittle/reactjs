
>注册，登录注册，前端逻辑和简单验证（仅供参考）

#login or register
*转载需加来源*

```
// 倒计时时间
let count = 60
// 防止未重新获取动态码频繁点击
let locked = false
// 初始化对比验证码值
let orginCode = ''


// 下面以注册为例
   toRegister () {
        const self = this
        let RegularN = /^1(3|5|7|8|4)[0-9]{9}$/ // 手机号简单验证
        let RegularD = /^[0-9]{6}$/   //六位手机动态码
        let Regular = /^[A-Za-z]{4}$/    //验证码
        let nameStr = document.querySelector('#fastName').value  //手机号
        let verCodeStr = document.querySelector('#verCode').value   //手机动态码
        let passCode = document.querySelector('#passwordCode').value   //密码
        let agreed = document.querySelector('#agreed').checked // 协议
        let codeStr = document.querySelector('#authcode2').value  //验证码
        // 协议验证
        if (!agreed) {
            let demo = document.querySelector('#fastNotice')
            demo.className = 'notice'
            demo.innerHTML = '请阅读并同意《服务协议》，再进行注册！'
            return false
        }
        // 手机号验证
        if (!nameStr.length) {
            this.errorText('手机号不能为空', 'show')
            return false
        } else if (!RegularN.test(nameStr)) {
            this.errorText('手机号格式错误', 'show')
            return false
        }

        // 这里再加一次验证是防止用户不获取动态码直接注册（因为注册的时候不需要验证码字段）
        if (!codeStr.length) {
            self.errorText('验证码不能为空', 'show')
            return false
        } else if (!Regular.test(codeStr)) {
            self.errorText('验证码为4位字母', 'show')
            return false
        }
        // 动态码验证
        if (!verCodeStr.length) {
            this.errorText('短信动态码不能为空', 'show')
            return false
        } else if (!RegularD.test(verCodeStr)) {
            this.errorText('短信动态码为6位数字', 'show')
            return false
        }
        // 密码验证
        if (!(passCode.length && passCode.length >= 6 && passCode.length <= 14)) {
            this.errorText('密码需要6-14位字符', 'show')
            return false
        }
        // 是不是获取验证码和到时验证
        if (!locked && count === 60) {
            // 没获动态码提示和获取动态码后判断
            orginCode ? this.errorText('请重新获取6位短信动态码', 'show') : this.errorText('请获取6位短信动态码', 'show')
            return false
        } else if (count < 60 && orginCode === verCodeStr) {
            // 60s内错误后不重新输入判断
            this.errorText('请核对6位短信动态码', 'show')
            return false
        }
        // 初始化动态码
        orginCode = verCodeStr
        // 下面是请求注册接口以及验证码错误判断
        let baseUrl = self.props.location.basename
        let fromUrl = self.props.location.query.from || (baseUrl ? BASE_URL + baseUrl : BASE_URL)
        self.props.actions.fetchRegister({
            mobile: nameStr,
            dynamicNumber: verCodeStr,
            password: passCode,
            repassword: passCode
        }, fromUrl).then(function (value) {
            self.errorFetch(value)
            value.msg.indexOf('动态码错误') !== -1 && (locked = false)
        })
    }

    
    // 绑定手机号输入keyup事件
     getVercode () {
        let Regular = /^1(3|5|7|8|4)[0-9]{9}$/
        let tel = document.querySelector('#fastName').value
        let getBtn = document.querySelector('#getCodeBtn')
        if (Regular.test(tel)) {
            count === 60 && (getBtn.className = 'getCodeBtn isTogo')
        } else if (tel.length > 11) {
            tel = tel.substr(0, 11)
            document.querySelector('#fastName').value = tel
            Regular.test(tel) && (getBtn.className = 'getCodeBtn isTogo')
        } else {
            getBtn.className = 'getCodeBtn'
        }
    }
    
   // 重新获取验证码
    changeCode () {
        const {actions} = this.props
        actions.fetchGetCaptcha()
    }


    //获取手机动态码
    getCodeFun (o) {
        const self = this
        o = o.target
        if (o.className.indexOf('isTogo') === -1) {
            return false
        }
        let Regular = /^[A-Za-z]{4}$/
        let nameStr = document.querySelector('#fastName').value
        let codeStr = document.querySelector('#authcode2').value
        let capId = document.querySelector('#authcodeIng2').getAttribute('data-code')

        if (!codeStr.length) {
            self.errorText('验证码不能为空', 'show')
            return false
        } else if (!Regular.test(codeStr)) {
            self.errorText('验证码为4位字母', 'show')
            return false
        }
        o.className = 'getCodeBtn'

        self.props.actions.fetchSendDyCodeByRegister({
            mobile: nameStr,
            captchaId: capId,
            captcha: codeStr
        }).then(function (value) {
            self.errorFetch(value)
            value.code === '0000' && (locked = true)
            if (value.msg === '验证码错误！') {
                o.className = 'getCodeBtn isTogo'
                o.innerHTML = '发送动态码'
                clearInterval(funTime)
                count = 60
                return false
            }
            let funTime = setInterval(function () {
                o.innerHTML = count + 's后重发'
                count--
                if (count === 0) {
                    o.className = 'getCodeBtn isTogo'
                    o.innerHTML = '发送动态码'
                    clearInterval(funTime)
                    count = 60
                }
            },
            1000)
        })
    }
    
    // 请示后错误提示
    errorFetch (value) {
        const self = this
        if (value.code === '9999') {
            self.errorText(value.msg, 'show')
            self.props.actions.fetchGetCaptcha()
        }
    }
    
    // 错误提示
    errorText (notice, type) {
        let demo = document.querySelector('#fastNotice')
        type === 'show' ? demo.className = 'notice' : demo.className = 'notice hide'
        notice.length && (demo.innerHTML = '提示信息：' + notice)
    }
```
