# Expanded Rows

## Usage 

To enable row expansion, simply: 
1. Add `expandedOptions` to the table component
2. Use the `expanded-row` slot

## Full example
```html
<template>
    <vue-good-table
        :expandedOptions="{
            key: 'id',
            expandedRows: [2],
            styleClass: 'my-custom-class'
        }"
        :rows="rows"
        :columns="columns">
        <template slot="expanded-row" slot-scope="props">
            <div>
                <span class="warning"></span>
                Are you sure you want to delete
                <span style="font-weight: bold;">{{props.row.name}}</span>?
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
</template>

<script>
export default {
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
        this.expandedRows = this.expandedRows.filter(v => v !== id)
      } else {
        this.expandedRows.push(clickedId)
      }
    }
  }
}
</script>

<style>
tr.my-custom-class {
  background-color: rgb(229, 240, 255); // blue
}
</style>

```

### Result
<expanded-rows />

::: tip Slot-scope props
You have access to the following fields:
* The original row object can be accessed via `props.row`
* The currently displayed table row index can be accessed via `props.index` . 
* The original row index can be accessed via `props.row.originalIndex`. You can then access the original row object by using `rows[props.row.originalIndex]`.
* You can access the formatted row data (for example - formatted date) via `props.formattedRow`
:::

## expandedOptions
* key: `String` or `Function`. If you pass a function, you will be passed the row as the first parameter as such:
```js
// using a function allows you to supply a custom field 
// (like a nested key or something more complex)
expandedOptions: {
    key: function(row) {
        return row.nested.field
    }
}
```
* expandedRows: `Array` of ids for the currently expanded rows
* styleClass: `String`. Style class for the `<tr>` tag of the expanded row. Useful for setting a prominent background color, fixed height or different scroll behaviour. Beware of css selector specificity when styling the row:
```css
tr.my-custom-class {
  background-color: rgb(229, 240, 255); // blue
}
/* won't work */
.my-custom-class {
  background-color: rgb(229, 240, 255); // blue
}
```

