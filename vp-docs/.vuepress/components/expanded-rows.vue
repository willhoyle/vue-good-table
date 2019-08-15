<template>
  <div>
    <vue-good-table
      :expandedOptions="{
        key: 'id',
        expandedRows,
        styleClass: 'my-custom-class'
      }"
      :rows="rows"
      :columns="columns"
    >
      <template slot="expanded-row" slot-scope="props">
        <div>
          <span class="warning"></span>
          Are you sure you want to delete
          <span
            style="font-weight: bold;"
          >{{props.row.name}}</span>?
          <a class="nav-link">Confirm</a>
        </div>
      </template>
      <template slot="table-row" slot-scope="props">
        <span v-if="props.column.field == 'custom'">
          <span @click="clicked(props.row.id)" class="trash"></span>
        </span>
        <span v-else>{{props.formattedRow[props.column.field]}}</span>
      </template>
    </vue-good-table>
  </div>
</template>

<script>
export default {
  name: 'expanded-rows',
  data() {
    return {
      expandedRows: [2],
      columns: [
        { label: 'ID', field: 'id' },
        { label: 'Name', field: 'name' },
        { label: 'Age', field: 'age', type: 'number' },
        { label: '', field: 'custom' }
      ],
      rows: [
        { id: 1, name: 'John', age: 20 },
        { id: 2, name: 'Jane', age: 24 },
        { id: 3, name: 'Susan', age: 16 }
      ]
    }
  },
  methods: {
    clicked(clickedId) {
      if (this.expandedRows.some(id => id == clickedId)) {
        this.remove(clickedId)
      } else {
        this.expandedRows.push(clickedId)
      }
    },
    remove(id) {
      this.expandedRows = this.expandedRows.filter(v => v !== id)
    }
  }
}
</script>

<style>
table {
  display: table;
}
tr.my-custom-class {
  background-color: rgb(229, 240, 255);
}
.trash {
  display: inline-block;
  text-indent: -9999px;
  width: 24px;
  height: 24px;
  background: url(/vue-good-table/bin.svg);
  background-size: 24px 24px;
  cursor: pointer;
}
.warning {
  display: inline-block;
  text-indent: -9999px;
  width: 20px;
  height: 20px;
  background: url(/vue-good-table/warning.svg);
  background-size: 20px 20px;
}
</style>
