<template>
    <div id="main" ref="main" class="h-100 px-2 py-1 bg-dark" style="overflow:auto;">
        <ScenarioItem v-for="(item, index) in list" :key="JSON.stringify(item)+index" :item="item"></ScenarioItem>
    </div>
</template>
<script>
import { Data } from "../js/data.js";
import { Event } from "../js/event.js";

import ScenarioItem from "./ScenarioItem.vue";

export default {
    props: {
        type: String,
        id: String
    },
    created: function() {
        var $vm = this;
        Event.$on("set-current-scenario", function(type, id) {
            $vm.$refs.main.scrollTop = 0;
        });
    },
    methods: {},
    computed: {
        list: function() {
            return Data.getAll("scenario_" + this.type)[this.id];
        }
    },
    components: {
        ScenarioItem
    }
};
</script>
