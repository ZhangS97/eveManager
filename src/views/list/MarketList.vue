<template>
  <page-header-wrapper>

    <a-card
      style="margin-top: 24px"
      :bordered="false"
      title="标准列表">

      <a-tree :load-data="onLoadData" :tree-data="treeData" />

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
      ]
    }
  },
  created () {

  },
  methods: {
    onLoadData (treeNode) {
      return new Promise(resolve => {
        if (treeNode.dataRef.children) {
          resolve()
          return
        }
        treeNode.dataRef.children = [
          { title: 'Child Node', key: `${treeNode.eventKey}-0` },
          { title: 'Child Node', key: `${treeNode.eventKey}-1` }
        ]
        this.treeData = [...this.treeData]
        resolve()
      })
    }
  }
}
</script>
