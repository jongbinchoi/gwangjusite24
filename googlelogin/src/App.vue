<template>
  <div>
    <google-signin-button
      client-id="716095266829-9kjr4qp4r3cacerpvh0ghp8gjujdn2am.apps.googleusercontent.com"
      @success="onSuccess"
      @failure="onFailure"
    ></google-signin-button>

    <div v-if="isSignedIn">
      <p>Signed in as: {{ user.name }}</p>
      <button @click="signOut">Sign out</button>
    </div>
  </div>
</template>

<script>
/* eslint-disable */

import GoogleSignInButton from 'vue-google-signin-button';

export default {
  components: {
    GoogleSignInButton,
  },

  data() {
    return {
      isSignedIn: false,
      user: null,
    };
  },

  mounted() {
    gapi.load('auth2', () => {
      gapi.auth2.init({
        client_id: '716095266829-9kjr4qp4r3cacerpvh0ghp8gjujdn2am.apps.googleusercontent.com',
      });
    });
  },

  methods: {
    onSuccess(user) {
      this.isSignedIn = true;
      this.user = user.getBasicProfile();
    },

    onFailure(error) {
      console.error(error);
    },

    signOut() {
      gapi.auth2.getAuthInstance().signOut().then(() => {
        this.isSignedIn = false;
        this.user = null;
      });
    },
  },
};
</script>
