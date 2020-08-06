<template>
  <div class="app-container">
    <el-table :data="tableData" style="width: 100%;">
      <el-table-column prop="cusCompanyName" label="对方公司" width="180" />
      <el-table-column prop="ourCompanyName" label="我方公司" width="180" />
      <el-table-column prop="reason" label="备注" />
    </el-table>
  </div>
</template>

<script>
import data from './data.json'
export default {
  data() {
    return {
      tableData: [],
    }
  },
  mounted() {
    // 临时收集结果对象
    const resultObj = {}
    // 时间年月收集key
    // const timeKey = 'timeObjArr'
    data.list.forEach((item) => {
      // 两个id 联合起来作为唯一key, 保证不会重复
      const key = `${item.cusCompanyId}_${item.ourCompanyId}`
      const date = new Date(item.settlementPeriod)
      const timeArr = [date.getFullYear(), date.getMonth() + 1]
      if (resultObj[key]) {
        if (
          resultObj[key]['timeObjArr'] &&
          resultObj[key]['timeObjArr'][`${timeArr[0]}`]
        ) {
          // 追加值 { timeObjArr: { 2020: ['02', '03'] } }
          resultObj[key]['timeObjArr'][`${timeArr[0]}`].push(timeArr[1])
          // 这里可能会有重复的月份加进来，去重
          resultObj[key]['timeObjArr'][`${timeArr[0]}`] = [
            ...new Set(resultObj[key]['timeObjArr'][`${timeArr[0]}`]),
          ]
        } else {
          // 追加新属性2021 { timeObjArr: { 2021: ['02'] } }
          resultObj[key]['timeObjArr'][`${timeArr[0]}`] = [timeArr[1]]
        }
        resultObj[key].amount += item.amount
        resultObj[key].idList.push({ id: item.id })
      } else {
        // 这里 resultObj[key] 为空， 所以不能和上面的else一样直接访问属性timeKey，会报错，
        // 所以分两步 { timeObj: { 2020: ['02'] } }
        // item[timeKey] = { [`${timeArr[0]}`]: [timeArr[1]] }
        resultObj[key] = {
          ...item,
          timeObjArr: { [`${timeArr[0]}`]: [timeArr[1]] },
          reason: '分成款',
          idList: [{ id: item.id }],
        }
      }
    })
    console.log(resultObj)
    // 收集的对象转成集合， 去掉属性保留value
    const newList = Object.keys(resultObj).map((key) => resultObj[key])
    console.log(newList)
    this.tableData = newList
  },
  methods: {},
}
</script>

<style scoped>

</style>

//================================= 数据 ==========================================
{
  "list":[
    {
      "id": "yx1",
      "cusCompanyId": "1000",
      "ourCompanyId": "2000",
      "cusCompanyName": "百度科技",
      "ourCompanyName": "欧珀科技",
      "settlementPeriod": 1596668255000,
      "amount": 3000
    },
    {
      "id": "yx2",
      "cusCompanyId": "1000",
      "ourCompanyId": "2000",
      "cusCompanyName": "百度科技",
      "ourCompanyName": "欧珀科技",
      "settlementPeriod": 1599346655000,
      "amount": 3000
    },
    {
      "id": "yx3",
      "cusCompanyId": "1000",
      "ourCompanyId": "2001",
      "cusCompanyName": "百度科技",
      "ourCompanyName": "欧珀科技1",
      "settlementPeriod": 1599346655000,
      "amount": 3000
    },
    {
      "id": "yx4",
      "cusCompanyId": "1001",
      "ourCompanyId": "2000",
      "cusCompanyName": "阿里科技",
      "ourCompanyName": "欧珀科技1",
      "settlementPeriod": 1599346655000,
      "amount": 3000
    },
    {
      "id": "yx5",
      "cusCompanyId": "1001",
      "ourCompanyId": "2000",
      "cusCompanyName": "阿里科技",
      "ourCompanyName": "欧珀科技1",
      "settlementPeriod": 1599346655000,
      "amount": 3000
    },
    {
      "id": "yx6",
      "cusCompanyId": "1001",
      "ourCompanyId": "2000",
      "cusCompanyName": "阿里科技",
      "ourCompanyName": "欧珀科技1",
      "settlementPeriod": 1604617055000,
      "amount": 3000
    }
  ]
}
