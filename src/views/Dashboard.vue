<script setup>
import BestSellingWidget from '@/components/dashboard/BestSellingWidget.vue';
import NotificationsWidget from '@/components/dashboard/NotificationsWidget.vue';
import RecentSalesWidget from '@/components/dashboard/RecentSalesWidget.vue';
import RevenueStreamWidget from '@/components/dashboard/RevenueStreamWidget.vue';
import StatsWidget from '@/components/dashboard/StatsWidget.vue';
</script>

<template>
    <div>
        <div class="grid grid-cols-12 gap-8">
            <StatsWidget />
        </div>

        <Fluid>
            <div class="flex mt-8">
                <div class="card flex flex-col gap-4 w-full">
                    <div class="font-semibold text-xl">Send Event(s) or AC(s)</div>
                    <div class="flex flex-col md:flex-row gap-4">
                        <div class="flex flex-wrap gap-2 w-full">
                            <label for="firstname">Select AC(s)</label>
                            <select id="firstname" multiple="true" v-model="selectedACs" @change="selectACs($event)">
                                <option v-for="ac in acList" :key="ac.id" :value="ac.id">{{ ac.name }}</option>
                            </select>
                            <!-- <InputText id="firstname2" type="text" /> -->
                        </div>
                        <div class="flex flex-wrap gap-2 w-full">
                            <label for="lastname">Select Event(s)</label>
                            <select multiple="true" class="form-control" v-model="selectedEvents" @change="selectEvents($event)">
                                <option v-for="event in eventsList" :key="event.id" :value="event.id">{{ event.name }}</option>
                            </select>
                        </div>
                    </div>

                    <div class="flex flex-wrap">
                        <label for="address">Event JSON</label>
                        <textarea rows="10" id="address" class="form-control border-input" placeholder="" v-model="formattedJson"> </textarea>
                        <!-- <Textarea id="address" rows="4" /> -->
                    </div>

                    <div class="flex flex-col md:flex-row gap-4">
                        <div class="flex flex-wrap gap-2 w-full">
                            <label for="state">State</label>
                            <Select id="state" v-model="dropdownItem" :options="dropdownItems" optionLabel="name" placeholder="Select One" class="w-full"></Select>
                        </div>
                        <div class="flex flex-wrap gap-2 w-full">
                            <label for="zip">Zip</label>
                            <InputText id="zip" type="text" />
                        </div>
                    </div>
                    <div class="text-center">
                        <button type="submit" class="btn btn-info btn-fill float-right">Publish</button>
                        <button type="submit" @click="handleClear" class="btn btn-secondary btn-fill float-right clear-btn">Clear</button>
                    </div>
                </div>
            </div>
        </Fluid>
    </div>
</template>
<script>
export default {
    components: {},
    data() {
        return {
            selectedEvents: [{ id: 0, name: 'Select Event' }],
            eventsList: [],
            selectedACs: [],
            acList: [],
            jsonEvents: []
        };
    },
    mounted() {
        const jsonFiles = import.meta.glob('@/assets/eventsjson/*.json', { as: 'json', eager: true });
        const data = [];
        for (const path in jsonFiles) {
            data.push(jsonFiles[path]);
        }

        // Build the event select options.
        for (let i = 0; i < data.length; i++) {
            const jsonArray = data[i].default;
            for (let j = 0; j < jsonArray.length; j++) {
                // If associated with an AC
                if (jsonArray[j].acID) {
                    this.acList.push(jsonArray[j]);
                }
                // Else, just a list of events
                else {
                    this.eventsList.push(jsonArray[j]);
                }
            }
        }
    },
    methods: {
        // Build events selects & table CRUD list
        selectEvents() {
            this.selectedACs = [];
            if (this.selectedEvents.length > 0) {
                this.jsonEvents = [];

                for (let i = 0; i < this.selectedEvents.length; i++) {
                    const event = this.eventsList.find((obj) => obj.id === this.selectedEvents[i]);
                    this.jsonEvents.push(event);

                    //this.tableData.push(event)
                }
            }
        },
        selectACs() {
            this.selectedEvents = [];
            if (this.selectedACs.length > 0) {
                this.jsonEvents = [];

                for (let i = 0; i < this.selectedACs.length; i++) {
                    const event = this.acList.find((obj) => obj.id === this.selectedACs[i]);
                    this.jsonEvents.push(event);
                }
            }
        },
        handleClear() {
            this.selectedACs = [];
            this.selectedEvents = [];
            this.jsonEvents = [];
        }
    },
    computed: {
        formattedJson() {
            return this.jsonEvents.length > 0 ? JSON.stringify(this.jsonEvents, null, 2) : '';
        }
    }
};
</script>
<style>
.center {
    margin: auto;
}
.clear-btn {
    margin-left: 10px;
}
.cardheader {
    width: 25%;
}
.cardbtn {
    margin-top: 15px;
    margin-right: 15px;
    position: absolute;
    top: 0;
    right: 0;
}
</style>

