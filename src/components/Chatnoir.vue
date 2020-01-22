<template>
    <div class="container-fluid main-container h-100">
        <div class="row h-100">
            <div class="col-12 col-md-4 col-xl-3 p-0 sidebar">
                <ScenarioList v-bind:isShowSidebar="isShowSidebar" />
            </div>
            <div class="col-12 col-md-8 col-xl-9 p-0 h-100">
                <Scenario
                    v-if="scenario.type&&scenario.id"
                    v-bind:type="scenario.type"
                    v-bind:id="scenario.id"
                />
            </div>
        </div>
    </div>
</template>

<script>
import ScenarioList from "./ScenarioList.vue";
import Scenario from "./Scenario.vue";

import { Data } from "../js/data.js";
import { Event } from "../js/event.js";

export default {
    data: function() {
        return {
            isShowSidebar: true,
            scenario: {
                type: "",
                id: ""
            }
        };
    },
    created: function() {
        var $vm = this;
        Event.$on("render", function(type, id) {
            $vm.scenario.type = type;
            $vm.scenario.id = id;
            Event.$emit("set-current-scenario", type, id);
        });
        Event.$on("toggle-sidebar", function(forceOption) {
            if (typeof forceOption === "boolean") {
                $vm.isShowSidebar = forceOption;
            } else {
                $vm.isShowSidebar = !$vm.isShowSidebar;
            }
        });
    },
    computed: {},
    components: {
        ScenarioList,
        Scenario
    }
};
</script>
<style scoped>
.main-container {
    padding-top: 56px;
}

.sidebar {
    z-index: 16;
    background: white;
    border-right: none;
    position: absolute;
}

@media (min-width: 768px) {
    .sidebar {
        position: initial;
        height: 100%;
        border-right: 1px solid rgba(0, 0, 0, 0.25);
    }
}
</style>


