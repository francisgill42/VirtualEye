<template>
<div>
    <v-row v-if="isSuperAdmin">

      {{cards}}

          <v-col cols="12" sm="8" md="6" class="" v-for="(card,i) in cards" :key="i">
            <v-card :to="card.link" :class="card.color" class="pa-2">
            <v-list-item dark>
            <v-list-item-content>
            <div class="text-center">{{added_zero(card.count)}}</div>
            <div class="text-center" v-text="card.text"></div>
            </v-list-item-content>
            </v-list-item>
            </v-card>
          </v-col>

         
           
  </v-row>

  <v-row v-if="isProjectAdmin || isManager">
    <v-col md="12"><Project /></v-col>
  </v-row>

</div>
</template>

<script>
import Project from '@/components/Project';

export default {

  components : { Project },

  methods : {
    added_zero(v) { return v < 10 ? '0' + v : v },
  },
 data : () => ({
    loaded : false,
    arr : [],
    cards : [],
    value: [],
    data: {
        labels: [],
          
        datasets: [
          {
            backgroundColor: [],
            data: [],
          },          
        ],
      },
      options : {
            showLines: false,
          }
 }),
   async mounted () {

    this.loaded = false
    try {

      await this.$axios.get('all')
      .then(res => {
        
        this.cards = [
                    {link : '/project',text:'Total Projects',count:res.data.ProjectCount,color:'primary',chartColor : 'rgb(45 87 163)'},
                    {link : '/client',text:'Total Clients',count:res.data.isProjectAdmin = 5000,color:'green',chartColor : 'rgb(45 87 163)'},
                    {link : '/user',text:'Total Users',count:res.data.UserCount,color:'secondary lighten-1',chartColor : 'rgb(244 67 54)'},
                    {link : '/manager',text:'Total Managers',count:res.data.UserCount,color:'orange darken-4', chartColor : 'rgb(230 81 0)'},
                    // {link : '/covid',text:'Daily Man Hours',count:res.data.GetDailyManHoursCount,color:'orange lighten-1',chartColor : 'rgb(255 167 38)'},
                    // {link : '/covid',text:'Lost Work Hours',count:res.data.GetLostWorkHoursCount,color:'brown lighten-1',chartColor : 'rgb(141 110 99)'},

        ];

      });

      this.loaded = true
    } catch (e) {
      console.error(e)
    }
  },
  computed : {
    isSuperAdmin () {
          return this.$auth.user && this.$auth.user.user_type == 'Super Admin'
    },

    isProjectAdmin () {
          
    },
    isManager () {
      return this.$auth.user && this.$auth.user.user_type == 'Wewatch Manager'
    }

  }
}
</script>
