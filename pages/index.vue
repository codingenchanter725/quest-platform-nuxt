<template>
  <v-main id="main" class="pa-0 ma-0">
    <v-container class="main_body d-flex flex-column justify-space-between">
      <div class="px-sm-4 px-3">
        <v-row align="center" class="header py-lg-5">
          <v-col cols="6" class="text-left px-0">
            <img
              src="~/assets/img/cello_logo_transparent.png"
              width="auto"
              height="55"
              alt="logo"
              class="logo-img"
            />
          </v-col>
          <v-col
            cols="6"
            class="d-flex px-0 align-center justify-end user-avatar"
          >
            <v-btn class="mx-2" fab small color="success">
              <v-icon color="grey darken-4" dense>
                mdi-account
              </v-icon>
            </v-btn>

            <v-btn
              tile
              outlined
              color="white"
              class="ml-sm-4 ml-1 px-sm-7 py-sm-4 px-4 py-2 login-btn"
              >LOGIN</v-btn
            >
          </v-col>
        </v-row>
      </div>
      <div class="wizard-panel">
        <v-stepper
          v-model="curr"
          vertical
          color="red"
          class="d-flex flex-column justify-space-between"
        >
          <div class="step-point-line">
            <v-stepper-step
              v-for="(step, n) in questions"
              :key="n"
              :complete="stepComplete(n + 1)"
              :step="n + 1"
              :color="stepStatus(n + 1)"
              complete-icon=" "
              class="py-0 pr-0"
            >
            </v-stepper-step>
            <nav />
          </div>
          <v-stepper-content
            v-for="(step, n) in questions"
            :key="'key-' + n"
            :step="n + 1"
            class="my-0"
          >
            <div class="question-panel">
              <v-row justify="center" class="question-mark-panel">
                <img
                  src="~/assets/img/Q-mark.png"
                  class="question-mark"
                  alt="question-mark"
                />
              </v-row>
              <div class="question-body mt-5 mt-md-8 scroll-container scroll-x-hidden">
                  <h2>
                    {{ step.question }}
                  </h2>
                  <p class="mt-sm-5 mb-0 text-center">
                    {{ step.hint }}
                  </p>
              </div>
            </div>
            <div
              class="answer-panel text-center"
              justify="center"
            >
              <input
                v-model="answers[n]"
                placeholder="Type your answer here"
                class="row px-5 py-2 my-5 mb-sm-5 mb-lg-10 mb-xl-12 text-center"
                @keyup="keypress"
                ref="answerInput"
              />
              <v-btn
                v-if="n + 1 < questions.length"
                color="success"
                tile
                class="px-sm-7 py-sm-5 px-5 py-3 text-capitalize"
                depressed
                @click="nextStep(n + 2)"
                >Submit</v-btn
              >
              <v-btn
                v-else
                color="success"
                class="px-sm-7 py-sm-5 px-5 py-3 text-capitalize"
                tile
                depressed
                @click="done()"
                >Finish</v-btn
              >
            </div>
          </v-stepper-content>
        </v-stepper>
      </div>
      <div class="px-sm-4 footer-panel">
        <v-row class="footer flex-column my-0 px-sm-0 px-4">
          <v-divider color="white" />
          <v-col cols="12" class="px-0 pb-0">
            <v-row>
              <v-col cols="6" class="text-left py-sm-2 py-0">
                <h3 class="timer white--text text-center">
                  <div id="timer" class="d-flex">
                    <span id="minutes" class="min-box pr-3"
                      >{{ minutes }}
                      <p>min</p></span
                    >
                    <span id="seconds" class="sec-box pl-3"
                      >{{ seconds }}
                      <p>secs</p></span
                    >
                  </div>
                </h3>
              </v-col>
              <v-col cols="6" class="text-right py-sm-2 py-0 pr-10">
                <img
                  src="~/assets/img/footer_logo_transparent.png"
                  width="100"
                  height="100"
                  class="footer-logo"
                  alt="footer-logo"
                />
              </v-col>
            </v-row>
          </v-col>
        </v-row>
      </div>
      <v-alert
        tile
        prominent
        min-height="50"
        min-width="350"
        type="success"
        origin="bottom-right"
        transition="slide-x-reverse-transition"
        mode="in-out"
        class="submit-alert"
        ref="submitAlert"
        :value="alertState"
      >
        Your answers submitted
      </v-alert>
      <v-alert
        tile
        prominent
        min-height="50"
        min-width="350"
        type="warning"
        origin="bottom-right"
        transition="slide-x-reverse-transition"
        mode="in-out"
        class="warn-alert"
        :value="warnAlertState"
      >
        Please type answer
      </v-alert>
      <v-dialog v-model="dialog" width="500px" max-width="90%">
        <v-card color="red" class="px-5 py-3">
          <p v-for="(answer, n) in answers" :key="n" class="mt-5 white--text">
            Answer{{ n + 1 }} : {{ answer }}
          </p>
        </v-card>
      </v-dialog>
    </v-container>
  </v-main>
