<template>
  <v-layout row>
    <v-flex xs12 sm6 offset-sm3>
      <v-text-field solo label="Search" id="input" :append-icon="voice ? 'stop' : 'keyboard_voice'" :append-icon-cb="() => (voice = !voice)" prepend-icon="search" v-model="filterText"></v-text-field>
      <br>
      <v-card>
        <v-list two-line subheader>
          <transition-group name="slide-y-reverse-transition">
            <v-list-tile v-for="item in getData" v-bind:key="item.interviewRef" avatar @click="">
              <v-list-tile-avatar>
                <v-icon :color="btnColor" v-bind:class="[item.iconClass]">{{ icon }}</v-icon>
              </v-list-tile-avatar>
              <v-list-tile-content>
                <v-list-tile-title>{{ item.firstName +" "+ item.lastName }} </v-list-tile-title>
                <v-list-tile-sub-title>{{ item.interviewRef }}</v-list-tile-sub-title>
              </v-list-tile-content>
              <v-list-tile-action>
                <v-btn icon ripple @click="share">
                  <v-icon color="grey">share</v-icon>
                </v-btn>
              </v-list-tile-action>
            </v-list-tile>
            <v-list-tile v-if="getData.length === 0" key="data0">
              <v-list-tile-content>
                <v-list-tile-title class="text-xs-center">ไม่พบข้อมูล</v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
          </transition-group>
        </v-list>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
  let recognition;
  export default {
    props: ['items', 'icon', 'btnColor'],
    data() {
      return {
        filterText: '',
        topic: '',
        platform: true,
        voice: false,
        sortAsc: true
      }
    },
    watch: {
      voice: function(el) {
        if (el == true) {
          recognition.start();
        } else {
          recognition.stop();
        }
      }
    },
    computed: {
      getData() {
        let result = this.items;
        if (this.filterText) {
          result = result.filter(item => Object.keys(item).map((key) => item[key].includes(this.filterText)).includes(true));
        }
        let ascDesc = this.sortAsc ? 1 : -1;
        return result.sort((a, b) => ascDesc * a.interviewRef.localeCompare(b.interviewRef));
      }
    },
    methods: {
      share(index) {
        const title = "YWC15";
        const text = "ยินดีด้วยกับคุณ" + index.path[3].childNodes[2].childNodes[0].innerText;
        const url = "https://pwa-app-tests.firebaseapp.com/";
        if (this.platform == true) {
          try {
            navigator.share({
              title,
              text,
              url
            });
          } catch (error) {
            console.log('Error sharing: ' + error);
            return;
          }
        } else {
          var titleFB = title;
          var summary = text;
          var urlFB = url;
          var image = 'https://pwa-app-tests.firebaseapp.com/static/img/logoShare.png';
          window.open('http://www.facebook.com/sharer.php?s=100&p[title]=' + titleFB + '&p[summary]=' + summary + '&p[url]=' + urlFB + '&p[images][0]=' + image, 'sharer,toolbar=0,status=0,width=548,height=325');
        }
      }
    },
    created() {
      if (navigator.platform == "Win32" || navigator.platform == "MacIntel") {
        this.platform = false
      }
      recognition = new webkitSpeechRecognition();
      recognition.continuous = false;
      recognition.interimResults = false;
      recognition.lang = 'th-TH';
      recognition.onresult = event => {
        let interim_transcript = '';
        for (let i = event.resultIndex; i < event.results.length; ++i) {
          if (event.results[i].isFinal) {
            console.log(event.results[i][0].transcript)
            this.filterText += event.results[i][0].transcript;
            this.voice = false;
          } else {
            interim_transcript += event.results[i][0].transcript;
          }
        }
      };
      recognition.onspeechend = () => {
        this.voice = false;
      }
    }
  }
</script>
