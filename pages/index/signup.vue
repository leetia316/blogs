<template>
  <div>
    <form class="form-signin" ref="formSignup">
      <h2 class="form-signin-heading">注册</h2>
      <label for="inputEmail" class="sr-only">Email address</label>
      <input type="email" id="inputEmail" name="email" class="form-control"
             placeholder="邮箱" required
             autofocus>
      <label for="inputName" class="sr-only">Full Name</label>
      <input type="text" id="inputName" name="name" class="form-control" placeholder="用户名" required
             autofocus>
      <label for="inputPassword" class="sr-only">Password</label>
      <input type="password" id="inputPassword" name="password" class="form-control" placeholder="密码"
             required>
      <button class="btn btn-lg btn-primary btn-block" type="submit" @click="signup($event,$refs.formSignup)">注册
      </button>
    </form>
  </div> <!-- /container -->
</template>

<script>
  import axios from '~plugins/axios'
  export default {
    layout: 'blog',
    created() {
      this.$store.commit('SET_MENU_NAME', '注册')
    },
    methods: {
      signup (event, form) {
        event.preventDefault()
        if (!this.verify(form.email.value, form.name.value, form.password.value)) return
        let body = {
          email: form.email.value,
          name: form.name.value,
          password: form.password.value
        }
        axios.post('/api/users', body).then(res => {
          res = res.data
          if (res.success) {
            this.$store.commit('SET_USER', res.user)
            localStorage.setItem('user', JSON.stringify(res.user))
            this.$router.replace({path: '/'})
          } else {
            alert(res.errors)
          }
        })
      },
      verify (email, name, password) {
        let str = null
        const myreg = /^([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+@([a-zA-Z0-9]+[_|\_|\.]?)*[a-zA-Z0-9]+\.[a-zA-Z]{2,3}$/;
        if (!myreg.test(email)) {
          str = "邮箱格式错误！"
        } else if (name === '') {
          str = "用户名不能为空！"
        } else if (password === '') {
          str = "密码不能为空！"
        } else if (name.length > 8) {
          str = "用户名不得长于8位！"
        } else if (name.trim() !== name) {
          str = "用户名不能含有空格！"
        } else if (password.length > 30) {
          str = "密码不得长于30位！"
        } else if (password.trim() !== password) {
          str = "密码不能含有空格"
        }
        if (str) {
          alert(str)
          return false;
        }
        return true;
      }
    }
  }
</script>

<style scoped>
  .form-signin {
    max-width: 330px;
    padding: 15px;
    margin: 0 auto;
  }

  .form-signin .form-signin-heading,
  .form-signin .checkbox {
    margin-bottom: 10px;
  }

  .form-signin .checkbox {
    font-weight: normal;
  }

  .form-signin .form-control {
    position: relative;
    height: auto;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
    padding: 10px;
    font-size: 16px;
  }

  .form-signin .form-control:focus {
    z-index: 2;
  }

  .form-signin input[type="email"] {
    margin-bottom: -1px;
    border-bottom-right-radius: 0;
    border-bottom-left-radius: 0;
  }

  .form-signin input[type="password"] {
    margin-bottom: 10px;
    border-top-left-radius: 0;
    border-top-right-radius: 0;
  }

</style>