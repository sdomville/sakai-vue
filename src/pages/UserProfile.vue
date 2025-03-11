<template>
  <div class="content">
    <!-- <div class="container-fluid">
      <div class="row">
        <div class="col-md-8">
          <add-event>
          </add-event>
        </div>
        <div class="col-md-4">
          <user-card>
          </user-card>
        </div>
      </div>
    </div> -->
  </div>
</template>
<script>
  import AddEvent from './Events/AddEvent.vue'
  import UserCard from './Events/UserCard.vue'

  export default {
    components: {
      AddEvent,
      UserCard
    }
  }

</script>
<style>

</style>
