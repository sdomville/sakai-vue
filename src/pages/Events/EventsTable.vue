<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">

        <card class="strpied-tabled-with-hover col-md-12"
                  body-classes="table-full-width table-responsive"
            >
          <template slot="header">
            <div class="cardheader">
            <h4 class="card-title">Events CRUD</h4>
            </div>
            <button class="btn btn-success float-right" @click="openModal">Add Event</button>
          </template>
          <l-table class="table-hover table-striped table-sm"
                    :columns="table1.columns"
                    :data="table1.data"
                    :crudIcons="icons">
          </l-table>
        </card>
        
      </div>
    </div>
    <ModalComponent :is-visible="isModalVisible">
      <template #content class="content">
        <add-event @close="closeModal">
        </add-event>
      </template>
    </ModalComponent>
  </div>
</template>
<script>
  import LTable from 'src/components/Table.vue'
  import Card from 'src/components/Cards/Card.vue'
  import ModalComponent from 'src/components/ModalComponent.vue';
  import AddEvent from './AddEvent.vue'
  import UserCard from './UserCard.vue'

  const tableColumns = ['AC ID', 'Name', 'area of concern', 'Alert Type']
  const tableData = [{
    "ac id": 1,
    name: 'Event 1',
    "area of concern": 'Space Race',
    "alert type": 'L1',
  },
  {
    "ac id": 2,
    name: 'Event 2',
    "area of concern": 'Ground',
    "alert type" : 'L3',
  },
  {
    "ac id": 3,
    name: 'Sage Rodriguez',
    "area of concern": '$56,142',
    "alert type": 'Netherlands',
  },
  {
    "ac id": 4,
    name: 'Philip Chaney',
    "area of concern": '$38,735',
    "alert type": 'Korea, South',
  },
  {
    "ac id": 5,
    name: 'Doris Greene',
    "area of concern": '$63,542',
    "alert type": 'Malawi',
  }]
  export default {
    components: {
      LTable,
      Card,
      ModalComponent,
      AddEvent,
      UserCard
    },
    data () {
      return {
        isModalVisible: false,
        icons: ["fa fa-paper-plane", "fa fa-trash"],
        table1: {
          columns: [...tableColumns],
          data: [...tableData]
        },
      }
    },
    methods: {
      openModal() {
        this.isModalVisible = true;
      },
      closeModal() {
        this.isModalVisible = false;
      },
      myFunction(data) {
        console.log('Function from parent component executed', data);
      }
    },
  }
</script>
<style>
</style>