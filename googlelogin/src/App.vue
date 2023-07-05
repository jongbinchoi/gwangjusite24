<template>
  <!-- content에 자신의 OAuth2.0 클라이언트ID를 넣습니다. -->
<meta name ="google-signin-client_id" content="892284951186-04edfrnds7d8ngftf21v0orcl1a9nqc4.apps.googleusercontent.com">
  <ul>
    <li id="GgCustomLogin" @click="loginWithGoogle">
      <span>Login with Google</span>
    </li>
  </ul>
</template>

<script>
export default {
  name: "GoogleSignIn",
  methods: {
    loginWithGoogle() {
      gapi.load("auth2", () => {
        gapi.auth2.init();
        const options = new gapi.auth2.SigninOptionsBuilder();
        options.setPrompt("select_account");
        options.setScope(
          "email profile openid https://www.googleapis.com/auth/user.birthday.read"
        );
        gapi.auth2
          .getAuthInstance()
          .signIn(options)
          .then(
            this.onSignIn,
            this.onSignInFailure
          );
      });
    },
    onSignIn(googleUser) {
      const access_token = googleUser.getAuthResponse().access_token;
      this.$http
        .get("https://people.googleapis.com/v1/people/me", {
          params: {
            personFields: "birthdays",
            key: "AIzaSyAbpzEXkZJDoDh4GnSHxozPzee7M54FHYQ",
            access_token: access_token
          }
        })
        .then((response) => {
          console.log(response.data);
          const profile = googleUser.getBasicProfile();
          console.log(profile);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    onSignInFailure(t) {
      console.log(t);
    }
  },
  mounted() {
    const script = document.createElement("script");
    script.src = "https://apis.google.com/js/platform.js";
    script.async = true;
    script.defer = true;
    document.body.appendChild(script);
  }
};
</script>
