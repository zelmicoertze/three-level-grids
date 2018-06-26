<!--
    Author      : Zelmi Coertze
    Date        : 13/06/2018
    Description :
    The second level of grids, contains the micro levels.
-->

<template>
    <div>

        <div
            @click="subItemClicked(subItem)"
            class="sub"
            v-if="subItem.expanded === false"
            v-bind:id="subItem.id"
        >

            <div v-if="subItem.progress && subItem.progress === 'Start' && editMode === true" class='subStart'>
                <v-select
                    :items="progressStates"
                    v-model="subItem.progress"
                    label="Progress"
                    single-line
                    color="grey darken-4"
                ></v-select>
            </div>

            <div v-else-if="subItem.progress && subItem.progress === 'Start' && editMode === false" class='subStart'>
                Progress: {{ subItem.progress }}
            </div>

            <div v-else-if="subItem.progress && subItem.progress === 'Busy' && editMode === true" class='subBusy'>
                <v-select
                    :items="progressStates"
                    v-model="subItem.progress"
                    label="Progress"
                    single-line
                    color="grey darken-4"
                ></v-select>
            </div>

            <div v-else-if="subItem.progress && subItem.progress === 'Busy' && editMode === false" class='subBusy'>
                Progress: {{ subItem.progress }}
            </div>

            <div v-else-if="subItem.progress && subItem.progress === 'Done' && editMode === true" class='subDone'>
                <v-select
                    :items="progressStates"
                    v-model="subItem.progress"
                    label="Progress"
                    single-line
                    color="grey darken-4"
                ></v-select>
            </div>

            <div v-else-if="subItem.progress && subItem.progress === 'Done' && editMode === false" class='subDone'>
                Progress: {{ subItem.progress }}
            </div>

            <div v-else>
                Sub {{ subItem.name }}
            </div>

        </div>

        <div
            class="subExpanded"
            v-else
            @click="subItemClicked(subItem)"
            v-bind:id="subItem.id"
        >

            <table class="gridTable">
                <tr v-for="i in parseInt(micRows)" v-bind:key="i">

                    <td v-for="j in parseInt(micColumns)" v-bind:key="j" align="center">
                        <div
                            v-for="micItem in subItem.children"
                            v-bind:key="micItem.id"
                            v-if="micItem.row == i && micItem.column == j"
                        >
                            <mic-grid-item
                                v-bind:micItem="micItem"
                                v-on:micItemClicked="micItemClicked"
                            ></mic-grid-item>
                        </div>
                    </td>

                </tr>
            </table>
        </div>

    </div>
</template>

<script>
import MicroGridItemComponent from '../grids/MicroGridItemComponent'

export default {
    name: 'SubGridItemComponent',

    components: {
        'mic-grid-item': MicroGridItemComponent
    },

    methods: {
        subItemClicked (subItem) {
            if (!this.suppressSubClick) {
                this.$emit('subItemClicked', subItem)
            }

            this.suppressSubClick = false
        },

        micItemClicked (micItem) {
            this.suppressSubClick = true
            this.$emit('micItemClicked', micItem)
        }
    },

    props: [
        'subItem', 'editMode'
    ],

    data () {
        return {
            suppressSubClick: false,
            micRows: 0,
            micColumns: 0,
            progressStates: [
                'Start', 'Busy', 'Done'
            ]
        }
    },

    created () {
        if (this.subItem.children) {
            this.subItem.children.forEach(item => {
                if (item.row > this.micRows) {
                    this.micRows = item.row
                }

                if (item.column > this.micColumns) {
                    this.micColumns = item.column
                }
            })
        }
    }
}
</script>

<style>
    .sub {
        border: 1px solid gray;
        color: rgb(64, 64, 64);
        height: 91px;
        width: 91px;
        text-align: center;
    }

    .subExpanded {
        border: 1px solid gray;
        color: rgb(64, 64, 64);
        height: 91px;
        width: 91px;
        text-align: center;
        padding: 3px;
    }

    .subStart {
        height: 100%;
        background-color:  rgb(102, 163, 255);
        padding: 5px;
    }

    .subBusy {
        height: 100%;
        background-color: rgb(255, 224, 102);
        padding: 5px;
    }

    .subDone {
        height: 100%;
        background-color:  rgb(133, 224, 133);
        padding: 5px;
    }
</style>
