<template>
<div id="nlu">
  <v-container grid-list-xl fluid>
      <v-widget>
        <div slot="widget-content">
          <v-layout row wrap>
        <v-flex lg8 xs12>
          <v-layout row>
                  <v-flex xs4>
                    <v-subheader>Analyse an article or other content </v-subheader>
                  </v-flex>
                  <v-flex xs8>
                    <v-text-field v-model="nlu_text"
                      name="nlu_text"
                      label="Insert your text"
                      value="Insert your text"
                      color="teal"
                      multi-line
                      rows="10"
                      v-class="hello"
                    ></v-text-field>
                  </v-flex>
          </v-layout>
          <v-layout row wrap="">
            <v-flex xs12>
                    <div class="text-xs-right">
                      <v-btn color="info" round @click="analyze()">ANALYZE</v-btn>
                    </div>
            </v-flex>
          </v-layout>
          <v-layout row wrap v-if="loading === true">
            <v-flex lg12>
              <div class="text-xs-center">
                <v-progress-circular indeterminate :width="3" color="red"></v-progress-circular>
              </div>
            </v-flex>
          </v-layout>
          <v-layout row wrap justify-right v-if="result_ok === true">
            <div id="nlu_content">
              <v-tabs icons-and-text centered dark color="purple">
                <v-tabs-slider color="yellow"></v-tabs-slider>
                <v-tab href="#tab-0">
                  Keywords
                </v-tab>
                <v-tab href="#tab-1">
                  Entities
                </v-tab>
                <v-tab href="#tab-2">
                  Sentiment
                </v-tab>
                <v-tab href="#tab-3">
                  Semantic Roles
                </v-tab>
                <v-tab href="#tab-4">
                  Concepts
                </v-tab>
                <v-tab-item
                  :key="tab-0"
                  :id="'tab-0'"
                >
                <v-card flat>
                    <v-layout row v-for="keyword in keywords">
                      <v-flex lg3>{{ keyword.value }}</v-flex>
                      <v-flex lg6> <v-progress-linear v-bind:value="keyword.pct * 100" color="success"></v-progress-linear> </v-flex>
                      <v-flex lg3>{{ keyword.pct }} </v-flex>
                    </v-layout>
                  </v-card>
                  
                </v-tab-item>
              </v-tabs>  
            </div>
          </v-layout>
        </v-flex>
      </v-layout>
        </div>
      </v-widget>
  </v-container>
</div>

</template>

<script>
import VWidget from '@/components/VWidget';
export default {
  components: {
    VWidget
  },
  data: () => ({
    nlu_text: "",
    loading: false,
    result_ok: false,
    keywords: [{value: 'Officer Dana Deasy', pct: 0.81}, {value: 'Secretary Patrick Shanahan', pct: 0.73}, {value: 'Joint Artificial Intelligence Center', pct: 0.72}],
    tab: null
  }),
  computed: {
  },

  created () {
  },

  methods: {
    analyze: function (e) {
      console.log(this.nlu_text)
      this.loading = true
      const vm = this
      setTimeout(() => {
        vm.loading = false
        vm.displayContent()
      }, 2000)
    },
    displayContent: function () {
      this.result_ok = true
    }
  },  
};
</script>
<style lang="stylus" scoped>
.media-menu {
  min-width:260px;
  border-right:1px solid #eee; 
  min-height:calc(100vh - 50px - 64px);  
}
.media-detail {
  min-width:300px;
  border-left:1px solid #eee;  
}

.v-tabs__content {
  margin-top: 25px;
}

#nlu_content {
  margin-top: 50px;
}

input[name=nul_text] {
  padding-top: 20px;
}

</style>
