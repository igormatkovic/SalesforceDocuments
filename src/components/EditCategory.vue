<template>
    <div class="category-blick">
        <select @change="changeCategory($event, rowData)" v-bind:value="rowData.category">
            <option v-for="category in categoryTypes">
                {{category}}
            </option>
        </select>
    </div>
</template>

<script>

    export default {
        data() {
            return {
                categoryTypes: SfDocuments.fileCategories
            }
        },
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
            changeCategory(event, row) {
                console.log(event);
                console.log(row);
                console.log(event.target.value);
            }
        }
    }
</script>

<style>
    .edit-type-input {
        display:none;
        font-size:14px;
        width:100%;
        box-sizing: border-box;
        background:transparent;
        border:0;
    }
    .custom-actions button.ui.button {
        padding: 8px 8px;
    }
    .custom-actions button.ui.button > i.icon {
        margin: auto !important;
    }
</style>
