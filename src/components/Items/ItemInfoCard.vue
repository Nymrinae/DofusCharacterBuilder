<template>
  <v-container class="ma-0 pa-0">
    <v-row no-gutters class="align-center">
      <v-img
        :src="getLocalImageLink(item.imgUrl)"
        width="100px"
        height="100px"
        contain
      />
      <v-btn
        class="btnAdd"
        color="green"
        rounded
        small
        fab
        @click="buildItem"
      >
        <v-icon color="white"> mdi-plus </v-icon>
      </v-btn>
    </v-row>
    <v-row no-gutters>
      <v-card-text
        class="text-justify"
        v-text="getItemDescription(item.description)"
      />
    </v-row>
    <v-row no-gutters style="margin-left: 56px">
      <v-col
        v-for="(stat, s) in parseStat(item.statistics)"
        :key="`stat#${s}`"
        cols="6"
      >
        <v-row no-gutters align="center" justify="center">
          <v-col cols="2">
            <v-img
              :src="getLocalIconLink(stat.name)"
              width="33"
              height="33"
            />
          </v-col>
          <v-col cols="9">
            <v-subheader class="font-weight-black" v-text="printStat(stat)" />
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapActions } from 'vuex'
import {
  cleanItem,
  parseItem
} from '@/helpers/parser'

export default {
  props: {
    item: { type: Object, required: true }
  },
  data: () => ({
    currentItemURL: null,
    currentItemStats: null
  }),
  methods: {
    ...mapActions('build', ['setItem']),
    ...mapActions('stats', ['updateStatsFromItem']),
    buildItem() {
      const itemToSave = {
        name: this.item.name,
        type: this.item.type,
        icon: this.currentItemURL.split('/').slice(-1),
        stats: this.currentItemStats
      }

      this.setItem(itemToSave)

      const updateStatArray = []
      itemToSave.stats.map(e => updateStatArray.push({ [e.name]: e.min }))

      this.updateStatsFromItem([updateStatArray, 'add'])
    },
    getItemDescription: (desc) => {
      const parseDesc = desc.split('.')

      return parseDesc.length > 3
        ? parseDesc.slice(0, 2).join('.') + '...'
        : desc
    },
    getLocalIconLink: (statName) => {
      const elements = ['air', 'feu', 'eau', 'neutre', 'terre']
      const checkElem = elements.map(e => statName.includes(e)).indexOf(true)

      return require(`@@/assets/icons/${checkElem >= 0 ? elements[checkElem] : statName}.png`)
    },
    getLocalImageLink(url) {
      try {
        const itemURL = `https://github.com/Nymrinae/dofus-assets/blob/master/${url.split('/').slice(-1)[0]}?raw=true`
        this.currentItemURL = itemURL

        return itemURL
      } catch (e) {
        this.currentItemURL = '@@/assets/logo.png'
        return require('@@/assets/logo.png')
      }
    },
    parseStat(stat) {
      if (!stat) {
        this.currentItemStats = []
        return []
      }

      const parsedItem = parseItem(stat)
      const cleanedItem = cleanItem(parsedItem)

      this.currentItemStats = cleanedItem

      return cleanedItem
    },
    printStat: stat => `${stat.min}${stat.max ? `-${stat.max}` : ''}`
  }
}
</script>

<style>
.btnAdd {
  position: absolute;
  z-index: 2;
  margin-left: 190px;
  margin-top: -30px;
}
</style>
