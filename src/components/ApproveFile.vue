<template>
    <div class="edit-block">
        <div class="edit-name" @click="editName(rowData, rowIndex)">{{rowData.name}}</div>
        <input class="edit-name-input"
               @blur="cancelEdit(rowData, rowIndex)"
               @keyup.enter="saveEdit(rowData, rowIndex)"
               @keyup.esc="cancelEdit(rowData, rowIndex)"
        />
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
                let elDiv = this.$el.children[0];
                let elInput = this.$el.children[1];
                if(show) {
                    elDiv.style.display = 'none';
                    elInput.style.display = 'block';
                    elInput.value = name;
                    elInput.focus();
                } else {
                    elInput.style.display = 'none';
                    elDiv.style.display = 'block';
                    elDiv.value = name;
                }
            },
            editName (data, index) {
                this.showEdit(data.name, true);
                console.log('Edit', data.name, index)
            },
            saveEdit (data, index) {
                let name = this.$el.children[1].value;
                this.showEdit(name, false);
                console.log('save', name, index)
            },
            cancelEdit (data, index) {
                this.showEdit(data.name, false);
                console.log('cancel: ', data.name, index)
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
