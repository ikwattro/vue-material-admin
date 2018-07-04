<template>
<div id="recordsList">
  <v-container grid-list-xl fluid>
    <v-widget title="Record Authority documents" class="mt-3">
            <div slot="widget-content">
              <v-btn color="red" dark fab small @click="redirect">
                  <v-icon dark>add</v-icon>
                </v-btn>
              <v-list two-line subheader>
                <v-subheader inset>Files</v-subheader>
                <v-list-tile avatar v-for="item in records" :key="item.id" @click="handleClick(item)">
                  <v-list-tile-avatar>
                    <v-icon :class="[item.iconClass]">{{ item.icon }}</v-icon>
                  </v-list-tile-avatar>
                  <v-list-tile-content>
                    <v-list-tile-title>{{ item.props.title }}</v-list-tile-title>
                    <v-list-tile-sub-title>Uploaded {{ item.props.publishedDate }}</v-list-tile-sub-title>
                  </v-list-tile-content>
                  <v-list-tile-action>
                    <v-btn icon ripple>
                      <v-icon color="grey lighten-1">info</v-icon>
                    </v-btn>
                  </v-list-tile-action>
                </v-list-tile>
              </v-list>
            </div>
          </v-widget>
  </v-container>
</div>

</template>

<script>
import axios from 'axios';
import VWidget from '@/components/VWidget';
export default {
  components: {
    VWidget
  },
  data: () => ({
    nlu_text: '',
    records: [],
    loading: false,
    result_ok: false,
    folders: [
        { icon: 'assignment', iconClass: 'blue white--text', title: 'Photos', subtitle: 'Jan 9, 2014' },
        { icon: 'folder', iconClass: 'grey lighten-1 white--text', title: 'Recipes', subtitle: 'Jan 17, 2014' },
        { icon: 'folder', iconClass: 'grey lighten-1 white--text', title: 'Work', subtitle: 'Jan 28, 2014' }
      ],
      files: [
        { icon: 'assignment', iconClass: 'blue white--text', title: 'Vacation itinerary', subtitle: 'Jan 20, 2014' },
        { icon: 'call_to_action', iconClass: 'amber white--text', title: 'Kitchen remodel', subtitle: 'Jan 10, 2014' }
      ],
    keywords: [{value: 'Officer Dana Deasy', pct: 0.81}, {value: 'Secretary Patrick Shanahan', pct: 0.73}, {value: 'Joint Artificial Intelligence Center', pct: 0.72}],
    tab: null
  }),

  mounted() {
    const URL = process.env.API_URL + '/api/naa'
    const vm = this
    this.$http.get(URL)
      .then(function (response) {
        response.data.forEach(elt => {
          console.log(elt)
          vm.records.push({icon: 'assignment', props: elt.props, id: elt.id, iconClass: 'blue lighten-1 white--text'})
        })
      })
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
    },
    handleClick: function (e) {
      console.log(e)
      this.$router.push({name: 'recordsView', params: {id: e.id}})
    },
    redirect: function () {
      this.$router.push({name: 'NLU'})
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
