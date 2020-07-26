<template>
    <div>
        <h2>Max 2 niveles</h2>
        <!-- <input type="number" v-model="maxLevel" /> -->
        <DraggableTree :data="response" @drag="ondrag" crossTree="crossTree" draggable="draggable" ref="tree1">
            <div class="draggable-content" slot-scope="{data, store}">
                <button @click="store.toggleOpen(data)" v-if="data.children &amp;&amp; data.children.length">{{data.open ? '-' : '+'}}&nbsp;</button>
                <span class="draggable-label">{{data.text}}</span>
            </div>
        </DraggableTree>
    </div>
</template>
<script>
import { DraggableTree } from 'vue-draggable-nested-tree'
import * as th from 'tree-helper'

export default {
    components: { DraggableTree },
    data() {
        return {
            data: null,
            // niveles por defecto
            maxLevel: 2,
            response: [
                { text: 'node 1' },
                { text: 'node 2' },
                { text: 'node 3' },
                { text: 'node 4' },
                { text: 'node 5' },
                { text: 'node 6' },
                { text: 'node 7' },
                { text: 'node 8' },
                { text: 'node 9' },
            ],
        }
    },

    methods: {
        ondrag(node) {
            const { maxLevel } = this
            let nodeLevels = 1
            th.depthFirstSearch(node, (childNode) => {
                if (childNode._vm.level > nodeLevels) {
                    nodeLevels = childNode._vm.level
                }
            })
            nodeLevels = nodeLevels - node._vm.level + 1
            const childNodeMaxLevel = maxLevel - nodeLevels
            th.depthFirstSearch(this.response, (childNode) => {
                if (childNode === node) {
                    return 'skip children'
                }
                if (!childNode._vm) {
                    console.log(childNode)
                }
                this.$set(childNode, 'droppable', childNode._vm.level <= childNodeMaxLevel)
            })
        },
    },
}
</script>

<style>
/* Estilos de contenedor */
.he-tree {
    /* border: 1px solid #ccc;
    padding: 20px; */
    width: 300px;
}

/* Contenedor de los nodos */
.tree-node-inner {
    border: 1px solid #1e6c99;
    cursor: pointer;
    padding-bottom: 16px;
    padding-left: 16px;
    padding-right: 16px;
    padding-top: 16px;
}

/* Backgroud de animacion cuando se mueve el dragg */
.draggable-placeholder {
    background: #f4f4f4;
}

/* Border de dragg al moverlo */
.draggable-placeholder-inner {
    align-items: center;
    background: rgba(0, 136, 249, 0.09);
    border: 1px dashed #0088f8;
    box-sizing: border-box;
    color: #0088f9;
    display: flex;
    padding: 0;
    text-align: center;
}

.draggable-content {
    display: flex;
}

.draggable-content button {
    margin-right: 10px;
}

.draggable-label {
    display: block;
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    font-weight: 500;
    text-transform: capitalize;
}
</style>
