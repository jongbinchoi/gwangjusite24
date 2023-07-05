<template>
  <div id ="wrap">
    <header>
      <a class="logo" href="App.vue">
        <img src="./assets/mainlogo.png" height="100px">
      </a>
      <nav>
        <ul class="nav-items">
          <li><a href="#"><i class="fa-regular fa-user"></i></a></li>
        </ul>
      </nav>
    </header>
  </div>

  <body>
    <div class="location">
  <div class="container-sm text-center" style="width:30%">
   <h2>로그인</h2>
   <div class="form-floating mb-3 color">
  <input type="email" class="form-control id" id="floatingInput" placeholder="name@example.com">
  <label for="floatingInput">ID</label>
</div>
<div class="form-floating mb-3 color">
  <input type="password" class="form-control" id="floatingPassword" placeholder="Password">
  <label for="floatingPassword">Password</label>
</div>
<div class="mb-3">
<button type="button" class="btn btn-primary">로그인</button>
</div>
<div class="d-grid gap-2 mar">
  
  <button class="btn btn-outline-secondary text-black" type="button "> Google 계정으로 가입하기</button>
  <button class="btn btn-outline-secondary text-black" type="button" style="background-color: #FEE500;"><i class="fa-solid fa-comment"></i>&nbsp;&nbsp;카카오 로그인</button>
  <button class="btn btn-outline-secondary" type="button "><i class="far fa-light fa-envelope evloctaion"></i> 이메일 계정으로 가입하기</button>

</div>
      </div>  
      
  </div>


  <meta name ="google-signin-client_id" content="892284951186-04edfrnds7d8ngftf21v0orcl1a9nqc4.apps.googleusercontent.com">
  <ul>
    <li id="GgCustomLogin" @click="loginWithGoogle">
      <span>Login with Google</span>
    </li>
  </ul>

</body>
  
  
  

</template>

<script>
/* global gapi */
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

<style>
*{ /* *전체 스타일에 적용하겠다. */
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  }
  
  header{
    width: 100%;
    height: 100px;
    z-index: 2000; /*쌓임 순서*/
    background-color: #fff;
    box-shadow: 0 5px 2px rgba(0, 0, 0, 0.05), 0 1px 0 rgba(0, 0, 0, 0.05);
  }
  
  li{
    list-style: none;/*정렬 되지 않은 태그라서 앞에 점이 붙어서 없애주겠다.*/
  }
  
  a{
    text-decoration: none;/*a태그가 텍스트일 때 항상 밑줄이 붙어서 없애줘야함.*/
  }
  
  nav{
    float: right;
  }
  
  .logo{
    display:inline-block;/*inline-block 크기설정가능해짐*/
    height: 100px;
    margin: 12px 0 12px 50px;
  }
  
  .logo > img { height: 80px; }
  
  .nav-items > li {
    display: inline-block;
  }
  
  .nav-items > li > a {
    line-height: 100px;
    padding-right: 50px;
    color: rgba(0, 0, 0, 0.4);
    font-size:25px;
  }
  
  .nav-items > li > a:hover {/*a:ho*/
    color: rgba(0, 0, 0, 0.8);
  }
  
.login{
  font-size: 50px;
  font-weight: bold;
  padding-top: 100px;

  position: relative;
  left: 750px;
}

.input-text{
  padding: 10px;
  font-size: 15px;
  background-color: #9EBDF8;
  border: 1.5px solid black;
  border-radius: 4px;
}

.location{
  position: relative;
  top: 150px;
}

.color>input{
  background-color: rgba(158, 189, 248, 0.3);
  
}

.btn-primary{
  width: 100%;
  
}

.id{
  margin-top: 30px ;
}
.mar{
  margin-top:70px;
}


</style>
