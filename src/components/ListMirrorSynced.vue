<template>
    <div id="main" class="main">
        <div class="draggables">
            <draggable-container id="draggable1" :my-data="myData"
                                 :draggable-visible="draggable1Visible" :dragged-index="draggedIndex" :drag-over-index="dragOverIndex"
                                 :target-draggable="targetDraggable1"
                                 :follow-reorder="true" :my-order="myOrder"
                                 @start="startDrag" @end="endDrag" @move="onMove" @change="onChange">
            </draggable-container>
            <draggable-container id="draggable2" :my-data="myData"
                                 :draggable-visible="draggable2Visible" :dragged-index="draggedIndex" :drag-over-index="dragOverIndex"
                                 :target-draggable="targetDraggable2" :other-target-draggable="targetDraggable1"
                                 :follow-reorder="true" :my-order="myOrder"
                                 @start="startDrag" @end="endDrag" @move="onMove" @change="onChange">
            </draggable-container>
        </div>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    import DraggableContainer from '@/components/DraggableContainer.vue';

    const myList = ['News', 'Documentation', 'Forum', 'Community', 'About Us'];
    @Component({
        name: 'list-mirror-synced',
        components: { DraggableContainer }
    })
    export default class ListMirrorSynced extends Vue {

        private myData: string[] = [...myList];
        private myOrder: number[] = [];
        private draggedIndex: number = -1;
        private dragOverIndex: number = -1;
        private initiatedDrag: string = '';

        private targetDraggable1: boolean = false;
        private targetDraggable2: boolean = false;

        get draggable1Visible() {
            if (this.initiatedDrag === 'draggable2') {
                return this.targetDraggable1;
            } else if (this.initiatedDrag === 'draggable1') {
                return !this.targetDraggable2;
            } else {
                return false;
            }
        }

        get draggable2Visible() {
            if (this.initiatedDrag === 'draggable1') {
                return this.targetDraggable2;
            } else if (this.initiatedDrag === 'draggable2') {
                return !this.targetDraggable1;
            } else {
                return false;
            }
        }

        startDrag(event: any) {
            this.initiatedDrag = event.target.id;
            this.draggedIndex = event.oldIndex;
            document.querySelector('.main')!.classList.add('dragging');
        }

        endDrag() {
            this.draggedIndex = -1;
            this.dragOverIndex = -1;
            this.initiatedDrag = '';
            this.targetDraggable1 = false;
            this.targetDraggable2 = false;
            document.querySelector('.main')!.classList.remove('dragging');
        }

        onMove(event: any) {
            this.draggedIndex = event.draggedContext.index;
            let overTargetDraggable;

            if (this.initiatedDrag === 'draggable1') {
                overTargetDraggable = this.targetDraggable2 = (event.related.parentElement.id === 'draggable2');
            } else if (this.initiatedDrag === 'draggable2') {
                overTargetDraggable = this.targetDraggable1 = (event.related.parentElement.id === 'draggable1');
            }
            if (!overTargetDraggable) {
                this.dragOverIndex = event.draggedContext.futureIndex;
            } else {
                this.dragOverIndex = event.willInsertAfter ? event.relatedContext.index + 1 : event.relatedContext.index;
            }
            const newIndices = [...Array(this.myData.length).keys()];
            newIndices.splice(this.draggedIndex, 1);
            newIndices.splice(this.dragOverIndex, 0, this.draggedIndex);
            this.myOrder = newIndices;
        }

        onChange(event: any) {
            if (event.added) {
                const myData = this.myData.concat([]);
                myData.splice(this.myOrder.indexOf(this.draggedIndex), 0, event.added.element);
                this.myData = myData;
            }
            this.myOrder = [...Array(this.myData.length).keys()];
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .main {
        width: 100vw;
        height: 100%;
        display: flex;
        justify-content: center;
    }
    .main.dragging, .main.dragging * {
        cursor: move;
        cursor: grabbing;
    }

    .draggables {
        width: 580px;
        display: flex;
        flex-direction: row;
        justify-content: space-evenly;
    }
    .dragcontainer {
        width: 240px;
    }
    >>> ul {
        display: flex;
        flex-direction: column;
        margin: 0;
        padding: 4px;
    }
    >>> li {
        width: 240px;
        background: #e0e0e0;
        padding: 6px 8px;
        margin-bottom: 10px;
        border: 1px solid hsl(225, 55%, 80%);
    }
    >>> li.ghost {
        color: #999;
        background: #fff;
    }

</style>
