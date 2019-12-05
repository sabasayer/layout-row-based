<template>
    <div ref="container" class="layout-container">
        <layout-container-row
            v-for="(row, i) in rows"
            :key="i+'_'+key"
            :row="row"
            :edit-mode="editMode"
        >
            <template #default="{col}">
                <layout-container-item
                    v-for="(item, j) in getItems(row.row, col)"
                    :item="item"
                    :key="j"
                >
                    <slot :id="item.id">
                        row : {{ row.row }}, col : {{ col }}
                    </slot>
                </layout-container-item>
            </template>
        </layout-container-row>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop, Watch } from "vue-property-decorator";
import { LayoutItem, LayoutRow } from "../../types/lib/layout-row";
import cloneDeep from "lodash/cloneDeep";
import LayoutContainerRow from "@/components/LayoutContainerRow.vue";
import { Draggable, Droppable } from "@shopify/draggable";
import {
    DragMoveEvent,
    DroppableDroppedEvent
} from "../../types/lib/shopify-events";
import LayoutContainerItem from "@/components/LayoutContainerItem.vue";

@Component({
    components: { LayoutContainerItem, LayoutContainerRow }
})
export default class LayoutContainerComponent extends Vue {
    @Prop() readonly layoutItems!: LayoutItem[];
    @Prop() readonly rows!: LayoutRow[];
    @Prop({ type: Boolean, default: false }) readonly editMode!: boolean;

    cloneLayoutItems: LayoutItem[] = [];
    key:number = 0;

    get getItems() {
        return (row: number, col: number) => {
            return this.layoutItems
                ? this.layoutItems.filter(
                      e => e.row == row && e.column == col
                  )
                : undefined;
        };
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
        this.$emit("update:layoutItems",this.cloneLayoutItems);
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

        let areaRow = +(dropArea.attributes.getNamedItem("data-row")?.value ?? 0);
        let areaCol = +(dropArea.attributes.getNamedItem("data-col")?.value ?? 0);

        let dragItemId = dragItem.attributes.getNamedItem("data-id")?.value;

        let item = this.cloneLayoutItems.find(e => e.id == dragItemId);

        if (!item) return;

        let oldRow = item.row;
        let oldCol = item.column


        if (areaRow != oldRow) item.row = +areaRow;
        if (areaCol != oldCol) item.column = +areaCol;


        if(areaRow != oldRow && areaCol != oldCol)
        {
            this.key++;
            this.updateLayoutItems()
        }

        console.log("droppable:dropped", areaRow, areaRow, dragItemId, item);
    }
}
</script>

<style scoped>
.layout-container {
    display: block;
    background-color: gray;
}
</style>
<style>
.draggable-mirror {
    opacity: 0.7;
}
.bg-pink {
    background-color: pink;
}
</style>
