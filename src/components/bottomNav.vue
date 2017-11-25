<template>
    <v-bottom-nav fixed shift :value="true" :active.sync="e2" :color="computedColor">
        <v-btn dark value="home">
            <span>home</span>
            <v-icon>home</v-icon>
        </v-btn>
        <v-btn dark value="content">
            <span>content</span>
            <v-icon>content_paste</v-icon>
        </v-btn>
        <v-btn dark value="design">
            <span>design</span>
            <v-icon>color_lens</v-icon>
        </v-btn>
        <v-btn dark value="marketing">
            <span>marketing</span>
            <v-icon>pie_chart</v-icon>
        </v-btn>
        <v-btn dark value="programming">
            <span>programming</span>
            <v-icon>code</v-icon>
        </v-btn>
          <v-btn dark value="allMajor">
            <span>all</span>
            <v-icon>subject</v-icon>
        </v-btn>
    </v-bottom-nav>
</template>

<<script>
    export default {
        data() {
            return {
                e2: 'home'
            }
        },
        watch: {
            e2: function(res) {
                this.$emit('dataMajor', res);
            }
        },
        computed: {
            computedColor() {
                switch (this.e2) {
                    case 'home':
                        return 'purple'
                        break
                    case 'allMajor':
                        return 'pink'
                        break
                    case 'content':
                        return 'light-blue'
                        break
                    case 'design':
                        return 'amber'
                        break
                    case 'marketing':
                        return 'red'
                        break
                    case 'programming':
                        return 'teal'
                        break
                }
            }
        }
    }
</script>
