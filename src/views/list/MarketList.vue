<template>
  <page-header-wrapper>

    <a-card
      style="margin-top: 24px"
      :bordered="false"
      title="标准列表">

      <a-tree
        :load-data="onLoadData"
        :tree-data="treeData"
        :expanded-keys="expandedKeys"
        :auto-expand-parent="true"
        @select="onSelect" />

    </a-card>
  </page-header-wrapper>
</template>
<script>
export default {
  data () {
    return {
      treeData: [
        { title: 'Expand to load', key: '0' },
        { title: 'Expand to load', key: '1' },
        { title: 'Tree Node', key: '2', isLeaf: true }
      ],
      expandedKeys: []
    }
  },
  methods: {
    onSelect (selectedKeys, info) {
      console.log('onSelect', selectedKeys, info)
      if (selectedKeys.length > 0) {
        selectedKeys.forEach(function (item) {
          console.log(item)
          this.expandedKeys.push(item)
        }.bind(this))
      } else {
        for (let i = 0; i < this.expandedKeys.length; i++) {
          const item = this.expandedKeys[i]
          if (item === info.node.eventKey) {
            console.log('item === info.node.eventKey', info.node.eventKey)
            this.expandedKeys.slice(i, 1)
            console.log(item)
            console.log(this.expandedKeys)
          }
        }
      }

      console.log(this.expandedKeys)
      this.selectedKeys = selectedKeys
    },
    onExpand (expandedKeys) {
      console.log('onExpand', expandedKeys)
      // if not set autoExpandParent to false, if children expanded, parent can not collapse.
      // or, you can remove all expanded children keys.
      this.expandedKeys = expandedKeys
      this.autoExpandParent = false
    },
    onLoadData (treeNode) {
      console.log(treeNode)
      console.log(treeNode.dataRef.children)
      return new Promise(resolve => {
        if (treeNode.dataRef.children) {
          resolve()
          return
        }
        setTimeout(() => {
          treeNode.dataRef.children = [
            { title: 'Child Node', key: `${treeNode.eventKey}-0` },
            { title: 'Child Node', key: `${treeNode.eventKey}-1` }
          ]
          this.treeData = [...this.treeData]
          resolve()
        }, 1000)
      })
    }
  }
}
</script>
