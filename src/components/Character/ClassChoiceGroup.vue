<template>
  <v-container class="mt-10">
    <v-slide-group
      center-active
      show-arrows
    >
      <v-slide-item
        v-for="(c, i) in classes"
        :key="`class#${i}`"
        v-slot:default="{ active, toggle }"
      >
        <v-card
          class="mx-2"
          style="border-radius: 50%; border: 1px solid black"
          height="100"
          width="100"
          outlined
          @click="toggle"
        >
          <v-img
            :src="require(`@@/assets/classes/avatars/${c.toLowerCase()}.png`)"
            @click="UPDATE_ACTIVE_CLASS(c)"
          />
        </v-card>
      </v-slide-item>
    </v-slide-group>
  </v-container>
</template>

<script>
import { mapMutations, mapState } from 'vuex'

export default {
  computed: mapState('character', ['classes']),
  methods: mapMutations('character', ['UPDATE_ACTIVE_CLASS'])
}
</script>

<style>
</style>
