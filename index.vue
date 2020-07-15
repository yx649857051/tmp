<template>
  <div>
    <el-table
      ref="multipleTable"
      :data="tableData"
      tooltip-effect="dark"
      style="width: 100%"
      @select="handleSelect"
      @select-all="handleSelectAll"
    >
      <el-table-column
        type="selection"
        width="55"
      />
      <el-table-column
        label="菜单"
        width="120"
      >
        <template slot-scope="scope">{{ scope.row.menu }}</template>
      </el-table-column>
      <el-table-column
        prop="name"
        label="通知配置"
        width="200"
      >
        <template slot-scope="scope">
          <div class="a">
            <li v-for="item in scope.row.typeList" :key="item">
              <el-checkbox v-model="item.checked" @change="bool => handleChange(bool, scope.row)">{{ item.label }}</el-checkbox>
            </li>
          </div>
        </template>
      </el-table-column>
    </el-table>
    <el-button @click="handleClick">点击</el-button>
  </div>

</template>

<script>
export default {
  data() {
    return {
      tableData: [
        { menu: '菜单1', typeList: [
          { label: '审核1', name: 'flag1', checked: true },
          { label: '审核2', name: 'flag2', checked: true }]
        },
        { menu: '菜单2', typeList: [
          { label: '审核1', name: 'flag3', checked: true },
          { label: '审核2', name: 'flag4', checked: false },
          { label: '审核3', name: 'flag5', checked: true }
        ]
        }
      ]
    }
  },
  mounted() {
    this.tableData.forEach(item => {
      const arr = item.typeList.filter(v => !v.checked)
      if (!(arr && arr.length)) {
        this.$refs.multipleTable.toggleRowSelection(item)
      }
    })
  },
  methods: {
    handleChange(bool, row) {
      if (!bool) {
        this.$refs.multipleTable.toggleRowSelection(row, false)
        return
      }
      const arr = row.typeList.filter(v => !v.checked)
      if (!(arr && arr.length)) {
        this.$refs.multipleTable.toggleRowSelection(row)
      }
    },
    handleSelect(selection, row) {
      const newRow = selection.find(item => item.menu === row.menu)
      row.typeList.forEach(item => {
        item.checked = !!newRow
      })
    },
    handleSelectAll(selection) {
      this.tableData.forEach(item => {
        item.typeList.forEach(typeItem => {
          typeItem.checked = !!(selection && selection.length)
        })
      })
    },
    handleClick() {
      const obj = {}
      this.tableData.forEach(v => {
        v.typeList.forEach(cv => {
          obj[cv.name] = cv.checked
        })
      })
      console.log(obj)
    }
  }
}
</script>

<style scoped>
 .a{
    display: flex;
   flex-wrap: wrap;
  }
</style>
