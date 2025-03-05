<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-xl-3 col-md-6 center">
          <stats-card>
            <div slot="header" class="icon-dnager">
              <i class="nc-icon nc-alien-33 text-danger"></i>
            </div>
            <div slot="content">
              <p class="card-category">Alerts</p>
              <h4 class="card-title">105</h4>
            </div>
            <div slot="footer">
              <i class="fa fa-refresh"></i>Updated now
            </div>
          </stats-card>
        </div>

        <div class="col-xl-3 col-md-6 center">
          <stats-card>
            <div slot="header" class="icon-success">
              <i class="nc-icon nc-notes text-success"></i>
            </div>
            <div slot="content">
              <p class="card-category">Events</p>
              <h4 class="card-title">1345</h4>
            </div>
            <div slot="footer">
              <i class="fa fa-calendar-o"></i>Last day
            </div>
          </stats-card>
        </div>

        <div class="col-xl-3 col-md-6 center">
          <stats-card>
            <div slot="header" class="icon-info">
              <i class="nc-icon nc-favourite-28 text-primary"></i>
            </div>
            <div slot="content">
              <p class="card-category">Avg ETA (ms)</p>
              <h4 class="card-title">23</h4>
            </div>
            <div slot="footer">
              <i class="fa fa-clock-o"></i>Last day
            </div>
          </stats-card>
        </div>
      </div>


      <!-- second row of cards -->
      <div class="row">
        <div class="col-xl-3 col-md-6 center">
          <stats-card>
            <div slot="header" class="icon-warning">
              <i class="nc-icon nc-atom text-warning"></i>
            </div>
            <div slot="content">
              <p class="card-category">Test Cases</p>
              <h4 class="card-title">363</h4>
            </div>
            <div slot="footer">
              <i class="fa fa-refresh"></i>Last day
            </div>
          </stats-card>
        </div>

        <div class="col-xl-3 col-md-6 center">
          <stats-card>
            <div slot="header" class="icon-success">
              <i class="nc-icon nc-light-3 text-success"></i>
            </div>
            <div slot="content">
              <p class="card-category">Epic AC Coverage</p>
              <h4 class="card-title">45%</h4>
            </div>
            <div slot="footer">
              <i class="fa fa-calendar-o"></i>Last day
            </div>
          </stats-card>
        </div>

        <div class="col-xl-3 col-md-6 center">
          <stats-card>
            <div slot="header" class="icon-danger">
              <i class="nc-icon nc-vector text-danger"></i>
            </div>
            <div slot="content">
              <p class="card-category">Issues</p>
              <h4 class="card-title">+45</h4>
            </div>
            <div slot="footer">
              <i class="fa fa-refresh"></i>Updated now
            </div>
          </stats-card>
        </div>
      </div>

      <card>
        <h4 slot="header" class="card-title">Send Event</h4>
        <form>
          <div class="row">
            <div class="col-md-6">
              <div class="form-group">
                <label for="sel1">Select AC(s):</label>
                <select multiple="true" class="form-control" v-model="selectedACs" @change="selectACs($event)" >
                  <option v-for="ac in acList" :key="ac.id" :value="ac.id">{{ac.name}}</option>
                </select>
              </div>            
            </div>
            <div class="col-md-6">
              <div class="form-group">
                <label for="sel1">Select Event(s):</label>
                <select multiple="true" class="form-control" v-model="selectedEvents" @change="selectEvents($event)" >
                  <option v-for="event in eventsList" :key="event.id" :value="event.id">{{event.name}}</option>
                </select>
              </div>
            </div>
          </div>

          <div class="row">
            <div class="col-md-12">
              <div class="form-group">
                <label>Event JSON</label>
                <textarea rows="10" class="form-control border-input"
                          placeholder=""
                          v-model="formattedJson"
                          >
                  </textarea>
              </div>
            </div>
          </div>
          <div class="text-center">
            <button type="submit" class="btn btn-info btn-fill float-right">
              Publish
            </button>
          </div>
          <div class="clearfix"></div>
        </form>
      </card>

    </div>
  </div>
</template>
<script>
  import ChartCard from 'src/components/Cards/ChartCard.vue'
  import StatsCard from 'src/components/Cards/StatsCard.vue'
  import LTable from 'src/components/Table.vue'


  export default {
    components: {
      LTable,
      ChartCard,
      StatsCard
    },
    data () {
      return {
        selectedEvents: [], //'',
        eventsList: [],
        selectedACs: [],
        acList: [],
        jsonEvents: [{"name": 123423432}] 

      }
    },
    mounted () {
      const assetsContext = require.context('@/assets/eventsjson', false, /\.(json)$/);
      const jsonFiles = assetsContext.keys().map(key => assetsContext(key));
      console.log(jsonFiles);

      // Build the select options 
      for(let i = 0; i < jsonFiles.length; i++) {
        const jsonArray = jsonFiles[i]
        for(let j = 0; j < jsonArray.length; j++) {
          // If associated with an AC
          if( jsonArray[j].acID ) {
            this.acList.push(jsonArray[j])
          }
          // Else, just a list of events
          else {
            this.eventsList.push(jsonArray[j])
          }
        }
      }
    },
    methods: {
      selectEvents() {
        console.log("selectedEvents --- ", this.selectedEvents)
        // clear jsonEvents
        // push to jsonEvents
      },
      selectACs() {
        console.log("selected ACs --- ", this.selectedACs)
      }
    },
    computed: {
      formattedJson() {
        return JSON.stringify(this.jsonEvents, null, 2);
      }
    }

  }
</script>
<style>
.center {
  margin: auto;
}

</style>
