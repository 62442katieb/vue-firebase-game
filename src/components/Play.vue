<template name="play">
  <div id="play">
    <div  class="container">
    <!-- Modal Component -->
    <b-modal id="levelUp" ref="levelUp" title="You've Levelled Up!" ok-only>
      <p class="my-4">
        <h3>Level {{currentLevel.level}}</h3>
        <img :src="currentLevel.img" width="120px" height="120px"/>
        <p class="lead">You've unlocked: {{currentLevel.character}}</p>
      </p>
    </b-modal>

    <div class="main pt-3">

      Add your stuff here

      <b-alert :show="dismissCountDown"
         :variant="score.variant"
         class="toast"
         @dismissed="dismissCountdown=0"
         @dismiss-count-down="countDownChanged">
         {{score.message}}
      </b-alert>

    </div>

  </div>
  </div>
</template>

<style>

.btn:not(:disabled):not(.disabled) {
    cursor: pointer;
}

#bottonNav {
  position: fixed !important;
  bottom: 0;
  width: 100%;
  height: 56px;
}

.abtext {
  padding-bottom: 60px;
}

.inputManual {
  width: 75px;
}

  h4.left-align {
    text-align: left;
    padding-top: 30px;
  }

  .nl-Value {
    color: #dc3545;
    cursor: pointer;
    background-color: #ffc1075e;
    padding: 3px;
    border-style: solid;
    border-color: #ffc1075e;
    border-radius: 5px;
    border-width: thin;
    }

  .selectedNumber {
    border-width: thick;
    border-color: #dc3545;
    }

  .bshelf {
    height: 250px;
    position: absolute;
  }

  .text-justify{
    padding-bottom: 40px;
  }

</style>

<script>
  import Vue from 'vue';
  // import _ from 'lodash';
  // import axios from 'axios';
  import nlp from 'compromise';
  import { VueHammer } from 'vue2-hammer';
  import GridLoader from 'vue-spinner/src/PulseLoader';
  import { db } from '../firebaseConfig';
  // import config from '../config';

  nlp.plugin({
    regex: {
      '[0-9]+': 'Value',
    },
  });

  window.db = db;

  Vue.use(VueHammer);

  Vue.use(require('vue-shortkey'));

  /* function randomInt(min, max) {
    return Math.floor(Math.random() * ((max - min) + 1)) + min;
  } */


  export default {
    name: 'play',
    props: ['userInfo', 'userData', 'levels', 'currentLevel', 'anonID'],
    data() {
      return {
        startTime: null,
        dismissSecs: 1,
        dismissCountDown: 0,
        score: {
          variant: 'warning',
          message: '',
        },
        status: 'loading',
      };
    },
    computed: {

    },
    watch: {
      currentLevel() {
        // console.log('detected change', this.userData.score, this.currentLevel.min);
        if (this.userData.score === this.currentLevel.min && this.currentLevel.min) {
          this.$refs.levelUp.show();
          db.ref(`/users/${this.userInfo.displayName}`).child('level').set(this.currentLevel.level);
        }
      },
    },
    mounted() {
      this.startTime = new Date();
    },
    components: { GridLoader },
    directives: {
    },
    methods: {
      showAlert() {
        this.dismissCountDown = this.dismissSecs;
      },
    },
  };
</script>
