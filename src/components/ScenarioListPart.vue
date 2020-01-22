<template>
    <Collapse>
        <ul class="list-unstyled collapsable" v-show="isExpand">
            <li v-for="item in list" v-bind:key="item.id">
                <h5 v-if="item.list&&item.list.length">
                    <a
                        :id="'#'+item.id"
                        @click.prevent="toggle(item)"
                    >{{item.nameKey?Ui.getText(item.nameKey):item.name}}</a>
                </h5>
                <ScenarioListPart
                    v-if="item.list&&item.list.length"
                    :list="item.list"
                    :isExpand="item.isExpand"
                    :activeType="activeType"
                    :activeId="activeId"
                />
                <a
                    v-if="!item.list"
                    :id="item.type+'_'+item.id"
                    :data-id="item.id"
                    :href="'#!/scenario/'+item.type+'/'+item.id"
                    :title="item.name"
                    :class="{active:item.type==activeType&&item.id==activeId}"
                >
                    <span>{{item.name}}</span>
                </a>
            </li>
        </ul>
    </Collapse>
</template>
<script>
import Collapse from "./Collapse.vue";

export default {
    name: "ScenarioListPart",
    props: {
        isExpand: Boolean,
        list: Array,
        activeType: String,
        activeId: String
    },
    data: function() {
        return {};
    },
    methods: {
        toggle: function(item) {
            this.$set(item, "isExpand", !item.isExpand);
        }
    },
    components: {
        Collapse
    }
};
</script>
<style scoped>
ul {
    overflow-x: hidden;
    margin-bottom: 0;
}

ul > li {
    margin-bottom: 0.25rem;
}

ul > li > * {
    cursor: pointer;
}

ul ul {
    margin-left: 1rem;
}

ul > li > a {
    color: rgba(0, 0, 0, 0.65);
    text-decoration: none;
    white-space: nowrap;
}

ul > li > a.active {
    font-weight: bold;
}
</style>
