<!--
    Author      : Zelmi Coertze
    Date        : 13/06/2018
    Description :
    A component that contains and displays multi-level grids.
-->

<template>
<v-app>
    <v-content>

        <div>
            <table class="gridTable">

                <!-- <tr>
                    <td v-for="j in parseInt(macColumns) + 1" v-bind:key="j" align="center" class="macNumbering">
                        <div v-show="j > 1">{{ j - 1 }}</div>
                    </td>
                </tr> -->

                <tr v-for="i in parseInt(macRows)" v-bind:key="i">
                    <!-- <td class="macNumbering">{{ i }}</td> -->

                    <td v-for="j in parseInt(macColumns)" v-bind:key="j" align="center">
                        <div
                            v-for="macItem in data"
                            v-bind:key="macItem.id"
                            v-if="macItem.row == i && macItem.column == j"
                        >
                            <macro-grid-item
                                v-bind:macItem="macItem"
                                v-on:macroItemClicked="macroItemClicked"
                                v-on:subItemClicked="subItemClicked"
                                v-on:microItemClicked="microItemClicked"
                                :editMode="editMode"
                            ></macro-grid-item>
                        </div>
                    </td>
                </tr>

            </table>

            <!-- <v-btn fab small @click="zoomInGetElements()" color="primary">
                <v-icon>
                    add
                </v-icon>
            </v-btn> -->

            <grid-edit-dialog
                :editDialog="editDialog"
                :editObject="editObject"
                v-on:cancelEditClicked="cancelEditClicked"
                v-on:saveEditClicked="saveEditClicked"
            ></grid-edit-dialog>

            <div class="select">
                <v-flex xs12 md2>
                    <v-select
                        :items="arrData"
                        v-model="selectedDataset"
                        label="Dataset"
                    ></v-select>
                </v-flex>
            </div>

            <v-flex xs12 md2>
                <v-btn @click="clearSelection()" color="primary">Clear Selection</v-btn>
            </v-flex>

            <div id="switch">

                <v-switch
                    label="Edit Mode: On"
                    v-model="editMode"
                    v-if="editMode === true"
                ></v-switch>

                <v-switch
                    label="Edit Mode: Off"
                    v-model="editMode"
                    v-else
                ></v-switch>

            </div>

            <v-snackbar
                v-model="snackbar"
                :timeout=1500
                color="grey darken-1"
                top
            >
                {{ snackbarText }}
                <v-btn dark flat @click="snackbar = false">Close</v-btn>
            </v-snackbar>

        </div>

    </v-content>
</v-app>
</template>

<script>
import MacroGridItemComponent from '../grids/MacroGridItemComponent'
import { gridData1, gridData2, gridData3, gridData4 } from '../grids/DummyGridData.js'
import GridEditDialogComponent from '../grids/GridEditDialogComponent'

export default {
    name: 'GridContainerComponent',

    components: {
        'macro-grid-item': MacroGridItemComponent,
        'grid-edit-dialog': GridEditDialogComponent
    },

    watch: {
        selectedDataset (value) {
            if (value === 'gridData1') {
                this.data = gridData1
            } else if (value === 'gridData2') {
                this.data = gridData2
            } else if (value === 'gridData3') {
                this.data = gridData3
            } else if (value === 'gridData4') {
                this.data = gridData4
            }

            this.editMode = false
        }
    },

    data () {
        return {
            data: gridData1,
            arrData: [
                'gridData1', 'gridData2', 'gridData3', 'gridData4'
            ],
            macRows: 0,
            macColumns: 0,
            editMode: false,
            editDialog: false,
            editObject: {},
            snackbar: false,
            snackbarText: '',
            selectedDataset: 'gridData1'
        }
    },

    methods: {
        macroItemClicked (macItem) {
            if (!this.editMode) {
                if (macItem.children) {
                    for (let index = 0; index < this.data.length; index++) {
                        if (this.data[index].id === (macItem.id)) {
                            if (this.data[index].expanded === true) {
                                this.data[index].expanded = false
                                for (let childIndex = 0; childIndex < this.data[index].children.length; childIndex++) {
                                    if (this.data[index].children[childIndex].expanded === true) {
                                        this.data[index].children[childIndex].expanded = false
                                    }
                                }
                            } else {
                                this.data[index].expanded = true
                            }
                        }
                    }
                } else {
                    this.snackbarText = macItem.id
                    this.snackbar = true
                }
            } else {
                this.editDialog = true
                this.editObject = macItem
            }
        },

        subItemClicked (subItem) {
            if (!this.editMode) {
                if (subItem.children) {
                    for (let index = 0; index < this.data.length; index++) {
                        if (this.data[index].children) {
                            for (let childIndex = 0; childIndex < this.data[index].children.length; childIndex++) {
                                if (this.data[index].children[childIndex].id === (subItem.id)) {
                                    if (this.data[index].children[childIndex].expanded === true) {
                                        this.data[index].children[childIndex].expanded = false
                                    } else {
                                        this.data[index].children[childIndex].expanded = true
                                    }
                                }
                            }
                        }
                    }
                } else {
                    this.snackbarText = subItem.id
                    this.snackbar = true
                }
            } else {
                // this.editDialog = true
            }
        },

        microItemClicked (micItem) {
            if (!this.editMode) {
                this.snackbarText = micItem.id
                this.snackbar = true
            } else {
                this.editDialog = true
                this.editObject = micItem
            }
        },

        clearSelection () {
            for (let row = 1; row <= this.macRows; row++) {
                for (let column = 1; column <= this.macColumns; column++) {
                    for (let index = 0; index < this.data.length; index++) {
                        if (this.data[index].expanded === true) {
                            this.data[index].expanded = false
                        }
                        if (this.data[index].children) {
                            for (let childIndex = 0; childIndex < this.data[index].children.length; childIndex++) {
                                if (this.data[index].children[childIndex].expanded === true) {
                                    this.data[index].children[childIndex].expanded = false
                                }
                            }
                        }
                    }
                }
            }
        },

        cancelEditClicked () {
            this.editDialog = false
            this.editObject = {}
        },

        saveEditClicked (item) {
            if (item.trend) {
                if (item.value > this.editObject.value) {
                    item.trend = 'inc'
                } else if (item.value < this.editObject.value) {
                    item.trend = 'dec'
                } else {
                    item.trend = 'none'
                }
            }

            Object.assign(this.editObject, item)
            this.cancelEditClicked()
        }
    },

    created () {
        this.data.forEach(item => {
            if (item.row > this.macRows) {
                this.macRows = item.row
            }

            if (item.column > this.macColumns) {
                this.macColumns = item.column
            }
        })
    }
}
</script>

<style>
    .macNumbering {
        color: lightgray;
        padding: 5px;
    }

    .switch {
        margin-left: 10px;
    }

    .select {
        margin-left: 10px;
    }

    body {
        user-select: none;
    }
</style>
