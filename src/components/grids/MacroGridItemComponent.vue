<!--
    Author      : Zelmi Coertze
    Date        : 13/06/2018
    Description :
    The first level of grids, contains the sub levels.
-->

<template>
    <div>

        <div
            class="mac"
            v-if="macItem.expanded === false"
            @click="macroItemClicked(macItem)"
            v-bind:id="macItem.id"
        >
            <div v-if="macItem.unit" class="macUnit">
                Unit {{ macItem.unit }}
            </div>

            <div v-else-if="!macItem.value">
                Mac {{ macItem.name }}
            </div>

            <div v-else-if="macItem.value < 3" class="macLow">
                <!-- Mac {{ macItem.name }} -->
                {{ macItem.value }}
            </div>

            <div v-else-if="macItem.value > 5" class="macHigh">
                <!-- Mac {{ macItem.name }} -->
                {{ macItem.value }}
            </div>

            <div v-else class="macNormal">
                <!-- Mac {{ macItem.name }} -->
                {{ macItem.value }}
            </div>

        </div>

        <div
            class="macExpanded"
            v-else
            @click="macroItemClicked(macItem)"
            v-bind:id="macItem.id"
        >

            <table class="gridTable">
                <!-- <tr>
                    <td v-for="j in parseInt(subColumns) + 1" v-bind:key="j" align="center" class="subNumbering">
                        <div v-show="j > 1">{{ j - 1 }}</div>
                    </td>
                </tr> -->

                <tr v-for="i in parseInt(subRows)" v-bind:key="i">

                    <!-- <td class="subNumbering">{{ i }}</td> -->

                    <td v-for="j in parseInt(subColumns)" v-bind:key="j" align="center">
                        <div
                            v-for="subItem in macItem.children"
                            v-bind:key="subItem.id"
                            v-if="subItem.row == i && subItem.column == j"
                        >
                            <sub-grid-item
                                v-bind:subItem="subItem"
                                v-on:subItemClicked="subItemClicked"
                                v-on:micItemClicked="micItemClicked"
                                :editMode="editMode"
                            ></sub-grid-item>
                        </div>
                    </td>

                </tr>
            </table>

        </div>

    </div>
</template>

<script>
import SubGridItemComponent from '../grids/SubGridItemComponent'

export default {
    name: 'MacroGridItemComponent',

    components: {
        'sub-grid-item': SubGridItemComponent
    },

    props: [
        'macItem', 'editMode'
    ],

    data () {
        return {
            suppressMacroClick: false,
            subRows: 0,
            subColumns: 0
        }
    },

    created () {
        if (this.macItem.children) {
            this.macItem.children.forEach(item => {
                if (item.row > this.subRows) {
                    this.subRows = item.row
                }

                if (item.column > this.subColumns) {
                    this.subColumns = item.column
                }
            })
        }
    },

    methods: {
        macroItemClicked (macItem) {
            if (!this.suppressMacroClick) {
                this.$emit('macroItemClicked', macItem)
            }

            this.suppressMacroClick = false
        },

        subItemClicked (subItem) {
            this.suppressMacroClick = true

            this.$emit('subItemClicked', subItem)
        },

        micItemClicked (micItem) {
            this.suppressMacroClick = true

            this.$emit('microItemClicked', micItem)
        }
    }
}
</script>

<style>
    .mac {
        border: 1px solid gray;
        color: rgb(64, 64, 64);
        height: 50px;
        width: 50px;
        text-align: center;
    }

    .macExpanded {
        border: 1px solid gray;
        color: rgb(64, 64, 64);
        height: 200px;
        width: 200px;
        text-align: center;
        padding: 5px;
    }

    .macLow {
        height: 100%;
        background-color: rgb(255, 102, 102);
        padding-top: 5px;
    }

    .macHigh {
        height: 100%;
        background-color: rgb(255, 179, 102);
        padding-top: 5px;
    }

    .macNormal {
        height: 100%;
        background-color: rgb(217, 140, 179);
        padding-top: 5px;
    }

    .subNumbering {
        padding-right: 5px;
        color: light;
    }
</style>
