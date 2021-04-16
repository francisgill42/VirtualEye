<template>
  <v-app dark>
    <v-navigation-drawer
    class="primary"
    dark
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"

          router
          exact
        >
          <v-list-item-action >
            <v-icon >{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.text" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      class="white"
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
     
    
        <!-- <img src="/logo.png" alt="login" style="width:75px" height="50px"> -->
         
         
       <span class="primary--text">  Virtual Eye  <v-icon color="primary">mdi-eye</v-icon> 
         </span>
         
      <v-spacer />
       <span class="secondary--text">{{Greetings}}</span> 
      <v-btn @click="logout" icon color="secondary">
        <v-icon>mdi-logout</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
      <v-breadcrumbs :items="items">
      <template v-slot:divider>
      <v-icon>mdi-chevron-right</v-icon>
      </template>
      </v-breadcrumbs>
      <nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer
      v-model="rightDrawer"
      :right="right"
      temporary
      fixed
    >
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light>
              mdi-repeat
            </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer 
      dark
      class="grey"
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      links : [],
      hide : false,
      clipped: true,
      drawer: false,
      fixed: false,
      items: [],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      text:'Varda Solutions'
    }
  },
  created () {



    var AllMenu = [
      {
          icon: 'mdi-apps',
          text:'Dashboard',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          text:'Client',
          to: '/client',
         },
         
        {
          icon: 'mdi-chart-bubble',
          text:'Project',
          to: '/project'
        },
      
         {
          icon: 'mdi-chart-bubble',
          text:'User',
          to: '/user'
        },
        

       
        {
          icon: 'mdi-chart-bubble',
          text:'Allocation',
          to: '/allocations'
        },
        
    ];
   

    if(this.isSecurityGuard){
    this.items = [
        {
          icon: 'mdi-apps',
          text:'Dashboard',
          to: '/'
        },
         {
          icon: 'mdi-apps',
          text:'Inspire',
          to: '/inspire'
        },
    ]    
    }
    else if(this.isUser){
      this.items = [
        {
          icon: 'mdi-apps',
          text:'Dashboard',
          to: '/'
        },
       
        {
          icon: 'mdi-apps',
          text:'Inspire',
          to: '/inspire'
        },
    ]
    }
    else if(this.isProjectAdmin){
      this.items = [
        {
          icon: 'mdi-apps',
          text:'Dashboard',
          to: '/'
        }
    ]
    }
    else if(this.isManager){
      this.items = [
        {
          icon: 'mdi-apps',
          text:'Dashboard',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          text:'Project',
          to: '/project'
        },
      
    ]
    }

    else{
      this.items = AllMenu;
    }

  },
  computed : {
  
    Greetings () {
      return 'Welcome, ' + this.$auth.user.email;
    },


    isSecurityGuard () {
      return this.$auth.user && this.$auth.user.user_type == "Security Guard";
    },
    
    isProjectAdmin () {
      return this.$auth.user && this.$auth.user.user_type == "project Admin";
    },

    isUser () {
      return this.$auth.user && this.$auth.user.user_type == "User";
    },
    isManager () {
      return this.$auth.user && this.$auth.user.user_type == 'Wewatch Manager'
    }
  },
  methods:{
     logout() {
       this.$auth.logout();
},
  }
}
</script>
