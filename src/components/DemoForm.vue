<template>
  <v-container grid-list-md>
    <v-layout
      text-xs-center
      wrap
      row
    >
      <v-flex xs12>
        <h1 class="headline">Upload Your File</h1>
        <upload-button
           accept=".csv,.tsv"
           :fileChangedCallback="fileChanged"
           icon
           dark
         >
          <template slot="icon">
            <v-icon dark>add</v-icon>
          </template>
        </upload-button>
      </v-flex>

      <v-flex xs12 sm6 offset-sm3>
        <v-list>
          <draggable
             v-model="origColumns"
             :options="{draggable:'.item'}"
             @start="drag = true"
             @end="drag = false"
          >
            <template v-for="(value, key, index) in columns">
              <v-list-tile
                 class="item"
                 :key="key"
                 @click=""
              >
                <v-list-tile-content>
                  <v-text-field
                     v-if="show[key]"
                  >
                  </v-text-field>
                  <span v-else>{{ value }}</span>
                </v-list-tile-content>

                <v-list-tile-action>
                  <v-btn
                     icon
                     @click="toggleRow(key)">
                    <v-icon>edit</v-icon>
                  </v-btn>
                </v-list-tile-action>
              </v-list-tile>
            </template>
          </draggable>
        </v-list>
      </v-flex>

      <v-flex xs12 sm6>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import Draggable from 'vuedraggable'
import UploadButton from 'vuetify-upload-button'
import parse from 'csv-parse/lib/sync'

export default {
  name: 'DemoForm',
  components: {
    Draggable,
    UploadButton
  },
  data: () => ({
    columns: {},
    origColumns: [],
    show: {}
  }),
  methods: {
    fileChanged (file) {
      const reader = new FileReader()
      reader.onload = () => {
        const info = parse(reader.result, {
          columns: true,
          skip_empty_lines: true
        })
        Object.keys(info[0]).forEach(c => {
          this.columns[c] = c
          this.show[c] = false
        })
        this.origColumns = Object.keys(info[0])
      }
      reader.readAsBinaryString(file)
    },
    toggleRow (key) {
      this.show[key] = !this.show[key]
    }
  }
}
</script>

<style>

</style>
