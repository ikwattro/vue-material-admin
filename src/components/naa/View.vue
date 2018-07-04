<template>
<div id="recordsList">
  <v-container grid-list-xl fluid>
    <v-layout row wrap>
      <v-flex lg4>
        <v-widget title="Related Functions" class="mt-3">
            <div slot="widget-content">
              <v-list two-line subheader>
                <!-- <v-subheader inset>Entities</v-subheader> -->
                <v-list-tile avatar v-for="f in infos.functions" :key="f.value">
                  <v-list-tile-avatar>
                    <v-icon class="blue white--text">{{ f.icon }}</v-icon>
                  </v-list-tile-avatar>
                  <v-list-tile-content>
                    <v-list-tile-title>{{ f.value }}</v-list-tile-title>
                    <v-list-tile-sub-title>Via {{ f.byNorm }}</v-list-tile-sub-title>
                  </v-list-tile-content>
                </v-list-tile>
              </v-list>
            </div>
          </v-widget>
      </v-flex>
      <v-flex lg4>
        <v-widget title="Related Directory Entities" class="mt-3">
            <div slot="widget-content">
              <v-list two-line subheader>
                <v-subheader inset>Via Persons</v-subheader>
                <v-list-tile avatar v-for="ph in infos.directories.viaPersons" :key="ph.value">
                  <v-list-tile-avatar>
                    <v-icon class="blue white--text">{{ ph.icon }}</v-icon>
                  </v-list-tile-avatar>
                  <v-list-tile-content>
                    <v-list-tile-title>{{ ph.value }}</v-list-tile-title>
                    <v-list-tile-sub-title>Via {{ ph.byNorm }}</v-list-tile-sub-title>
                  </v-list-tile-content>
                </v-list-tile>
                <v-subheader inset>Via Organizations</v-subheader>
                <v-list-tile avatar v-for="oh in infos.directories.viaOrgs" :key="oh.value">
                  <v-list-tile-avatar>
                    <v-icon class="blue white--text">{{ oh.icon }}</v-icon>
                  </v-list-tile-avatar>
                  <v-list-tile-content>
                    <v-list-tile-title>{{ oh.value }}</v-list-tile-title>
                    <v-list-tile-sub-title>Via {{ oh.byNorm }}</v-list-tile-sub-title>
                  </v-list-tile-content>
                </v-list-tile>
              </v-list>
            </div>
          </v-widget>
      </v-flex>
      <v-flex lg4>
        <v-widget title="Related Media" class="mt-3">
            <div slot="widget-content">
              <v-list two-line subheader>
                <v-subheader inset>Hansard</v-subheader>
                <v-list-tile avatar v-for="h in infos.media.hansard" :key="h.value">
                  <v-list-tile-avatar>
                    <v-icon class="blue white--text">{{ h.icon }}</v-icon>
                  </v-list-tile-avatar>
                  <v-list-tile-content>
                    <v-list-tile-title>{{ h.value }}</v-list-tile-title>
                    <v-list-tile-sub-title>Via {{ h.byNorm }}</v-list-tile-sub-title>
                  </v-list-tile-content>
                </v-list-tile>
              </v-list>
            </div>
          </v-widget>
      </v-flex>
    </v-layout>
    <v-layout row wrap>
      <v-flex lg6 xs12>
      <v-widget title="Entities" class="mt-3">
            <div slot="widget-content">
              <v-list two-line subheader>
                <!-- <v-subheader inset>Entities</v-subheader> -->
                <v-list-tile avatar v-for="item in infos.entities" :key="item.value" @click="handleClick(item)">
                  <v-list-tile-avatar>
                    <v-icon class="blue white--text">{{ item.icon }}</v-icon>
                  </v-list-tile-avatar>
                  <v-list-tile-content>
                    <v-list-tile-title>{{ item.value }}</v-list-tile-title>
                  </v-list-tile-content>
                </v-list-tile>
              </v-list>
            </div>
          </v-widget>
    </v-flex>
    <v-flex lg6 xs12>
      <v-widget title="Concepts" class="mt-3">
            <div slot="widget-content">
              <v-list two-line subheader>
                <!-- <v-subheader inset>Entities</v-subheader> -->
                <v-list-tile avatar v-for="concept in infos.concepts" :key="concept.value" @click="handleClick(concept)">
                  <v-list-tile-avatar>
                    <v-icon class="orange white--text">{{ concept.icon }}</v-icon>
                  </v-list-tile-avatar>
                  <v-list-tile-content>
                    <v-list-tile-title>{{ concept.value }}</v-list-tile-title>
                  </v-list-tile-content>
                </v-list-tile>
              </v-list>
            </div>
          </v-widget>
    </v-flex>
    </v-layout>
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
    infos: {},
    iconsMap: {
      'PERSON': 'person_pin',
      'ORGANIZATION': 'business',
      'LOCATION': 'location_on'
    }
  }),

  mounted() {
    const docId = this.$route.params.id
    const URL = process.env.API_URL + '/api/naa/record/' + docId
    const vm = this
    vm.infos['directories'] = []
    this.$http.get(URL)
      .then(function (response) {
        console.log(response)
        vm.infos.entities = response.data.entities
        vm.infos.entities.forEach(i => {
          i.icon = vm.iconsMap[i.type]
        })
        vm.infos.concepts = response.data.concepts
        vm.infos.concepts.forEach(i => {
          i.icon = 'blur_on'
        })
        vm.infos.functions = response.data.functions
        vm.infos.functions.forEach(i => {
          i.icon = 'blur_on'
          i.byNorm = i.by.join(', ')
        })
        vm.infos.media = response.data.media
        vm.infos.media.hansard.forEach(i => {
          i.icon = 'blur_on'
          i.byNorm = i.by.join(', ')
        })

        vm.infos.directories = response.data.directories
        console.log(vm.infos)
        vm.infos.directories.viaOrgs.forEach(i => {
          i.icon = 'blur_on'
          i.byNorm = i.by.join(', ')
        })
        vm.infos.directories.viaPersons.forEach(i => {
          i.icon = 'blur_on'
          i.byNorm = i.by.join(', ')
        })
        vm.$forceUpdate()
      })
  },

  computed: {
    
  },

  methods: {
    handleClick: function(e) {
      console.log(e)
    },
    getIcon (t) {
      console.log(t)
      return this.iconsMap[t]
    }
  },  
};
</script>
<style lang="stylus" scoped>

</style>
