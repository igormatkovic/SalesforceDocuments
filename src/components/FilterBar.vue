<template>
    <div class="filter-bar control is-grouped">
        <p class="control">
            <a @click="triggerShowDeleted" v-bind:class="{ 'is-dark': showDeleted }" class="button trash-button ">
                <span class="icon">
                  <i class="fa fa-trash"></i>
                </span>
                <span>Show trash</span>
            </a>
        </p>
        <p class="control">
            <a @click="doShare" class="button" :disabled="!selectedTo">Share</a>
        </p>
        <p class="control">
            <a @click="doCopy" class="button" :disabled="!selectedTo">Copy</a>
        </p>
        <p class="control is-expanded has-addons">
            <input id="search" type="text" v-model="filterText" class="input" @keyup.enter="doFilter"
                   placeholder="Search name, nickname, or email">
            <a @click="doFilter" class="button is-primary">Go</a>
        </p>
        <p class="control">
            <a @click="resetFilter" class="button">Reset</a>
        </p>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                filterText: '',
                showDeleted: false,
                selectedTo: false,
            }
        },
        methods: {
            doFilter () {
                this.$events.fire('filter-set', this.filterText)
            },
            doCopy () {
                this.$events.fire('do-copy', this.filterText)
            },
            doShare () {
                this.$events.fire('do-share', this.filterText)
            },
            resetFilter () {
                this.filterText = ''  // clear the text in text input
                this.$events.fire('filter-reset')
            },
            triggerShowDeleted() {
                this.showDeleted = !this.showDeleted;
                this.$events.fire('trigger-deleted')

                Vuemit.fire('vuetable:checkbox-toggled-all', {
                    isChecked: false
                })
            },
            setSelectedTo(status) {
                this.selectedTo = status;
            }
        },
        mounted() {
            let that = this;
            Vuemit.listen('vuetable:checkbox-toggled', function (data) {
                that.setSelectedTo(data.totalSelected > 0);
            })
            Vuemit.listen('vuetable:checkbox-toggled-all', function (data) {
                that.setSelectedTo(data.isChecked);
            })
            Vuemit.listen('vuetable:change-page', function (data) {
                console.log('triggered');
                that.setSelectedTo(false);
            })
        }
    }
</script>
<style>
    #search {
        width: 300px;
    }
</style>
