<template>
  <v-container class="pa-0 ma-0">
    <v-row>
      <v-col cols="3" align="center">
        <ItemContainer
          v-for="(item, i) in build.slice(0, 5)"
          :key="`item#${i}`"
          :item="item"
          :icon="item.default"
        />
      </v-col>
      <v-col cols="6">
        <CharacterView />
        <v-row style="margin-top: 86px">
          <v-col
            v-for="(item, d) in dofus.slice(0, 2)"
            :key="`item-${d}`"
            cols="6"
            align="center"
          >
            <ItemContainer
              :item="item"
            />
          </v-col>
        </v-row>
      </v-col>
      <v-col cols="3" align="center">
        <ItemContainer
          v-for="(item, j) in build.slice(5, 10)"
          :key="`item#${j}`"
          :item="item"
        />
      </v-col>
    </v-row>
    <v-row class="ml-3 mr-n9 mt-n6">
      <v-col
        v-for="(item, k) in dofus.slice(2, 6)"
        :key="`item#${k}`"
        cols="3"
      >
        <ItemContainer :item="item" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapState } from 'vuex'
import ItemContainer from '../Items/ItemContainer'
import CharacterView from './CharacterView'

export default {
  components: {
    CharacterView,
    ItemContainer
  },
  computed: mapState('build', ['build', 'dofus'])
}
</script>

<style>
.dofusSlots {
  margin-top: 64px;
}
</style>
