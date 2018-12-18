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
        <v-list two-line>
          <draggable
            v-model="columns"
            :options="{draggable:'.item'}"
            @start="drag = true"
            @end="drag = false"
            @change="updateColumn"
          >
            <template v-for="column in columns">
              <v-list-tile
                class="item"
                :key="column.name"
                @click=""
              >
                <v-list-tile-content>
                  <v-text-field
                    v-if="column.show"
                    :label="column.name"
                    v-model="column.displayName"
                  >
                  </v-text-field>
                  <span v-else>{{ column.displayName }}</span>
                </v-list-tile-content>

                <v-list-tile-action>
                  <template v-if="column.show">
                    <v-btn
                      icon
                      @click="resetColumn(column)">
                      <v-icon>close</v-icon>
                    </v-btn>
                    <v-btn
                      icon
                      @click="toggleRow(column)">
                      <v-icon>check</v-icon>
                    </v-btn>
                  </template>
                  <v-btn
                    icon
                    @click="toggleRow(column)
"
                    v-else
                  >
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
    columns: [],
    origColumns: []
  }),
  methods: {
    fileChanged (file) {
      const reader = new FileReader()
      reader.onload = () => {
        const info = parse(reader.result, {
          columns: true,
          skip_empty_lines: true
        })
        Object.keys(info[0]).forEach((c, idx) => {
          this.columns.push({ name: c, displayName: c, show: false, initIndex: idx, index: idx })
          this.origColumns.push({ name: c, index: idx })
        })
      }
      reader.readAsBinaryString(file)
    },
    resetColumn (column) {
      column.displayName = column.name
      column.show = !column.show
    },
    toggleRow (column) {
      column.show = !column.show
    },
    updateColumn (e) {
      const index = this.columns.findIndex(c => c.name === e.element.name)
      this.columns[index].index = e.newIndex
      console.log(e)
    }
  }
}
</script>

<style>

</style>
