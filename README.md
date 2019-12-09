# layout-row-based

Row based dynamic layout creater. 

Rows can be divided to columns and can contain any number of items.

Items can be moved from column to column and sorted inside columns by drag and drop.

For drag and drop used [shopify/draggable](https://github.com/Shopify/draggable).


## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Example Usage

```
 <layout-container
        :rows="rows"
        @update:rows="updateRows"
        :layout-items.sync="items"
        @update:layoutItems="updateLayout"
        edit-mode
        sortable
        >
            <template #default="{id}">
                <div style="padding:40px;background:pink;">id : {{id}} , order :{{getItem(id).order}}</div>
            </template>
</layout-container>

```

### Compiles and minifies for production
```
yarn build
```

### Run your unit tests
```
yarn test:unit
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
