
<template>
  <el-form
    label-width="100px"
    :model="formData"
    :label-position="'left'"
    @submit.native.prevent="formSubmit"
    :rules="rules"
    ref="member-form"
  >
    <el-form-item label="Username" prop="username">
      <el-input
        placeholder="Username"
        v-model="form.username"
        :disabled="mode === 'edit'"
      ></el-input>
    </el-form-item>
    <el-form-item label="Name" prop="name">
      <el-input placeholder="Name" v-model="form.name"></el-input>
    </el-form-item>
    <el-form-item label="Email" prop="email">
      <el-input placeholder="Email" v-model="form.email"></el-input>
    </el-form-item>
    <el-form-item label="Password" prop="password" v-if="mode !== 'edit'">
      <el-input
        placeholder="Password"
        v-model="form.password"
        show-password
      ></el-input>
    </el-form-item>
    <el-button type="success" native-type="submit">Save</el-button>
  </el-form>
</template>

<script>
export default {
  name: "MemberForm",
  props: {
    formData: {
      type: Object,
      default: () => {},
    },
    mode: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      form: {},
      rules: {
        name: [
          { required: true, message: "Please input name", trigger: "blur" },
        ],
        email: [
          { required: true, message: "Please input email", trigger: "blur" },
          {
            trigger: "blur",
            validator: (rule, value, callback) => {
              const re =
                /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;

              if (!re.test(String(value).toLowerCase())) {
                callback(new Error("Email is not valid"));
              } else {
                callback();
              }
            },
          },
        ],
        password: [
          { required: true, message: "Please input password", trigger: "blur" },
          {
            trigger: "blur",
            validator: (rule, value, callback) => {
              const regexPassword = /^(?=.*[A-Z])(?=.*[a-z])(?=.*[0-9])(?=.*[^A-Za-z0-9]).{15,}$/;

              if (!regexPassword.test(String(value))) {
                callback(new Error('Password must contains characters, number, special symbols and length should be at least 15'))
              } else {
                callback();
              }
            },
          }
        ],
        username: [
          { required: true, message: "Please input username", trigger: "blur" },
          {
            min: 3,
            max: 20,
            message: "Length should be 3 to 20",
            trigger: "blur",
          },
        ],
      },
    };
  },
  mounted() {
    this.form = this.formData;
},
  methods: {
    formSubmit() {
      this.$refs["member-form"].validate((valid) => {
        if (valid) {
          console.log(this.form)
          if (this.mode === "edit") {
            this.$emit("submit-form", this.form)
          } else {
            this.$emit("form-submit", this.form);
          }
        }
      });
    },
  },
};
</script>

<style>
</style>