</template>

<script>
  export default {
    data: function() {
      return {
        curr: 1,
        alertState: false,
        warnAlertState: false,
        timer: null,
        totalTime: 15 * 60,
        resetButton: false,
        title: "Let the countdown begin!!",
        answers: [],
        dialog: false,
        questions: [
          {
            question: "1: How will a decentralized aptent taciti sociosqu?",
            hint:
              "Duis hendrerit nisi ut purus semper, sit amet sodales ipsum tincidunt."
          },
          {
            question: "2: How will a decentralized aptent taciti sociosqu?",
            hint:
              "Duis hendrerit nisi ut purus semper, sit amet sodales ipsum tincidunt."
          },
          {
            question: "3: How will a decentralized aptent taciti sociosqu?",
            hint:
              "Duis hendrerit nisi ut purus semper, sit amet sodales ipsum tincidunt."
          },
          {
            question: "4: How will a decentralized aptent taciti sociosqu?",
            hint:
              "Duis hendrerit nisi ut purus semper, sit amet sodales ipsum tincidunt."
          },
          {
            question: "5: How will a decentralized aptent taciti sociosqu?",
            hint:
              "Duis hendrerit nisi ut purus semper, sit amet sodales ipsum tincidunt."
          }
        ]
      };
      s;
    },
    mounted() {
      this.startTimer();
    },
    computed: {
      minutes() {
        const minutes = Math.floor(this.totalTime / 60);
        return this.padTime(minutes);
      },
      seconds() {
        const seconds = this.totalTime - this.minutes * 60;
        return this.padTime(seconds);
      }
    },
    methods: {
      keypress(e) {
        if (e.keyCode === 13) {
          console.log(this.curr, this.questions.length);
          if (this.curr == this.questions.length) {
            this.done();
          } else {
            this.nextStep(this.curr + 1);
          }
        }
      },
      handle(value) {
        this.curr = value;
      },
      stepComplete(step) {
        return this.curr > step;
      },
      stepStatus(step) {
        return this.curr > step ? "rgb(100,214,121)" : "rgb(242,288,75)";
      },
      done() {
        if (!this.answers[this.curr - 1]) {
          this.warnAlertState = true;
          setTimeout(() => {
            this.warnAlertState = false;
          }, 3000);
        } else {
          this.alertState = true;
          this.dialog = true;
          setTimeout(() => {
            this.alertState = false;
            this.curr = 1;
            this.dialog = false;
            this.answers = [];
            this.resetTimer();
            this.startTimer();
          }, 3000);
        }
      },
      nextStep(step) {
        if (!this.answers[this.curr - 1]) {
          this.warnAlertState = true;
          setTimeout(() => {
            this.warnAlertState = false;
          }, 3000);
        } else {
          this.curr = step;
          this.resetTimer();
          this.startTimer();
        }
      },
      startTimer() {
        this.timer = setInterval(() => this.countdown(), 1000);
        this.resetButton = true;
        this.title = "Greatness is within sight!!";
      },
      stopTimer() {
        clearInterval(this.timer);
        this.timer = null;
        this.resetButton = true;
        this.title = "Never quit, keep going!!";
      },
      resetTimer() {
        this.totalTime = 15 * 60;
        clearInterval(this.timer);
        this.timer = null;
        this.resetButton = false;
        this.title = "Let the countdown begin!!";
      },
      padTime(time) {
        return (time < 10 ? "0" : "") + time;
      },
      countdown() {
        if (this.totalTime >= 1) {
          this.totalTime--;
        } else {
          this.totalTime = 0;
          this.resetTimer();
        }
      }
    }
  };
</script>

<style>
  html {
    overflow: hidden;
  }

  .v-stepper__step.v-stepper__step--inactive > span {
    background: rgb(130, 129, 129) !important;
  }

  .theme--light.v-stepper .v-stepper__step__step {
    color: transparent !important;
  }

  .v-stepper__step__step {
    width: 14px !important;
    height: 14px !important;
    min-width: 14px !important;
    margin-right: 0 !important;
  }
</style>

