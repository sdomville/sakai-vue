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
 

      <div class="row">
        <card class="col-md-12">
          <h4 slot="header" class="card-title">Send Event(s) or AC(s)</h4>
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
            <div class="text-center">
              <button type="submit" @click="handleClear" class="btn btn-secondary btn-fill float-right clear-btn">
                Clear
              </button>
            </div>
            <div class="clearfix"></div>
          </form>
        </card>
      </div>

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
        selectedEvents: [{id: 0, name: "Select Event"}],
        eventsList: [],
        selectedACs: [],
        acList: [],
        jsonEvents: [],
        icons: ["fa fa-paper-plane", "fa fa-trash"]
 
      }
    },
    mounted () {
      const assetsContext = require.context('@/assets/eventsjson', false, /\.(json)$/);
      const jsonFiles = assetsContext.keys().map(key => assetsContext(key));
      console.log(jsonFiles);
 
      // Build the event select options.
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
      // Build events selects & table CRUD list
      selectEvents() {
        this.selectedACs = [];
        if( this.selectedEvents.length > 0 ) {
          this.jsonEvents = [];
        
          for( let i = 0; i < this.selectedEvents.length; i++ ) {
            const event = this.eventsList.find(obj => obj.id === this.selectedEvents[i]);
            this.jsonEvents.push(event)

            //this.tableData.push(event)
          }
        }
      },
      selectACs() {
        this.selectedEvents = [];
        if( this.selectedACs.length > 0 ) {
          this.jsonEvents = [];
        
          for( let i = 0; i < this.selectedACs.length; i++ ) {
            const event = this.acList.find(obj => obj.id === this.selectedACs[i]);
            this.jsonEvents.push(event)
          }
        }
      },
      handleClear() {
        this.selectedACs = [];
        this.selectedEvents = [];
        this.jsonEvents = [];
      },
    },
    computed: {
      formattedJson() {
        return this.jsonEvents.length > 0 ? JSON.stringify(this.jsonEvents, null, 2) : '';
      }
    }
 
  }
</script>
<style>
  .center {
    margin: auto;
  }
  .clear-btn {
    margin-right: 20px;
  }
  .cardheader {
    width: 25%;
  }
  .cardbtn {
    margin-top: 15px;
    margin-right: 15px;
    position:absolute;
    top:0;
    right:0;
  }
 
</style>
