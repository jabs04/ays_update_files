<template>
  <div>
    <b-modal ref="loginmodal" id="my-modal" title="Login">
      <form action="javascript:void(0)" class="row" method="post">
        <div class="form-group col-12">
          <label for="email" class="font-weight-bold">{{__('auth.email')}}</label>
          <input
            type="text"
            v-model="email"
            name="email"
            id="email"
            class="form-control"
            :placeholder="__('auth.email')"
          />
        </div>
        <div class="form-group col-12">
          <label for="password" class="font-weight-bold">{{__('auth.login_password')}}</label>
          <input
            type="password"
            v-model="password"
            name="password"
            id="password"
            class="form-control"
            :placeholder="__('auth.login_password')"
          />
        </div>
        <div class="col-12 mb-2 text-center">
          <button
            type="submit"
            class="btn btn-primary btn-block"
            @click="login"
          >
            {{__('auth.login')}}
          </button>
        </div>
        <div class="col-12 text-center">
          <label
            class="d-flex align-items-center justify-content-center flex-column"
            >{{__('auth.dont_have_account')}}
            <div v-b-modal="'my-modal1'" @click="closeloginmodal">
              <a href="#">{{__('auth.signup')}}</a>
            </div></label
          >
        </div>
         <div class="col-12 text-center">
          <a @click="redirectToLogin" class="btn btn-primary btn-sm float-right">Admin | Provider | Handyman Login</a>
        </div>
      </form>
    </b-modal>
  </div>
</template>

<script>
import { displayError } from '../../messages';
import { mapGetters } from "vuex";
export default {
  name: "Login",
  data() {
    return {
      email: "demo@user.com",
      password: "12345678",
      baseUrl: window.baseUrl
    };
  },
  computed: {
    ...mapGetters(["userData"]),
  },
  methods: {
    async login() {
      this.$store
        .dispatch("login", {
          email: this.email,
          password: this.password,
          loginfrom: 'vue-app'
        })
        .then((response) => {
          this.$router.push({ name: this.$route.name, query: { redirect: this.$route.path } });
          this.closeloginmodal();
        })
        .catch((err) => {
          if(err.response != undefined){
            displayError(err.response.data.message)
          }
        });
    },
    closeloginmodal() {
      this.$refs["loginmodal"].hide();
    },
    show() {
      this.$refs["loginmodal"].show();
    },
    redirectToLogin(){
      window.location.href = baseUrl + "/login";
    }
  },
};
</script>