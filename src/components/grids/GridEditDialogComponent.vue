<template>
    <div>

        <v-dialog v-model="editDialog" max-width="290" persistent>

            <v-card>
                <v-card-title v-if="item" class="headline">{{ item.type }} {{ item.name }}</v-card-title>

                    <v-card-text>
                        <v-flex xs12 v-if="item.value">
                            <v-text-field
                                v-model="item.value"
                            ></v-text-field>
                        </v-flex>
                    </v-card-text>

                    <v-card-actions>
                        <v-spacer></v-spacer>

                        <v-btn color="primary" flat="flat" @click="cancelEditClicked">Cancel</v-btn>
                        <v-btn color="primary" flat="flat" @click="saveEditClicked">Save</v-btn>
                    </v-card-actions>
            </v-card>

        </v-dialog>

    </div>
</template>

<script>
export default {
    name: 'GridEditDialogComponent',

    props: [
        'editDialog', 'editObject'
    ],

    watch: {
        editObject (value) {
            if (value) {
                Object.assign(this.item, value)
            }
        }
    },

    data () {
        return {
            item: {}
        }
    },

    methods: {
        cancelEditClicked () {
            this.$emit('cancelEditClicked')
            this.item = {}
        },

        saveEditClicked () {
            this.$emit('saveEditClicked', this.item)
            this.item = {}
        }
    }
}
</script>
