<template>
  <v-app>
    <v-app-bar
      app
      class="blue-grey darken-4"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Motor Mapper"
          class="shrink mr-2"
          contain
          :src="require('./assets/logo.svg')"
          transition="scale-transition"
          width="40"
        />

        <h1 class="font-weight-black">MOTOR MAPPER</h1>
      </div>

      <v-spacer></v-spacer>

    </v-app-bar>

    <v-content>
      <v-container>
        <v-row>
          <v-col
            cols="12"
          >
            <p>Begin by going into the Betaflight CLI and typing <code>resource</code>. Enter in
             the resource id for each motor below. Now, <strong>with props
             off</strong>, goto the motors tab and spin up each motor. Use the dropdown to select
              which motor is spinning for each motor.</p>
          </v-col>
        </v-row>
        <v-form
          class="row"
          ref="form"
          v-model="valid"
        >
          <v-col
            cols="12"
            sm="12"
          >
            <v-switch
              v-model="resetMotors"
              :label="`Include commands to unset resources.`">
            </v-switch>
          </v-col>
          <v-col
            cols="12"
            sm="6"
          >
            <Motor
              :id="1"
              :parentData="motor1"
              @updateResource="motor1.resource = $event"
              @updateMotor="motor1.motor = $event"
            />
          </v-col>
          <v-col
            cols="12"
            sm="6"
          >
            <Motor
              :id="2"
              :parentData="motor2"
              @updateResource="motor2.resource = $event"
              @updateMotor="motor2.motor = $event"
            />
          </v-col>
          <v-col
            cols="12"
            sm="6"
          >
            <Motor
              :id="3"
              :parentData="motor3"
              @updateResource="motor3.resource = $event"
              @updateMotor="motor3.motor = $event"
            />
          </v-col>
          <v-col
            cols="12"
            sm="6"
          >
            <Motor
              :id="4"
              :parentData="motor4"
              @updateResource="motor4.resource = $event"
              @updateMotor="motor4.motor = $event"
            />
          </v-col>
          <v-col>
            <v-btn
              raised
              color="success"
              class="mr-4"
              @click="validate"
              :disabled="!valid"
            >
              Map Motors
            </v-btn>
          </v-col>
        </v-form>
        <v-dialog id="map" v-model="dialog" max-width="500">
          <v-card>
            <v-card-title class="headline">New Motor Mapping</v-card-title>
            <v-card-text>
              <p>Copy and past the following into the your CLI.</p>
              <pre>
                {{ msg }}
              </pre>
            </v-card-text>
          </v-card>
        </v-dialog>
      </v-container>
    </v-content>
  </v-app>
</template>

<style lang="scss" scoped>
  .hidden {
    display: none;
  }
  pre {
    text-align: left;
    white-space: pre-line;
    background: rgb(50,50,50);
    color: white;
    padding: 1rem;
    border-radius: 5px;
  }
</style>

<script>
import Motor from './components/Motor.vue';

const defaultTxt = 'resource';

export default {
  name: 'App',

  components: {
    Motor,
  },

  data: () => ({
    valid: true,
    mapped: false,
    dialog: false,
    resetMotors: false,
    newMap1: '',
    newMap2: '',
    newMap3: '',
    newMap4: '',
    reset: '',
    msg: null,
    motor1: {
      resource: null,
      motor: null,
    },
    motor2: {
      resource: null,
      motor: null,
    },
    motor3: {
      resource: null,
      motor: null,
    },
    motor4: {
      resource: null,
      motor: null,
    },
  }),

  methods: {
    validate() {
      if (this.$refs.form.validate()) {
        this.mapped = true;
        this.dialog = true;
        this.reset = '';
        const motors = [this.motor1, this.motor2, this.motor3, this.motor4];
        motors.sort((a, b) => ((a.motor > b.motor) ? 1 : -1));
        if (this.motor1.motor !== 'Motor 1') {
          if (this.resetMotors) {
            this.reset += `${defaultTxt} ${motors[0].motor.toUpperCase()} NONE\n`;
          }
          this.newMap1 = `${defaultTxt} ${motors[0].motor.toUpperCase()} ${motors[0].resource}\n`;
        }
        if (this.motor2.motor !== 'Motor 2') {
          if (this.resetMotors) {
            this.reset += `${defaultTxt} ${motors[1].motor.toUpperCase()} NONE\n`;
          }
          this.newMap2 = `${defaultTxt} ${motors[1].motor.toUpperCase()} ${motors[1].resource}\n`;
        }
        if (this.motor3.motor !== 'Motor 3') {
          if (this.resetMotors) {
            this.reset += `${defaultTxt} ${motors[2].motor.toUpperCase()} NONE\n`;
          }
          this.newMap3 = `${defaultTxt} ${motors[2].motor.toUpperCase()} ${motors[2].resource}\n`;
        }
        if (this.motor4.motor !== 'Motor 4') {
          if (this.resetMotors) {
            this.reset += `${defaultTxt} ${motors[3].motor.toUpperCase()} NONE\n`;
          }
          this.newMap4 = `${defaultTxt} ${motors[3].motor.toUpperCase()} ${motors[3].resource}\n`;
        }
        const mapMsg = this.reset + this.newMap1 + this.newMap2 + this.newMap3 + this.newMap4;
        this.msg = `${mapMsg.replace(/^\s+|\s+$/g, '')}\nsave`;
      }
    },
  },
};
</script>
