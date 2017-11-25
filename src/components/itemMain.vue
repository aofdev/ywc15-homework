<template>
    <v-layout row>
        <v-flex xs12 sm6 offset-sm3>
    
            <v-text-field solo label="Search" id="input" :append-icon="voice ? 'stop' : 'keyboard_voice'" :append-icon-cb="() => (voice = !voice)" prepend-icon="search" v-model="filterText"></v-text-field>
            <br>
            <v-card>
                <v-list two-line subheader>
                    <v-list-tile v-if="getData.length === 0" key="data1">
                        <v-list-tile-content>
                            <v-list-tile-title class="text-xs-center">ไม่พบข้อมูล</v-list-tile-title>
                        </v-list-tile-content>
                    </v-list-tile>
                </v-list>
                <v-list two-line subheader v-for="item in getData" v-bind:key="item.interviewRef">
                    <itemMajorList v-if="item.major == 'content'" :item="item" :iconClass="'light-blue white--text'" :icon="'content_paste'"></itemMajorList>
                    <itemMajorList v-if="item.major == 'design'" :item="item" :iconClass="'amber white--text'" :icon="'color_lens'"></itemMajorList>
                    <itemMajorList v-if="item.major == 'marketing'" :item="item" :iconClass="'red white--text'" :icon="'pie_chart'"></itemMajorList>
                    <itemMajorList v-if="item.major == 'programming'" :item="item" :iconClass="'teal white--text'" :icon="'code'"></itemMajorList>
                </v-list>
            </v-card>
        </v-flex>
    </v-layout>
</template>

<script>
    let recognition;
    export default {
        props: ['items'],
        data() {
            return {
                filterText: '',
                topic: '',
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
        created() {
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
        },
        components: {
            'itemMajorList': () =>
                import ('./itemMajorList.vue')
        }
    }
</script>
