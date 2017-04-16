<template>
    <div class="custom-actions">
       {{ rowData.uploaded.name }}
       {{ rowData.uploaded.date }}
    </div>
</template>

<script>
    export default {
        props: {
            rowData: {
                type: Object,
                required: true
            },
            rowIndex: {
                type: Number
            }
        },
        methods: {
            itemAction (action, data, index) {
                console.log('custom-actions: ' + action, data.name, index)
            }
        }
    }
</script>

<style>

</style>
