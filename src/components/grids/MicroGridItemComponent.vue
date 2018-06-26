<!--
    Author      : Zelmi Coertze
    Date        : 13/06/2018
    Description :
    The third level of grids, contains the inspection data.
-->

<template>
    <div>

        <div
            @click="micItemClicked(micItem)"
            class="mic"
            v-bind:id="micItem.id"
        >

            <div v-if="micItem.trend === 'inc'" class="micInc">
                <v-icon>arrow_upward</v-icon>
            </div>

            <div v-else-if="micItem.trend === 'dec'" class="micDec">
                <v-icon>arrow_downward</v-icon>
            </div>

            <div v-else-if="micItem.trend === 'none'" class="micNone">
                <v-icon>arrow_forward</v-icon>
            </div>

            <div v-else-if="micItem.value" class="micProgress">
                <v-progress-circular
                    :rotate="-90"
                    :value="parseInt(micItem.value)"
                    color="primary"
                    >
                    {{ micItem.value }}
                </v-progress-circular>
            </div>
        </div>

    </div>
</template>

<script>
export default {
    name: 'MicroGridItemComponent',

    props: [
        'micItem'
    ],

    methods: {
        micItemClicked (micItem) {
            this.$emit('micItemClicked', micItem)
        }
    }
}
</script>

<style>
    .mic {
        border: 1px solid gray;
        color: rgb(64, 64, 64);;
        height: 38px;
        width: 38px;
        text-align: center;
    }

    .micExpanded {
        border: 1px solid gray;
        color: rgb(64, 64, 64);;
        height: 38px;
        width: 38px;
        text-align: center;
    }

    .micInc {
        height: 100%;
        background-color: rgb(255, 153, 153);
        padding: 5px;
    }

    .micDec {
        height: 100%;
        background-color: rgb(255, 194, 153);
        padding: 5px;
    }

    .micNone {
        height: 100%;
        background-color: rgb(173, 235, 173);
        padding: 5px;
    }

    .micProgress {
        padding-top: 2px;
    }
</style>
