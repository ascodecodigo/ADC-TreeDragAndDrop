<template>
    <div>
        <h2>Max 2 niveles</h2>
        <!-- <input type="number" v-model="maxLevel" /> -->
        <DraggableTree :data="response" @drag="ondrag" crossTree="crossTree" draggable="draggable" ref="tree1">
            <div slot-scope="{data, store}">
                <b @click="store.toggleOpen(data)" v-if="data.children &amp;&amp; data.children.length">{{data.open ? '-' : '+'}}&nbsp;</b>
                <span>{{data.text}}</span>
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
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
