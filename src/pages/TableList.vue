<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <!-- <div class="col-12">
          <card class="strpied-tabled-with-hover"
                body-classes="table-full-width table-responsive"
          >
            <template slot="header">
              <h4 class="card-title">Striped Table with Hover</h4>
              <p class="card-category">Here is a subtitle for this table</p>
            </template>
            <l-table class="table-hover table-striped"
                     :columns="table1.columns"
                     :data="table1.data">
            </l-table>
          </card>

        </div> -->

        <!-- <div class="col-12">
          <card class="card-plain">
            <template slot="header">
              <h4 class="card-title">Table on Plain Background</h4>
              <p class="card-category">Here is a subtitle for this table</p>
            </template>
            <div class="table-responsive">
              <l-table class="table-hover"
                       :columns="table2.columns"
                       :data="table2.data">
              </l-table>
            </div>
          </card>
        </div> 

        <div class="col-12">
          <card class="strpied-tabled-with-hover"
                body-classes="table-full-width table-responsive"
          >
            <template slot="header">
              <h4 class="card-title">Events CRUD</h4>
              <p class="card-category">Add and Delete Events</p>
            </template>
            <l-table class="table-hover table-striped table-sm"
                     :columns="table1.columns"
                     :data="table1.data">
            </l-table>
          </card>

        </div> -->





              
      <!-- <card class="col-md-6"> AN OPTION.... -->
      <card class="strpied-tabled-with-hover col-md-12"
                body-classes="table-full-width table-responsive"
          >
        <template slot="header">
          <div class="cardheader">
          <h4 class="card-title">Events CRUD</h4>
          </div>
          <div class="cardbtn">
              
      
            <button class="btn btn-success float-right" @click="openModal">Add Event</button>
            <ModalComponent :is-visible="isModalVisible" @close="closeModal">
              <!-- <p>This is the modal content.</p> -->
              <div class="content">
                <edit-profile-form>
                </edit-profile-form>
              </div>
            </ModalComponent>
        
          </div>
            <!-- </div> -->
          <!-- <p class="card-category">Add and Delete Events</p> -->
        </template>
        <l-table class="table-hover table-striped table-sm"
                  :columns="table1.columns"
                  :data="table1.data">
        </l-table>
      </card>
      <!-- </card> -->





      </div>
    </div>
  </div>
</template>
<script>
  import LTable from 'src/components/Table.vue'
  import Card from 'src/components/Cards/Card.vue'
  import ModalComponent from 'src/components/ModalComponent.vue';
  import EditProfileForm from './UserProfile/EditProfileForm.vue'
  import UserCard from './UserProfile/UserCard.vue'

  const tableColumns = ['ACID', 'Name', 'AOC', 'AlertType']
  const tableData = [{
    acid: 1,
    name: 'Event 1',
    aoc: 'Space Race',
    alerttype: 'L1',
  },
  {
    acid: 2,
    name: 'Event 2',
    aoc: 'Ground',
    alerttype: 'L3',
  },
  {
    acid: 3,
    name: 'Sage Rodriguez',
    aoc: '$56,142',
    alerttype: 'Netherlands',
  },
  {
    acid: 4,
    name: 'Philip Chaney',
    aoc: '$38,735',
    alerttype: 'Korea, South',
  },
  {
    acid: 5,
    name: 'Doris Greene',
    aoc: '$63,542',
    alerttype: 'Malawi',
  }]
  export default {
    components: {
      LTable,
      Card,
      ModalComponent,
      EditProfileForm,
      UserCard
    },
    data () {
      return {
         isModalVisible: false,
        table1: {
          columns: [...tableColumns],
          data: [...tableData]
        },
        table2: {
          columns: [...tableColumns],
          data: [...tableData]
        }
      }
    },
    methods: {
    openModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
    },
  }
</script>
<style>
</style>