<template>
  <v-app id="app">
    <v-navigation-drawer
      fixed
      :clipped="$vuetify.breakpoint.lgAndUp"
      app
      v-model="drawer"
    >
      <v-list dense>
        <template v-for="(item, ndx) in menuItems">
          <v-row
            v-if="item.heading"
            align="center"
            :key="ndx"
          >
            <v-col cols="6">
              <v-subheader v-if="item.heading">
                {{ item.heading }}
              </v-subheader>
            </v-col>
            <v-col 
              cols="6"
              class="text-center"
            >
              <a href="#!" class="body-2 black--text">EDIT</a>
            </v-col>
          </v-row>
          <v-list-group 
            v-else-if="item.children"
            :key="ndx"
            v-model="item.model"
            :prepend-icon="item.model ? item.icon : item['icon-alt']"
            append-icon=""
          >
          <!--  <v-list-tile slot="item" @click="menuAction">
              <v-list-tile-action>
                <v-icon>{{ item.model ? item.icon : item['icon-alt'] }}</v-icon>
              </v-list-tile-action> -->
            <template v-slot:activator>
              <v-list-item-content>
                <v-list-item-title>
                   {{ item.text }}
                </v-list-item-title>
              </v-list-item-content>
            </template>
            </v-list-item>
            <v-list-item
              v-for="(child, i) in item.children"
              :key="i"
              @click="menuAction"
            >
              <v-list-item-action v-if="child.icon">
                <v-icon>{{ child.icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title>
                  {{ child.text }}
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-group>
          <v-list-item v-else @click="menuAction" v-bind:key="ndx">
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>
                {{ item.text }}
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      color="blue darken-3"
      dark
      app
      clipped-left
      fixed
    >
    <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title :style="$vuetify.breakpoint.smAndUp ? '' : 'min-width: 72px'" class="ml-0 pl-3">
        <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
        <span class="hidden-xs-only">Globomantics Rewards</span>
      </v-toolbar-title>
      <!-- <v-text-field
        light
        solo
        prepend-icon="search"
        placeholder="Search"
        style="max-width: 500px; min-width: 128px"
      ></v-text-field> -->
      <!-- <header-actions></header-actions> -->
    </v-app-bar>
    <v-content>
      <v-container fluid>
        <v-row
          align="center"
          justify="center"
        >
            <transactions></transactions>
        </v-row>
      </v-container>
    </v-content>
    <edit-transaction></edit-transaction>
  </v-app>
</template>

<script>
import Transactions from './Transactions.vue'
import EditTransaction from './EditTransactions.vue'

export default {
  name: 'Home',
  components: {
    Transactions,
    EditTransaction
  },
  computed: {
    isLoggedIn () {
      return this.$store.getters.isLoggedIn
    }
  },
  data: () => ({
    dialog: false,
    drawer: null,
    menuItems: [
      { icon: 'mdi-contacts', text: 'Add Transaction' },
      { icon: 'mdi-history', text: 'Current Month' },
      { icon: 'mdi-content-copy', text: 'Notes' },
      { icon: 'mdi-settings', text: 'Settings' },
      { icon: 'mdi-message', text: 'Send feedback' },
      { icon: 'mdi-help-circle', text: 'Help' }
    ]
  }),
  methods: {
    menuAction: function () {
      // TODO
    },
    showProfile: function () {
      console.log('show profile clicked!')
    }
  },
  mounted: function () {
    console.log('Is user logged in? ', this.isLoggedIn)
    if (!this.isLoggedIn) {
      this.$router.push({ path: '/login' })
    }
  }
}
</script>
