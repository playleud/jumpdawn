<template>
  <v-card class="mx-auto" id="create">
    <v-toolbar color="purple" dark>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>JumpDawn</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
    </v-toolbar>

    <v-carousel cycle hide-delimiters height="10rem">
      <v-carousel-item
        v-for="(item, i) in banners"
        :key="i"
        :src="item.src"
        reverse-transition="fade-transition"
        transition="fade-transition"
      ></v-carousel-item>
    </v-carousel>

    <v-list subheader three-line>
      <v-subheader>User Controls</v-subheader>

      <v-list-item>
        <v-list-item-content>
          <v-list-item-title>Content filtering</v-list-item-title>
          <v-list-item-subtitle
            >Set the content filtering level to restrict appts that can be
            downloaded</v-list-item-subtitle
          >
        </v-list-item-content>
      </v-list-item>

      <v-list-item>
        <v-list-item-content>
          <v-list-item-title>Password</v-list-item-title>
          <v-list-item-subtitle
            >Require password for purchase or use password to restrict
            purchase</v-list-item-subtitle
          >
        </v-list-item-content>
      </v-list-item>
    </v-list>

    <v-navigation-drawer
      class="deep-purple accent-4"
      absolute
      temporary
      v-model="drawer"
    >
      <v-list>
        <v-list-item v-for="item in items" :key="item.title" link>
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <template v-slot:append>
        <div class="pa-2">
          <v-btn block> Logout </v-btn>
        </div>
      </template>
    </v-navigation-drawer>

    <v-container>
      <v-row>
        <v-col v-for="n in 12" :key="n" class="d-flex child-flex" cols="4">
          <v-img
            :src="`//bookcover.yuewen.com/qdbimg/349573/1003354631/150`"
            :lazy-src="`https://picsum.photos/10/6?image=${n * 5 + 10}`"
            aspect-ratio="1"
            class="grey lighten-2"
          >
            <template v-slot:placeholder>
              <v-row class="fill-height ma-0" align="center" justify="center">
                <v-progress-circular
                  indeterminate
                  color="grey lighten-5"
                ></v-progress-circular>
              </v-row>
            </template>
          </v-img>
        </v-col>
      </v-row>

      <v-row dense>
        <v-col cols="12">
          <v-card color="#385F73" dark>
            <v-card-title class="text-h5"> Unlimited music now </v-card-title>

            <v-card-subtitle
              >Listen to your favorite artists and albums whenever and wherever,
              online and offline.</v-card-subtitle
            >

            <v-card-actions>
              <v-btn text> Listen Now </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>

        <v-col v-for="(item, i) in images" :key="i" cols="12">
          <v-card :color="item.color" dark>
            <div class="d-flex flex-no-wrap justify-space-between">
              <div>
                <v-card-title
                  class="text-h5"
                  v-text="item.title"
                ></v-card-title>

                <v-card-subtitle v-text="item.artist"></v-card-subtitle>

                <v-card-actions>
                  <v-btn
                    v-if="item.artist === 'Ellie Goulding'"
                    class="ml-2 mt-3"
                    fab
                    icon
                    height="40px"
                    right
                    width="40px"
                  >
                    <v-icon>mdi-play</v-icon>
                  </v-btn>

                  <v-btn v-else class="ml-2 mt-5" outlined rounded small>
                    START RADIO
                  </v-btn>
                </v-card-actions>
              </div>

              <v-avatar class="ma-3" size="125" tile>
                <v-img :src="item.src"></v-img>
              </v-avatar>
            </div>
          </v-card>
        </v-col>
      </v-row>
    </v-container>

    <v-speed-dial
      v-model="fab"
      :top="top"
      :bottom="bottom"
      :right="right"
      :left="left"
      :direction="direction"
      :open-on-hover="hover"
      :transition="transition"
    >
      <template v-slot:activator>
        <v-btn v-model="fab" color="blue darken-2" dark fab>
          <v-icon v-if="fab"> mdi-close </v-icon>
          <v-icon v-else> mdi-account-circle </v-icon>
        </v-btn>
      </template>
      <v-btn fab dark small color="green">
        <v-icon>mdi-pencil</v-icon>
      </v-btn>
      <v-btn fab dark small color="indigo">
        <v-icon>mdi-plus</v-icon>
      </v-btn>
      <v-btn fab dark small color="red">
        <v-icon>mdi-delete</v-icon>
      </v-btn>
    </v-speed-dial>

    <v-list two-line>
      <v-list-item-group v-model="selected" active-class="pink--text" multiple>
        <template v-for="(item, index) in chapters">
          <v-list-item :key="item.title">
            <template>
              <v-list-item-content>
                <v-list-item-title v-text="item.title"></v-list-item-title>
              </v-list-item-content>
            </template>
          </v-list-item>
          <v-divider v-if="index < items.length - 1" :key="index"></v-divider>
        </template>
      </v-list-item-group>
    </v-list>
  </v-card>
