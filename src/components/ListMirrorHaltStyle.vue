<template>
    <div id="main" class="main">
        <div class="draggables">
            <div class="dragcontainer" @mouseover="mouseOver" @mouseout="mouseOut" :class="{ 'no-drop': bannedForDrop === 'draggable1_list1' }">
                <draggable
                        id="draggable1_list1"
                        tag="ul"
                        class="list-group"
                        ghost-class="ghost"
                        chosen-class="chosen"
                        drag-class="drag"
                        :list="list1"
                        group="shared"
                        :forceFallback="true"
                        :animation="250"
                        :move="onMove" @start="startDrag" @end="endDrag" @change="onChange">
                    <li v-for="element in list1" :id="`1_${element.id}`" class="list-group-item">
                        {{ element.title }}
                    </li>
                </draggable>
            </div>
            <div class="dragcontainer" @mouseover="mouseOver" @mouseout="mouseOut" :class="{ 'no-drop': bannedForDrop === 'draggable2_list1' }">
                <draggable
                        id="draggable2_list1"
                        tag="ul"
                        class="list-group"
                        ghost-class="ghost"
                        chosen-class="chosen"
                        drag-class="drag"
                        :list="list1"
                        group="shared"
                        :forceFallback="true"
                        :animation="250"
                        :move="onMove" @start="startDrag" @end="endDrag" @change="onChange">
                    <li v-for="element in list1" :id="`2_${element.id}`" class="list-group-item">
                        {{ element.title }}
                    </li>
                </draggable>
            </div>
            <div class="dragcontainer" @mouseover="mouseOver" @mouseout="mouseOut" :class="{ 'no-drop': bannedForDrop === 'draggable3_list2' }">
                <draggable
                        id="draggable3_list2"
                        tag="ul"
                        class="list-group"
                        ghost-class="ghost"
                        chosen-class="chosen"
                        drag-class="drag"
                        :list="list2"
                        group="shared"
                        :forceFallback="true"
                        :animation="250"
                        :move="onMove" @start="startDrag" @end="endDrag" @change="onChange">
                    <li v-for="element in list2" :id="`3_${element.id}`" class="list-group-item">
                        {{ element.title }}
                    </li>
                </draggable>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    import draggable from 'vuedraggable';

    @Component({
        name: 'list-mirror-halt-style',
        components: { draggable }
    })
    export default class ListMirrorHaltStyle extends Vue {

        private list1 = [
            { title: 'News', id: 0 },
            { title: 'Documentation', id: 1 },
            { title: 'Forum', id: 2 },
            { title: 'Community', id: 3 },
            { title: 'About Us', id: 4}];
        private list2 = [
            { title: 'Archives', id: 5 },
            { title: 'Participation', id: 6 },
            { title: 'Meetings', id: 7 },
            { title: 'Schedule', id: 8 },
            { title: 'Internal', id: 9 },
            { title: 'Messages', id: 10 }
        ];
        private draggedIndex = -1;
        private draggedId = '';
        private initiatedDrag = '';
        private bannedForDrop = '';

        startDrag(event: any) {
            this.draggedId = event.item.id.substring(2);
            this.draggedIndex = event.oldIndex;
            this.initiatedDrag = event.target.id;
            this.bannedForDrop = '';
            if (this.initiatedDrag === 'draggable1_list1') {
                this.bannedForDrop = 'draggable2_list1';
            } else if (this.initiatedDrag === 'draggable2_list1') {
                this.bannedForDrop = 'draggable1_list1';
            }
            document.querySelector('.main')!.classList.add('dragging');
        }

        endDrag() {
            this.draggedIndex = -1;
            this.draggedId = '';
            this.initiatedDrag = '';
            this.bannedForDrop = '';

            document.querySelectorAll('.list-group-item').forEach((el: Element) => {
                (el as HTMLElement).classList.remove('duplicate');
                (el as HTMLElement).style.display = 'block';
            });
            document.querySelector('.main')!.classList.remove('dragging');
        }

        mouseOver(event: any) {
            if (!this.initiatedDrag) { return; }
            let overElement;
            if (event.target.classList.contains('list-group-item')) {
                overElement = event.target.parentElement;
            } else if (event.target.classList.contains('list-group')) {
                overElement = event.target;
            } else if (event.target.classList.contains('dragcontainer')) {
                overElement = event.target.children[0];
            }
            if (overElement.id === this.bannedForDrop) {
                document.querySelector('.main')!.classList.add('not-allowed');
                return;
            }
            document.querySelector('.main')!.classList.remove('not-allowed');
        }

        mouseOut(event: any) {
            if (!this.initiatedDrag) { return; }
            document.querySelector('.main')!.classList.remove('not-allowed');
        }

        onMove(event: any) {
           return event.related.parentElement.id !== this.bannedForDrop;
        }

        onChange(event: any) {
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
    .main.dragging.not-allowed, .main.dragging.not-allowed * {
        cursor: not-allowed;
    }
    .dragcontainer {
        width: 240px;
        height: fit-content;
    }
    .dragcontainer.no-drop {
        opacity: 0.3;
    }
    .draggables {
        width: 800px;
        display: flex;
        flex-direction: row;
        justify-content: space-evenly;
    }
    .list-group {
        list-style: none;
    }
    .list-group-item {
        width: 100%;
        background: #e0e0e0;
        margin-bottom: 10px;
        padding: 6px 8px;
        border: 1px solid hsl(225, 55%, 80%);
        list-style-type: none;
        cursor: default;
    }
    .list-group-item.duplicate {
        background: #e6e6e6;
        color: #aaa;
        border: 1px dotted hsl(225, 15%, 80%);
    }
    .list-group-item.ghost {
        background: rgba(0, 0, 0, 0.1);
        color: transparent;
        border: 1px dotted hsla(200, 50%, 50%, 0.5);
    }
    .list-group-item.drag {
        border: 1px solid #999;
    }
</style>
