<template>
  <ion-page>
    <ion-header :translucent="true" color="secundary">
      <ion-toolbar color="primary">
        <ion-title>Time Fighter</ion-title>
        <ion-buttons slot="primary">
          <ion-button color="primary" fill="solid" @click="infoAlert">
            <ion-icon :icon="infoIcon"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true"  color="tertiary">
      <ion-header color="secundary" class="ion-no-border ion-padding-top ion-padding-horizontal">
        <ion-grid >
          <ion-row>
            <ion-col class="ion-text-start" id="scoreBlink">
              Your Score: {{ score }}
            </ion-col>
            <ion-col class="ion-text-end">
              Time Left: {{ timeLeft }}
            </ion-col>
          </ion-row>
        </ion-grid>
      </ion-header>
    
      <div id="container">
        <ion-button id="TapMeButton" color="primary" @click="tap">TAP ME</ion-button>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import {
  alertController, createAnimation,
  IonButton,
  IonButtons, IonCol,
  IonContent, IonGrid,
  IonHeader,
  IonIcon,
  IonPage, IonRow,
  IonTitle,
  IonToolbar, toastController
} from '@ionic/vue';
import { defineComponent } from 'vue';
import {informationCircleOutline} from "ionicons/icons"
const INITIAL_TIME=60
const SCORE_INITIAL=0
export default defineComponent({
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButtons,
    IonButton,
    IonIcon,
    IonGrid,
    IonCol,
    IonRow
  },

  setup(){
    return {
      infoIcon: informationCircleOutline,
      started: false,
      counterInterval: null
    }
  },
  data(){
    return{
      score: SCORE_INITIAL,
      timeLeft: INITIAL_TIME,

    }
  },

  watch: {
    // cada vez que la pregunta cambie, esta función será ejecutada
    timeLeft: function (newtimeLeft) {
      if (newtimeLeft<=0){
        this.started = false
        clearInterval(this.counterInterval)
        this.showResult()
        this.timeLeft=INITIAL_TIME
        this.score=SCORE_INITIAL
      }

    }
  },
  methods: {
    bounce(){
    const animation = createAnimation()
        animation.addElement(document.getElementById('TapMeButton'))
        .duration(2000)
        .fromTo('transform', 'scale(2.0)', 'scale(1.0)')
        animation.play();
    },

    blink(){
      const animation = createAnimation()
      animation.addElement(document.getElementById('scoreBlink'))
          .duration(500)
          .fromTo('opacity', '0', '1')
      animation.play();
    },

    async infoAlert() {

      const alert = await alertController
          .create({
            header: 'Time Fighter 1.1',
            subHeader: 'Creat per Palanka',
            message: 'Podeu trobar el codi font al meu <a href="https://github.com/Palanka777/ComptadorIonic.git">repositori del github</a>',
            buttons: ['OK'],
          });
      await alert.present();
    },

    async tap() {

      this.blink()
      this.bounce()
      this.score++
      if(!this.started){
        this.counterInterval = setInterval( () => {
        this.timeLeft--}, 1000)
        this.started=true
      }
    },
    async showResult(){
      //TOAST
      const toast = await toastController.create({
        color: 'dark',
        duration: 2000,
        message: `Time's up! Your Score was: ${this.score}`,
        showCloseButton: true
      });

      await toast.present();

    },
  }
});
</script>

<style scoped>
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>