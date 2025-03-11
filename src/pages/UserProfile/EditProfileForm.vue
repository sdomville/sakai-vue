<template>
  <card>
    <h4 slot="header" class="card-title">Add Event</h4>
    <form>
      <div class="row">
        <div class="col-md-8">
          <base-input type="text"
                    label="Name"
                    placeholder=""
                    v-model="event.lastName">
          </base-input>
        </div>
       
        <div class="col-md-3">
          <base-input type="text"
                    label="Alert Type"
                    placeholder=""
                    v-model="event.name">
          </base-input>
        </div>
      </div>

      <div class="row">
        <div class="col-md-8">
          <base-input type="text"
                    label="Area of Concern"
                    placeholder=""
                    v-model="event.alertType">
          </base-input>
        </div>
        <div class="col-md-3">
          <base-input type="text"
                    label="AC ID"
                    placeholder=""
                    v-model="event.acid">
          </base-input>
        </div>

      </div>

      <div class="row">
        <div class="col-md-12">
          <div class="form-group">
            <label>Event JSON</label>
            <textarea rows="10" class="form-control border-input"
                    placeholder=""
                    v-model="event.eventJson">
            </textarea>
          </div>
        </div>
      </div>
      <div class="text-center">
        <button type="submit" class="btn btn-info btn-fill float-right" @click.prevent="saveEvent">
          Save Event
        </button>
      </div>
      <div class="clearfix"></div>
    </form>
  </card>
</template>
<script>
  import Card from 'src/components/Cards/Card.vue'

  export default {
    components: {
      Card
    },
    data () {
      return {
        event: {
          name: '',
          alertType: '',
          acid: '',
          eventJson: '',
        }
      }
    },
    methods: {
      saveEvent () {
        alert('Your event: ' + JSON.stringify(this.user))
      }
    }
  }

</script>
<style>

</style>
