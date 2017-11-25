<template>
    <v-list-tile v-bind:key="item.interviewRef" avatar @click="">
        <v-list-tile-avatar>
            <v-icon v-bind:class="[iconClass]">{{ icon }}</v-icon>
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
</template>

<<script>
    export default {
        props: ['item', 'iconClass', 'icon'],
        data() {
            return {
                platform: true
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
        }
    }
</script>
