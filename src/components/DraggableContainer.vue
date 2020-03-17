<template>
    <div class="dragcontainer">
        <ul v-if="!draggableVisible" class="my-list nopointer">
            <li v-for="(item, index) in myData" :class="{ ghost: draggedIndex === index }" :style="{ order: followReorder ? myOrder.indexOf(index) : 0}">{{ item }}</li>
        </ul>
        <draggable tag="ul" :id="id" handle=".handle" :list="filteredData" :group="{ name: 'myData' }" swapThreshold="0.75"
                   :animation="250" ghost-class="ghost" class="my-list" :move="event => $emit('move', event)" @start="$emit('start', $event)" @end="$emit('end', $event)"
                   :style="{ opacity: draggableVisible ? 1 : 0 }" @change="$emit('change', $event)" :forceFallback="true">
            <li class="handle" v-for="item in filteredData" :id="`${id}_${item}`" :key="`${id}_${item}`" >{{ item }}</li>
        </draggable>
    </div>
</template>

<script lang="ts">
    import {Vue} from "vue-property-decorator";
    import draggable from 'vuedraggable';

    export default Vue.extend({
        components: { draggable },
        props: {
            id: { type: String, required: true },
            myData: { type: Array as () => Array<String>, required: true },
            myOrder: { type: Array as () => Array<Number>, required: true },
            followReorder: { type: Boolean, default: false },
            draggableVisible: { type: Boolean, required: true },
            draggedIndex: { type: Number, required: true, default: -1 },
            dragOverIndex: { type: Number, required: true, default: -1 },
            targetDraggable: { type: Boolean, required: true }
        },
        computed: {
            filteredData: function() : String[] {
                if (this.draggedIndex < 0 || this.dragOverIndex < 0 || !this.targetDraggable) {
                    return this.myData;
                } else {
                    return [...this.myData.slice(0, this.draggedIndex), ...this.myData.slice(this.draggedIndex + 1)];
                }
            }
        }
    });
    /*@Component({
        name: 'draggable-container',
        components: { draggable }
    })
    export default class DraggableContainer extends Vue {
        @Prop({
            required: true
        })
        id!: string;

        @Prop({
            required: true,
            type: Array
        })
        myData!: string[];

        @Prop({
            required: true,
            type: Array
        })
        myOrder!: number[];

        @Prop({
            default: false
        })
        followReorder!: boolean;

        @Prop({
            required: true
        })
        draggableVisible!: boolean;

        @Prop({
            required: true,
            default: -1
        })
        draggedIndex!: number;

        @Prop({
            required: true,
            default: -1
        })
        dragOverIndex!: number;

        @Prop({
            required: true
        })
        targetDraggable!: boolean;

        get filteredData() {
            if (this.draggedIndex < 0 || this.dragOverIndex < 0 || !this.targetDraggable) {
                return this.myData;
            } else {
                return [...this.myData.slice(0, this.draggedIndex), ...this.myData.slice(this.draggedIndex + 1)];
            }
        }
    }*/

</script>

<style scoped>
    .dragcontainer {
        position: relative;
    }
    ul {
        list-style: none;
    }
    ul.my-list {
        position: absolute;
    }
    ul.my-list.nopointer {
        pointer-events: none
    }
    li {
        list-style-type: none;
        cursor: default;
    }
</style>