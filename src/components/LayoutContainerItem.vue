<template>
    <div v-if="item" class="layout-container__item" :data-id="item.id">
        <slot> </slot>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";
import { LayoutItem } from "../lib/layout-row";

@Component
export default class LayoutContainerItemComponent extends Vue {
    @Prop() readonly item!: LayoutItem;
}
</script>

<style scoped>
.layout-container__item + .layout-container__item{
    margin-top: 8px;
}
</style>
