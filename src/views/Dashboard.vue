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
                            <label for="state">ACs</label>
                            <!-- <Select id="state" v-model="dropdownItem" :options="dropdownItems" optionLabel="name" placeholder="Select One" class="w-full"></Select> -->
                            <MultiSelect v-model="multiselectACValue" :options="acList" @change="selectACs($event)" optionLabel="name" placeholder="Select ACs" :filter="true">
                                <template #value="slotProps">
                                    <div class="inline-flex items-center py-1 px-2 bg-primary text-primary-contrast rounded-border mr-2" v-for="option of slotProps.value" :key="option.id">
                                        <!-- <span :class="'mr-2 flag flag-' + option.code.toLowerCase()" style="width: 18px; height: 12px" /> -->
                                        <div>{{ option.name }}</div>
                                    </div>
                                    <template v-if="!slotProps.value || slotProps.value.length === 0">
                                        <div class="p-1">Select AC(s)</div>
                                    </template>
                                </template>
                                <template #option="slotProps">
                                    <div class="flex items-center">
                                        <!-- <span :class="'mr-2 flag flag-' + slotProps.option.code.toLowerCase()" style="width: 18px; height: 12px" /> -->
                                        <div>{{ slotProps.option.name }}</div>
                                    </div>
                                </template>
                            </MultiSelect>
                        </div>
                        <div class="flex flex-wrap gap-2 w-full">
                            <label for="state">Events</label>
                            <!-- <Select id="state" v-model="dropdownItem" :options="dropdownItems" optionLabel="name" placeholder="Select One" class="w-full"></Select> -->
                            <MultiSelect v-model="multiselectEventValue" :options="eventsList" @change="selectEvents($event)" optionLabel="name" placeholder="Select ACs" :filter="true">
                                <template #value="slotProps">
                                    <div class="inline-flex items-center py-1 px-2 bg-primary text-primary-contrast rounded-border mr-2" v-for="option of slotProps.value" :key="option.id">
                                        <!-- <span :class="'mr-2 flag flag-' + option.code.toLowerCase()" style="width: 18px; height: 12px" /> -->
                                        <div>{{ option.name }}</div>
                                    </div>
                                    <template v-if="!slotProps.value || slotProps.value.length === 0">
                                        <div class="p-1">Select Event(s)</div>
                                    </template>
                                </template>
                                <template #option="slotProps">
                                    <div class="flex items-center">
                                        <!-- <span :class="'mr-2 flag flag-' + slotProps.option.code.toLowerCase()" style="width: 18px; height: 12px" /> -->
                                        <div>{{ slotProps.option.name }}</div>
                                    </div>
                                </template>
                            </MultiSelect>
                        </div>
                        <!-- <div class="flex flex-wrap gap-2 w-full">
                            <label for="zip">Zip</label>
                            <InputText id="zip" type="text" />
                        </div> -->
                    </div>

                    <div class="flex flex-wrap">
                        <label for="address">Event JSON</label>
                        <Textarea id="address" rows="25" v-model="formattedJson"/>
                    </div>

                    <div class="text-center">
                        <!-- TODO: new buttons -->
                        <button type="submit" class="btn btn-info btn-fill float-right">Publish</button>
                        <button type="submit" @click="handleClear" class="btn btn-secondary btn-fill float-right clear-btn">Clear</button>
                    </div>
                </div>
            </div>
        </Fluid>
    </div>
</template>
<script>
import StatsWidget from '@/components/dashboard/StatsWidget.vue';
import { ref } from 'vue';

export default {
    components: { StatsWidget },
    data() {
        return {
            eventsList: [],
            acList: [],
            jsonEvents: [],
            multiselectACValue: ref(null),
            multiselectEventValue: ref(null)
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
            this.multiselectACValue = [];
            if (this.multiselectEventValue.length > 0) {
                this.jsonEvents = [];
                for (let i = 0; i < this.multiselectEventValue.length; i++) {
                    const event = this.eventsList.find((obj) => obj.id === this.multiselectEventValue[i].id);
                    this.jsonEvents.push(event);
                }
            }
        },
        selectACs() {
            this.multiselectEventValue = [];
            if (this.multiselectACValue.length > 0) {
                this.jsonEvents = [];

                for (let i = 0; i < this.multiselectACValue.length; i++) {
                    const event = this.acList.find((obj) => obj.id === this.multiselectACValue[i].id);
                    this.jsonEvents.push(event);
                }
            }
        },
        handleClear() {
            this.multiselectACValue = [];
            this.multiselectEventValue = [];
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
