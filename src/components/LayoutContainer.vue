<template>
    <div ref="container" class="layout-container">
        <div v-if="editMode" class="layout-container__settings">
            <v-button v-if="cloneRows.length > 0" @click="removeRow" icon="minus" />
            <v-button @click="addRow" icon="plus" />
        </div>
        <layout-container-row
            v-for="(row, i) in cloneRows"
            :key="i+'_'+key"
            :row="row"
            @update:row="onRowUpdated(...arguments,i)"
            :edit-mode="editMode"
        >
            <template #default="{col}">
                <slot name="col-before" :col="col" :row="row"></slot>
                <slot name="col" :col="col" :row="row">
                    <layout-container-item
                        v-for="(item, j) in getItems(row.row, col)"
                        :item="item"
                        :key="j"
                    >
                        <slot :id="item.id"></slot>
                    </layout-container-item>
                </slot>
                <slot name="col-after" :col="col" :row="row"></slot>
            </template>
        </layout-container-row>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop, Watch } from "vue-property-decorator";
import { LayoutItem, LayoutRow } from "../lib/layout-row";
import cloneDeep from "lodash/cloneDeep";
import LayoutContainerRow from "./LayoutContainerRow.vue";
import { Draggable, Droppable } from "@shopify/draggable";
import { DragMoveEvent, DroppableDroppedEvent } from "../lib/shopify-events";
import LayoutContainerItem from "./LayoutContainerItem.vue";
import ButtonComponent from "./button.vue";

@Component({
    components: {
        LayoutContainerItem,
        LayoutContainerRow,
        "v-button": ButtonComponent
    }
})
export default class LayoutContainerComponent extends Vue {
    @Prop() readonly layoutItems!: LayoutItem[];
    @Prop() readonly rows!: LayoutRow[];
    @Prop({ type: Boolean, default: false }) readonly editMode!: boolean;

    cloneRows: LayoutRow[] = [];
    cloneLayoutItems: LayoutItem[] = [];
    key: number = 0;

    get getItems() {
        return (row: number, col: number) => {
            return this.layoutItems
                ? this.layoutItems.filter(e => e.row == row && e.column == col)
                : undefined;
        };
    }

    @Watch("rows", { immediate: true, deep: true })
    onRowsChanged() {
        if (this.rows) {
            this.cloneRows = cloneDeep(this.rows);
        } else this.cloneRows = [];
    }

    @Watch("layoutItems", { immediate: true, deep: true })
    onLayoutItemsChanged() {
        if (this.layoutItems)
            this.cloneLayoutItems = cloneDeep(this.layoutItems);
        else this.cloneLayoutItems = [];
    }

    mounted() {
        if (this.editMode) {
            this.initDragEvents();
        }
    }

    updateLayoutItems() {
        this.$emit("update:layoutItems", this.cloneLayoutItems);
    }

    initDragEvents() {
        const droppable = new Droppable(this.$refs.container, {
            draggable: ".layout-container__item",
            dropzone: ".layout-container__cell",
            mirror: {
                constrainDimensions: true
            }
        });

        droppable.on("droppable:dropped", this.onDropped);
        droppable.on("droppable:returned", this.onDropped);
    }

    onDropped(e: DroppableDroppedEvent) {
        let dropArea = e.data.dropzone;
        let dragItem = e.data.dragEvent.data.originalSource;

        let areaRow = +(
            dropArea.attributes.getNamedItem("data-row")?.value ?? 0
        );
        let areaCol = +(
            dropArea.attributes.getNamedItem("data-col")?.value ?? 0
        );

        let dragItemId = dragItem.attributes.getNamedItem("data-id")?.value;

        let item = this.cloneLayoutItems.find(e => e.id == dragItemId);

        if (!item) return;

        let oldRow = item.row;
        let oldCol = item.column;

        if (areaRow != oldRow) item.row = +areaRow;
        if (areaCol != oldCol) item.column = +areaCol;

        if (areaRow != oldRow && areaCol != oldCol) {
            this.key++;
            this.updateLayoutItems();
        }
    }

    onRowUpdated(row: LayoutRow, i: number) {
        this.$set(this.cloneRows, i, row);
        this.$emit("update:rows", this.cloneRows);
    }

    addRow(){
        this.cloneRows.push({
            row:this.cloneRows.length+1,
            columns:1
        })
    }

    removeRow(){
        this.cloneRows.splice(this.cloneRows.length-1,1);
    }   
}
</script>

<style scoped>
.layout-container {
    display: block;
}
.layout-container__settings{
    padding: 4px;
    display: flex;
    justify-content: flex-end;
}
</style>
<style>
.draggable-mirror {
    opacity: 0.7;
}
</style>
