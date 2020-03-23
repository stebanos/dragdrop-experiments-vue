<template>
    <div id="main" class="main">
        <div class="draggables">
            <draggable
                    id="draggable1"
                    tag="ul"
                    class="list-group"
                    ghost-class="ghost"
                    chosen-class="chosen"
                    drag-class="drag"
                    :list="list1"
                    :group="{ name: 'shared', pull: 'clone' }"
                    :forceFallback="true"
                    :animation="250"
                    :removeCloneOnHide="false"
                    :move="onMove" @start="startDrag" @end="endDrag" @change="onChange">
                <li v-for="element in list1" :id="`1_${element.id}`" class="list-group-item">
                    {{ element.title }}
                </li>
            </draggable>
            <draggable
                    id="draggable2"
                    tag="ul"
                    class="list-group"
                    ghost-class="ghost"
                    chosen-class="chosen"
                    drag-class="drag"
                    :list="list1"
                    :group="{ name: 'shared', pull: 'clone' }"
                    :forceFallback="true"
                    :animation="250"
                    :removeCloneOnHide="false"
                    :move="onMove" @start="startDrag" @end="endDrag" @change="onChange">
                <li v-for="element in list1" :id="`2_${element.id}`" class="list-group-item">
                    {{ element.title }}
                </li>
            </draggable>
            <draggable
                    id="draggable3"
                    tag="ul"
                    class="list-group"
                    ghost-class="ghost"
                    chosen-class="chosen"
                    drag-class="drag"
                    :list="list2"
                    :group="{ name: 'shared', pull: 'clone' }"
                    :forceFallback="true"
                    :animation="250"
                    :removeCloneOnHide="false"
                    :move="onMove" @start="startDrag" @end="endDrag" @change="onChange">
                <li v-for="element in list2" :id="`3_${element.id}`" class="list-group-item">
                    {{ element.title }}
                </li>
            </draggable>
        </div>
        <div class="notice">This example is unfinished. Incorrect behaviour occurs when moving items from the 3rd list to the 1st or 2nd list. Nothing is removed from the 3d list and items in the other lists sometimes get replaced.</div>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    import draggable from 'vuedraggable';


    // Note: draggables have removeCloneOnHide set to false. This makes it so that the styling of the 'cloned' element gets set immediately when moving to a different list.
    // Although it still doesn't seem to work all of the time. Further investigation needed.
    @Component({
        name: 'list-mirror-initial-and-unrelated',
        components: { draggable }
    })
    export default class ListMirrorInitialAndUnrelated extends Vue {

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

        startDrag(event: any) {
            this.draggedId = event.item.id.substring(2);
            this.draggedIndex = event.oldIndex;
            this.initiatedDrag = event.target.id;
            document.querySelector('.main')!.classList.add('dragging');
        }

        endDrag() {
            this.draggedIndex = -1;
            this.draggedId = '';
            this.initiatedDrag = '';
            document.querySelectorAll('.list-group-item').forEach((el: Element) => {
                (el as HTMLElement).classList.remove('duplicate');
                (el as HTMLElement).style.display = 'block';
            });
            document.querySelector('.main')!.classList.remove('dragging');
        }

        onMove(event: any) {
            const lists = document.querySelectorAll('.list-group');
            const list1 = lists[0].querySelectorAll(`.list-group-item[id$='_${this.draggedId}']`);
            const list2 = lists[1].querySelectorAll(`.list-group-item[id$='_${this.draggedId}']`);
            const list3 = lists[2].querySelectorAll(`.list-group-item[id$='_${this.draggedId}']`);

/*            console.log('');
            list1.forEach((el, index) => {
                console.log(`%clist1 %c${index} %c${el.id.substring(2)}: ${el.innerText} %c${el.classList.value} %c${el.getAttribute('draggable') !== null ? el.getAttribute('draggable') : ''}`, 'color:darkgreen', 'color:blue', 'background:darkgreen;color:white', 'color:black', 'background:black;color:white;');
            });
            console.log('');
            list2.forEach((el, index) => {
                console.log(`%clist2 %c${index} %c${el.id.substring(2)}: ${el.innerText}  %c${el.classList.value} %c${el.getAttribute('draggable') !== null ? el.getAttribute('draggable') : ''}`, 'color:darkblue', 'color:blue', 'background:darkblue;color:white', 'color:black', 'background:black;color:white;');
            }); */

            // This needs cleaning up.
            if (event.related.parentElement.id === 'draggable3') {
                if (this.initiatedDrag === 'draggable3') { return true; }
                else {
                    const xel1 = [...list1].filter(el => [...el.classList].indexOf('sortable-fallback') === -1)[0] as HTMLElement;
                    const xel2 = [...list2].filter(el => [...el.classList].indexOf('sortable-fallback') === -1)[0] as HTMLElement;
                    xel1.classList.add('duplicate');
                    xel2.classList.add('duplicate');
                    xel1.style.display = 'block';
                    xel2.style.display = 'block';
                    return true;
                }
            } else if (this.initiatedDrag === 'draggable3') {
                const xel3 = [...list3].filter(el => [...el.classList].indexOf('sortable-fallback') === -1)[0] as HTMLElement;
                xel3.classList.add('duplicate');
                return true;
            }

            const initiatedByDraggable1 = this.initiatedDrag === 'draggable1';
            const list =  initiatedByDraggable1 ? [...list1] : [...list2];
            const otherList = initiatedByDraggable1 ? [...list2] : [...list1];
            const targettingOtherList = event.related.parentElement.id === (initiatedByDraggable1 ? 'draggable2' : 'draggable1');
            const el = list.filter(el => [...el.classList].indexOf('sortable-fallback') === -1)[0] as HTMLElement;
            const otherEl = otherList.filter(el => [...el.classList].indexOf('chosen') === -1)[0] as HTMLElement;
            if (el === undefined || otherEl === undefined) { return true; }

            if (targettingOtherList) {
                el.classList.add('duplicate');
                otherEl.style.display = 'none';
                otherEl.classList.remove('duplicate');
            } else {
                el.classList.remove('duplicate');
                otherEl.style.display = 'block';
                otherEl.classList.add('duplicate');
            }
            return true;
        }

        onChange(event: any) {
            if (event.added) {
                if (this.draggedIndex > event.added.newIndex) {
                    this.list1.splice(this.draggedIndex + 1, 1);
                } else {
                    this.list1.splice(this.draggedIndex, 1);
                }
            }
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
    .main.dragging * {
        cursor: move;
        cursor: grabbing;
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
        width: 240px;
        background: #e0e0e0;
        margin-bottom: 10px;
        padding: 6px 8px;
        border: 1px solid hsl(225, 55%, 80%);
        list-style-type: none;
        cursor: default;
    }
    .list-group-item.ghost {
        background: rgba(0, 0, 0, 0.1);
        color: transparent;
        border: 1px dotted hsla(200, 50%, 50%, 0.5);
    }
    .list-group-item.drag {
        border: 1px solid #999;
    }
    .list-group-item.duplicate {
        background: #e6e6e6;
        color: #aaa;
        border: 1px dotted hsl(225, 15%, 80%);
    }
    .notice {
        position: absolute;
        top: 70px;
        max-width: 75%;
        color: orangered;
    }
    @media only screen and (max-width: 600px) {
        .list-group-item {
            width: 110px;
            font-size: 0.8em;
        }
        .draggables {
            width: 380px;
        }
        .notice {
            top: unset;
            bottom: 24px;
        }
    }
</style>
