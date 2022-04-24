<template>
  <div class="bg">

    <dashboard-navbar/>

 
<jumbo-tron/>



    <div class="container-fluid">
      <div class="row">
        
        <div class="col-12">
          <!--<router-view/> change to name of component-->
          <taskListOverview />


        </div>
      </div>
    </div>
  
<footer-hf />
</div>

</template>

<script>
  import DashboardNavbar from '@/components/DashboardNavbar'
  import JumboTron from '@/components/JumboTron'
  import FooterHf from '@/components/FooterHf'
  import TaskListOverview from '@/views/TasklistOverview'
  

  export default {
    components: {
      
      DashboardNavbar,
      JumboTron,
      FooterHf,
      TaskListOverview
        
    },
    mounted () {
      this.$store.dispatch('loadUser');
    }
  }
</script>

<style scoped>
  .bg { 
    background-color: rgb(202, 184, 184);
  }
</style>