<style lang="scss" scoped>
  #main {
    background: url(~/assets/img/celo_dequest_bg_centred.jpg);
    background-size: 100% 100%;
    height: 100vh;
    width: 100%;
  }

  .main_body {
    height: 100vh;
  }

  .v-application--is-ltr .v-stepper--vertical .v-stepper__content {
    margin: -8px 36px -16px -36px;
    border-left: 0 !important;
  }
  
  .v-sheet.v-stepper {
    background: transparent;
  }

  .timer {
    font-size: 1.5rem;
    width: 90px;
  }

  .timer-rate {
    margin-top: -10px;
    font-size: 1.2rem;
    width: 90px;
  }

  .footer-logo {
    margin-top: -5%;
    position: relative;
    z-index: 10;
    @media (max-width: 1400px) {
      width: 15%;
      height: auto;
    }
    @media (max-width: 1200px) {
      width: 20%;
      height: auto;
      min-width: 60px;
      margin-top: -8%;
    }
  }

  .submit-alert,
  .warn-alert {
    position: absolute;
    bottom: 0;
    right: 0;
    z-index: 11;
  }

  .v-application--is-ltr .theme--light.v-stepper--vertical .v-stepper__content {
    position: absolute;
    width: 100%;
    top: 30%;
    transform: translate(0, -30%);
    @media (max-height: 850px) {
      top: 15%;
      transform: translate(0, -15%);
    }
    @media (max-height: 700px) {
      top: 30%;
      transform: translate(0, -30%);
    }
    @media (max-width: 940px) {
      padding-right: 30px;
    }
    @media (max-width: 650px) {
      padding-right: 0px;
    }
  }

  .wizard-panel {
    flex: 1;
  }

  .wizard-panel > div {
    height: 100%;
    background: none;
    box-shadow: none !important;
  }

  .v-stepper__step {
    flex-direction: row-reverse;
  }

  .v-stepper__step {
    z-index: 10;
  }

  .btn-submit {
    z-index: 10;
  }

  .question-panel,
  .answer-panel {
    position: relative;
    z-index: 10;
  }

  .question-mark-panel {
    margin-top: 20px;
    @media (max-width: 768px) {
      width: 20%;
      margin: auto;
    }
    @media (max-width: 650px) {
      width: 25%;
    }
    @media (max-width: 540px) {
      width: 20%;
    }
    img {
      max-height: 86px;
      @media (max-width: 1200px) {
        height: 64px;
      }
    }
  }

  .question-body {
    max-width: 650px;
    margin: 0 auto;
    padding: 0 20px;
    min-height: 200px;
    max-height: 350px;
    @media (max-width: 1400px) and (max-height: 700px) {
      min-height: 150px;
      max-height: 150px;
    }
    @media (max-width: 768px) {
      min-height: 200px;
      max-height: 350px;
    }
    @media (max-width: 450px) {
      min-height: 200px;
      max-height: 280px;
    }
    @media (max-width: 360px) {
      min-height: 200px;
      max-height: 200px;
    }

    font-family: EBGaramond;
    h2 {
      font-size: 45px;
      color: white;
      text-align: center;
      @media (max-width: 540px) {
        font-size: 35px;
      }
      @media (max-width: 360px) {
        font-size: 30px;
      }
    }

    p {
      color: white;
      text-align: center;
      font-size: 18px;
      @media (max-width: 540px) {
        font-size: 15px;
      }
    }
  }

  .answer-panel input {
    font-family: EBGaramond;
    font-size: 25px;
    margin: auto;
    color: rgb(180, 179, 179);
    border-bottom: 2px solid rgb(134, 136, 138);
    @media (max-width: 540px) {
      font-size: 20px;
    }
  }

  .answer-panel button {
    font-family: Jost;
    font-size: 20px;
    @media (max-width: 540px) {
      font-size: 17px;
    }
  }

  .login-btn {
    font-family: Jost;
  }

  .answer-panel input:focus-visible {
    outline: none;
  }

  .step-point-line {
    height: calc(100vh - 350px);
    max-height: 500px;
    min-height: 350px;
    top: 15%;
    padding-right: 50px;
    position: relative;
    display: flex;
    justify-content: space-between;
    flex-direction: column;
    @media (max-width: 650px) {
      padding-right: 30px;
    }
    @media (max-width: 450px) {
      padding-right: 15px;
    }
    @media (max-width: 360px) {
      padding-right: 10px;
    }
  }

  .step-point-line nav {
    height: 96%;
    width: 2px;
    background-color: rgb(106, 109, 112);
    position: absolute;
    top: 10px;
    right: 56px;
    @media (max-width: 650px) {
      right: 36px;
    }
    @media (max-width: 450px) {
      right: 21px;
    }
    @media (max-width: 360px) {
      right: 16px;
    }
  }
  
  .min-box,
  .sec-box {
    font-family: Jost;
    font-size: 28px;
    @media (max-width: 768px) {
      font-size: 25px;
    }
  }

  .min-box > p,
  .sec-box > p {
    font-size: 20px;
    line-height: 50%;
    @media (max-width: 768px) {
      font-size: 15px;
    }
  }

  .logo-img {
    @media (max-width: 768px) {
      width: 70%;
      min-width: 150px;
      height: auto;
    }
  }

  .user-avatar img {
    @media (max-width: 768px) {
      width: 40px;
      height: 40px;
    }
  }
</style>