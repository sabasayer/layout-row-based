<template >
    <div class="layout-container__icon" :style="style">
        <svg v-if="icon == 'gear'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 25.495 25.5"><path d="M24.745 14.194V11.3l-3.625-.657a8.556 8.556 0 00-.961-2.333l2.095-3.023-2.046-2.046-3 2.085a8.793 8.793 0 00-2.36-.993h-.006L14.196.751h-2.893l-.64 3.544a8.509 8.509 0 00-2.442.982L5.29 3.242 3.244 5.288l2.013 2.9a8.471 8.471 0 00-1.053 2.492l-3.452.624v2.893l3.442.624a8.351 8.351 0 001.047 2.513l-2 2.882 2.046 2.041 2.893-2.009a8.434 8.434 0 002.486 1l.635 3.5h2.893l.641-3.538a8.742 8.742 0 002.4-1.015l2.969 2.057 2.046-2.041-2.079-3a8.766 8.766 0 00.961-2.366zM12.669 16.56a3.791 3.791 0 113.788-3.788 3.793 3.793 0 01-3.788 3.788z" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"/></svg>
        <svg v-else-if="icon == 'plus'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 21.5 21.5">
            <g fill="none" stroke="currentColor" stroke-linecap="round" stroke-width="1.5">
                <path d="M10.752.75v20M20.75 10.751h-20" />
            </g>
        </svg>
        <svg v-else-if="icon == 'minus'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 13.5 1.5">
            <path
                d="M12.75.75h-12"
                fill="none"
                stroke="currentColor"
                stroke-linecap="round"
                stroke-width="1.5"
            />
        </svg>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";
import { LayoutItem } from "../lib/layout-row";

@Component
export default class IconComponent extends Vue {
    @Prop() readonly icon!: string;
    @Prop({type:String,default:'black'}) readonly color!: string;

    get style(){
        return {color:this.color}
    }
}
</script>

<style scoped>
.layout-container__icon{
    height: 12px;
    width: 12px;
    display: inline-flex;
    align-items: center;
}
.layout-container__icon svg{
    stroke:inherit;
    width: inherit;
    height: inherit;
}
</style>
