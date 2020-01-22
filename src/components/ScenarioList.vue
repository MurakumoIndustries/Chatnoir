<template>
    <div class="scenario-list">
        <ScenarioListPart
            id="scenario-list-scroller"
            :list="list"
            :isExpand="true"
            style="overflow-y:auto;"
            :activeType="activeType"
            :activeId="activeId"
        />
    </div>
</template>

<script>
import { Data } from "../js/data.js";
import { Event } from "../js/event.js";

import Collapse from "./Collapse.vue";
import ScenarioListPart from "./ScenarioListPart.vue";

import VueScrollTo from "vue-scrollto";

export default {
    created: function() {
        var $vm = this;
        Event.$on("set-current-scenario", function(type, id) {
            $vm.setCurrentScenario(type, id);
        });
    },
    props: {
        isShowSidebar: Boolean
    },
    data: function() {
        return {
            searchText: "",
            list: Data.getAll("scenariolist"),
            activeType: "",
            activeId: ""
        };
    },
    methods: {
        setCurrentScenario: function(type, id) {
            var $vm = this;
            console.log("setCurrentScenario");
            this.searchText = "";
            this.activeType = type;
            this.activeId = id;
            ////dirty fix for multiple animations
            var timeout = 0;

            var recursive = function(l) {
                var parentNeedOpen = false;
                for (var i = 0; i < l.length; i++) {
                    var group = l[i];
                    if (group.list) {
                        $vm.$set(group, "isExpand", false);
                        if (recursive(group.list)) {
                            parentNeedOpen = true;
                            $vm.$set(group, "isExpand", true);
                        }
                    } else if (group.type == type && group.id == id) {
                        return true;
                    }
                }
                return parentNeedOpen;
            };
            recursive(this.list);
            //_.each(this.scenariolist, function(group, index) {
            //    var oldGroupIsShow = group.isShow;
            //    group.isShow = false;
            //    if (
            //        _.some(group.scenarios, function(scenario) {
            //            return scenario.id == id;
            //        })
            //    ) {
            //        group.isShow = true;
            //    }
            //    if (group.isShow != oldGroupIsShow) {
            //        timeout = 300;
            //    }
            //});
            this.$nextTick(function() {
                window.setTimeout(function() {
                    VueScrollTo.scrollTo("#" + type + "_" + id, {
                        container: "#scenario-list-scroller"
                    });
                }, timeout);
            });
        }
    },
    components: {
        Collapse,
        ScenarioListPart
    }
};
</script>
<style scoped>
.scenario-list {
    display: flex;
    flex-direction: column;
    align-items: stretch;
}

@media (min-width: 768px) {
    .scenario-list {
        height: 100%;
        border-bottom: none;
    }
}

.scenario-list-search {
    overflow: visible;
}
</style>

