<template>
  <div class="register">
    <section class="form_container">
      <div class="manage_tip">
        <span class="title">DataMigrate 資轉工具</span>
        <el-form
          :model="registerUser"
          status-icon
          :rules="rules"
          ref="registerForm"
          label-width="80px"
          class="registerForm"
        >
          <el-form-item label="用戶名" prop="name">
            <el-input
              v-model="registerUser.name"
              placeholder="請輸入用戶名"
            ></el-input>
          </el-form-item>
          <el-form-item label="企業郵箱" prop="email">
            <el-input
              v-model="registerUser.email"
              placeholder="請輸入郵箱"
            ></el-input>
          </el-form-item>
          <el-form-item label="密碼" prop="password">
            <el-input
              v-model="registerUser.password"
              placeholder="請輸入密碼"
              type="password"
            ></el-input>
          </el-form-item>
          <el-form-item label="確認密碼" prop="password2">
            <el-input
              v-model="registerUser.password2"
              placeholder="請確認密碼"
              type="password"
            ></el-input>
          </el-form-item>

          <el-form-item label="選擇身份">
            <el-select
              class="choice"
              v-model="registerUser.identity"
              placeholder="請選擇身份"
            >
              <el-option label="管理員" value="manager"></el-option>
              <el-option label="員工" value="employee"></el-option>
            </el-select>
          </el-form-item>

          <el-form-item>
            <el-button
              type="primary"
              class="submit_btn"
              @click="submitForm('registerForm')"
              >註冊</el-button
            >
          </el-form-item>
        </el-form>
      </div>
    </section>
  </div>
</template>


<script>
export default {
  name: 'register',
  data () {
    var validatePass2 = (rule, value, callback) => {
      if (value !== this.registerUser.password) {
        callback(new Error('兩次密碼輸入不一致！'));
      } else {
        callback();
      }
    };
    return {
      registerUser: {
        name: '',
        email: '',
        password: '',
        password2: '',
        identity: ''
      },
      rules: {
        name: [
          {
            required: true,
            message: '用戶名不能為空',
            trigger: 'blur'
          },

          {
            min: 2,
            max: 30,
            message: '用戶名長度在2到30個字符之間',
            trigger: 'blur'
          }
        ],
        email: [
          {
            type: 'email',
            required: true,
            message: '邮箱格式不正确',
            trigger: 'blur'
          }
        ],
        password: [
          {
            required: true,
            message: '密碼不能為空',
            trigger: 'blur'
          },
          {
            min: 6,
            max: 30,
            message: '長度在6到30位之間',
            trigger: 'blur'
          }
        ],
        password2: [
          {
            required: true,
            message: '確認密碼不能為空',
            trigger: 'blur'
          },
          {
            min: 6,
            max: 30,
            message: '長度在6到30位之間',
            trigger: 'blur'
          },
          {
            validator: validatePass2,
            trigger: 'blur'
          }
        ]
      }
    };
  },
  methods: {
    submitForm (formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.$axios.post('/api/users/register', this.registerUser).then(res => {
            this.$message({
              message: '賬號註冊成功！',
              type: 'success'
            });
          });

          this.$router.push('/login');
        }
      });
    }
  }
};
</script>

<style scoped>
.register {
  position: relative;
  width: 100%;
  height: 100%;
  background: url(../assets/bg.png) no-repeat center center;
  background-size: 100% 100%;
}
.form_container {
  width: 370px;
  height: 210px;
  position: absolute;
  top: 10%;
  left: 34%;
  padding: 25px;
  border-radius: 5px;
  text-align: center;
}
.form_container .manage_tip .title {
  font-family: "Microsoft YaHei";
  font-weight: bold;
  font-size: 26px;
  color: grey;
}
.registerForm {
  margin-top: 20px;
  /* background-color: rgb(204, 189, 189); */
  padding: 20px 40px 20px 20px;
  border-radius: 5px;
  box-shadow: 0px 5px 10px grey;
}
.registerForm .label {
  background: #fff;
}

.registerForm .choice {
  width: 230px;
}
.registerForm .submit_btn {
  width: 100%;
}
</style>