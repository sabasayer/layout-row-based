<template>
    <div id="app">
        <h4>SABASAYER layout-row-based</h4>
        <layout-container :rows="rows" :layout-items.sync="items" edit-mode />
    </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import LayoutContainerComponent from "@/components/LayoutContainer.vue";
import { LayoutItem, LayoutRow } from "../types/lib/layout-row";

@Component({
    components: {
        "layout-container": LayoutContainerComponent
    }
})
export default class App extends Vue {
    rows: LayoutRow[] = [
        { row: 1, columns: 2 },
        { row: 2, columns: 3 },
        { row: 3, columns: 1 }
    ];

    items: LayoutItem[] = [
        { row: 2, column: 2, order: 1, id: 1 },
        { row: 1, column: 1, order: 1, id: 2 }
    ];
}
</script>

<style>
#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
