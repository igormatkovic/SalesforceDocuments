<template>
    <div class="edit-block">
        <div class="edit-name" @click="editName(rowData, rowIndex)">{{rowData.file_name}}</div>
        <input class="edit-name-input"
               @blur="cancelEdit(rowData, rowIndex)"
               @keyup.enter="saveEdit(rowData, rowIndex)"
               @keyup.esc="cancelEdit(rowData, rowIndex)"/>
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
            showEdit(name, show) {
                if(show) {
                    this.$el.children[0].style.display = 'none';
                    this.$el.children[1].style.display = 'block';
                    this.$el.children[1].value = name;
                    this.$el.children[1].focus();
                } else {
                    this.$el.children[1].style.display = 'none';
                    this.$el.children[0].style.display = 'block';
                    this.$el.children[0].value = name;
                }
            },
            editName (data, index) {
                this.showEdit(data.file_name, true);
                console.log('Edit', data.file_name, index)
            },
            saveEdit (data, index) {
                let name = this.$el.children[1].value;
                this.showEdit(name, false);
                console.log('save', name, index)
            },
            cancelEdit (data, index) {
                this.showEdit(data.file_name, false);
                console.log('cancel: ', data.file_name, index)
            }
        }
    }
</script>

<style>
    .edit-name-input {
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
