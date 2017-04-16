<template>
  <div class="container is-fluid">
    <nav class="level is-marginless">
      <div class="level-left">
        <div class="level-item">
          <filter-bar></filter-bar>
        </div>
      </div>
      <div class="level-right">
        <vuetable-pagination-info ref="paginationInfo"></vuetable-pagination-info>
      </div>
    </nav>
    <vuetable ref="vuetable"
      api-url="https://capellaportal.dev/api/test/loans"
      :fields="fields"
      :css="css"
      pagination-path=""
      :multi-sort="true"
      multi-sort-key="ctrl"
      :sort-order="sortOrder"
      detail-row-component="my-detail-row"
      :append-params="moreParams"
      @vuetable:cell-clicked="onCellClicked"
      @vuetable:pagination-data="onPaginationData"
    ></vuetable>
    <bulma-pagination ref="pagination"
      @vuetable-pagination:change-page="onChangePage"
    ></bulma-pagination>
  </div>
</template>

<script>
import accounting from 'accounting'
import moment from 'moment'
import Vuetable from './Vuetable'
import BulmaPagination from './BulmaPagination'
import VuetablePaginationInfo from './VuetablePaginationInfo'
import Vue from 'vue'
import VueEvents from 'vue-events'
Vue.use(VueEvents)
import CustomActions from './CustomActions'
import RowUploaded from './RowUploaded'
import EditName from './EditName'
import EditCategory from './EditCategory'
import DetailRow from './DetailRow'
import FilterBar from './FilterBar'

Vue.component('edit-name', EditName)
Vue.component('edit-category', EditCategory)
Vue.component('custom-actions', CustomActions)
Vue.component('row-uploaded', RowUploaded)
Vue.component('my-detail-row', DetailRow)

var store = {
  debug: true,
  state: {
    message: 'Hello!'
  },
  setMessageAction (newValue) {
    this.debug && console.log('setMessageAction triggered with', newValue)
    this.state.message = newValue
  },
  clearMessageAction () {
    this.debug && console.log('clearMessageAction triggered')
    this.state.message = ''
  }
}
export default {
  components: {
    Vuetable,
    BulmaPagination,
    VuetablePaginationInfo,
    FilterBar
  },
  data () {
    return {
      selectedTo: [],
      css: {
        tableClass: 'table is-bordered is-striped',
        ascendingIcon: 'fa fa-chevron-up',
        descendingIcon: 'fa fa-chevron-down',
        sortHandleIcon: 'fa fa-bars',
      },
      fields: [
        {
          name: '__checkbox',
          title: '#',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered'
        },
        {
          name: '__component:edit-name',
          title: 'File Name',
          sortField: 'file_name'
        },
        {
          name: '__component:edit-category',
          title: 'Category',
          sortField: 'category',
        },
        {
          name: 'approval',
          sortField: 'approval',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered'
        },
        {
          name: 'size',
          sortField: 'size',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered',
          callback: 'formatSize'
        },
        {
          name: '__component:row-uploaded',
          title: 'Uploaded',
          sortField: 'uploaded.date',
          callback: 'allcap'
        },
        {
          name: '__component:custom-actions',
          title: 'Actions',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered',
        }
      ],
      sortOrder: [
          {
            field: 'file_name',
            sortField: 'file_name',
            direction: 'asc'
          }
      ],
      moreParams: {}
    }
  },
  methods: {
    allcap (value) {
      return value.toUpperCase()
    },
    approvalLabel (value) {
      return value === 'M'
              ? '<span class="tag is-primary is-medium"><span class="icon"><i class="fa fa-mars"></i></span>&nbsp;Male</span>'
              : '<span class="tag is-danger is-medium"><span class="icon"><i class="fa fa-venus"></i></span>&nbsp;Female</span>'
    },
    editableName (value) {

//      var input = jq('<input />', {
//        'type': 'text',
//        'style': 'width:200px',
//        'class': 'saveFileName',
//        'data-id': jq(this).data('id'),
//        'data-original': jq(this).text(),
//        'value': jq(this).text()
//      });
//      jq(this).replaceWith(input);

      return '<div class="view">' +
                '<label @dblclick="editName(value)">'+value+'</label>'+
              '</div>'+
              '<input class="edit" type="text" v-model="value" @keyup.enter="doneEditName(value)" @keyup.esc="cancelEditValue(value)">';
    },
    genderLabel (value) {
      return value === 'M'
        ? '<span class="tag is-primary is-medium"><span class="icon"><i class="fa fa-mars"></i></span>&nbsp;Male</span>'
        : '<span class="tag is-danger is-medium"><span class="icon"><i class="fa fa-venus"></i></span>&nbsp;Female</span>'
    },
    formatNumber (value) {
      return accounting.formatNumber(value, 2)
    },
    formatDate (value, fmt = 'D MMM YYYY') {
      return (value == null)
        ? ''
        : moment(value, 'YYYY-MM-DD').format(fmt)
    },
    formatSize (fileSize, numDigits = 2) {
      if (typeof fileSize === 'undefined') {
        fileSize = 0;
      }

      var kilobyte = 1024;
      var megabyte = 1024 * kilobyte;
      var gigabyte = 1024 * megabyte;
      var metrics = [gigabyte, megabyte, kilobyte];
      var correspond = ['GB', 'MB', 'KB'];
      var size;
      for (var i = 0; i < metrics.length; i++) {
        if (fileSize >= metrics[i]) {
          size = fileSize / metrics[i];
          size = size.toFixed(numDigits);
          size += ' ' + correspond[i];
          return size;
        }
      }
      return '-';
    },
    onPaginationData (paginationData) {
      this.$refs.pagination.setPaginationData(paginationData)
      this.$refs.paginationInfo.setPaginationData(paginationData)
    },
    onChangePage (page) {
      this.$refs.vuetable.changePage(page)
    },
    onCellClicked (data, field, event) {
        console.log('cellClicked: ', field.name)
        //this.$refs.vuetable.toggleDetailRow(data.id)
    },
    onFilterSet (filterText) {
      this.moreParams = {
        'filter': filterText
      }

      Vue.nextTick( () => this.$refs.vuetable.refresh())
    },
    onTriggerDeleted () {
      this.$refs.vuetable.triggerDeleted()
      //Vue.nextTick( () => this.$refs.vuetable.refresh())
    },
    onFilterReset () {
      this.moreParams = {}
      this.$refs.vuetable.refresh()
      Vue.nextTick( () => this.$refs.vuetable.refresh())
    }
  },
  mounted() {
    this.$events.listen('filter-set', filterText => this.onFilterSet(filterText))
    this.$events.listen('filter-reset', this.onFilterSet)
    this.$events.listen('trigger-deleted', this.onTriggerDeleted)
  },
  beforeDestroy() {
    this.$events.remove('filter-set')
    this.$events.remove('filter-reset')
    this.$events.remove('trigger-deleted')
  }
}
</script>


<style>
  table.vuetable {
    font-size:12px;
  }
  table.vuetable td, table.vuetable tr {
     vertical-align:middle;
   }

</style>