</template>


<style>
#create .v-speed-dial {
  position: absolute;
}

.v-sheet{
  border-radius: 0
}

.v-card {
  border-radius: 0
}

#create .v-btn--floating {
  position: relative;
}
</style>

<script>
export default {
  data: () => ({
    settings: [],
    drawer: false,
    group: null,

    selected: [2],
    chapters: [
      {
        title: "Ali 施蒂利克教科书里绝对是基督教是劳动节",
      },
      {
        title: "Ali 施蒂利克教科书里绝对是基督教是劳动节",
      },
      {
        title: "Ali 施蒂利克教科书里绝对是基督教是劳动节",
      },
      {
        title: "Ali 施蒂利克教科书里绝对是基督教是劳动节li 施蒂利克教科书里绝对是基督教是劳动节",
      },
      {
        title: "Ali 施蒂利克教科书里绝对是基督教是劳动节",
      },
    ],

    direction: "top",
    fab: false,
    fling: false,
    hover: false,
    tabs: null,
    top: false,
    right: true,
    bottom: true,
    left: false,
    transition: "slide-y-reverse-transition",
    banners: [
      {
        src: "//bossaudioandcomic-1252317822.image.myqcloud.com/activity/document/ed3de3460a28936b25bae129499be61e.jpg",
      },
      {
        src: "//bossaudioandcomic-1252317822.image.myqcloud.com/activity/document/8e990b0e48ca61a8c56137832d16b24a.jpg",
      },
      {
        src: "//bossaudioandcomic-1252317822.image.myqcloud.com/activity/document/3543fb75ad5e815ac644b5268cdcf254.jpg",
      },
    ],
    // items: [
    //   ["mdi-email", "Inbox"],
    //   ["mdi-account-supervisor-circle", "Supervisors"],
    //   ["mdi-clock-start", "Clock-in"],

    // ],
    items: [
      { title: "书架", icon: "mdi-view-dashboard" },
      { title: "Account", icon: "mdi-account-box" },
      { title: "Admin", icon: "mdi-gavel" },
    ],
    images: [
      {
        color: "#1F7087",
        src: "https://cdn.vuetifyjs.com/images/cards/foster.jpg",
        title: "Supermodel",
        artist: "Foster the People",
      },
      {
        color: "#952175",
        src: "https://cdn.vuetifyjs.com/images/cards/halcyon.png",
        title: "Halcyon Days",
        artist: "Ellie Goulding",
      },
    ],
  }),
  computed: {
    activeFab() {
      switch (this.tabs) {
        case "one":
          return { class: "purple", icon: "account_circle" };
        case "two":
          return { class: "red", icon: "edit" };
        case "three":
          return { class: "green", icon: "keyboard_arrow_up" };
        default:
          return {};
      }
    },
  },
  watch: {
    group() {
      this.drawer = false;
    },
  },
};
</script>