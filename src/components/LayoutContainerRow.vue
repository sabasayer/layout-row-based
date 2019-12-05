<template>
    <div class="layout-container__row">
        <div
            v-for="col in row.columns"
            :key="col"
            class="layout-container__cell"
            :class="{ 'edit-mode': editMode }"
            :data-row="row.row"
            :data-col="col"
        >
            <slot :col="col"></slot>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";
import { LayoutRow } from "../../types/lib/layout-row";

@Component({})
export default class LayoutContainerRowComponent extends Vue {
    @Prop() readonly row!: LayoutRow;
    @Prop({ type: Boolean, default: false }) readonly editMode!: boolean;
}
</script>

<style scoped>
.layout-container__row {
    display: flex;
}

.layout-container__row + .layout-container__row {
    margin-top: 8px;
}

.layout-container__cell {
    flex: 1;
    flex-shrink: 0;
    background-color: aliceblue;
}

.layout-container__cell + .layout-container__cell {
    margin-left: 8px;
}

.layout-container__cell.edit-mode {
    padding: 10px;
}
</style>
