<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-app-bar-nav-icon @click="drawer = !drawer" />
      <site-title :title="site.title"></site-title>
      <v-spacer />
    </v-app-bar>
    <v-navigation-drawer app v-model="drawer">
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="title"> Application </v-list-item-title>
          <v-list-item-subtitle> subtext </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
      <v-divider></v-divider>
      <site-menu :items="site.menu"></site-menu>
    </v-navigation-drawer>
    <v-content>
      <router-view />
    </v-content>
    <site-footer :footer="site.footer"></site-footer>
  </v-app>
</template>

<script>
import SiteTitle from "@/views/site/title";
import SiteFooter from "@/views/site/footer";
import SiteMenu from "@/views/site/menu";

export default {
  components: { SiteTitle, SiteFooter, SiteMenu },
  name: "App",
  data() {
    return {
      drawer: false,
      site: {
        menu: [
          {
            title: "home",
            icon: "mdi-home",
            subItems: [
              {
                title: "Dashboard",
                to: "/",
              },
              {
                title: "About",
                to: "/about",
              },
            ],
          },
          {
            title: "about",
            active: true,
            icon: "mdi-account",
            subItems: [
              {
                title: "xxx",
                to: "/xxx",
              },
            ],
          },
        ],
        title: "나의 타이틀입니다",
        footer: "푸터입니다",
      },
    };
  },
  created() {
    this.subscribe();
  },
  methods: {
    subscribe() {
      this.$firebase
        .database()
        .ref()
        .child("site")
        .on(
          "value",
          (sn) => {
            const v = sn.val();
            if (!v) {
              // 값이 없을떄 -> 그냥 현재 값 저장
              this.$firebase.database().ref().child("site").set(this.site);
              return;
            }
            this.site = v;
          },
          (e) => {
            console.log(e.message);
          }
        );
    },
  },
};
</script>
