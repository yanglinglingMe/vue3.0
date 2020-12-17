<template>
  <div>
    <el-table
      :data="tableData"
      border
      style="width: 100%">
      <el-table-column
        fixed
        prop="date"
        label="日期1"
        width="150">
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="120">
      </el-table-column>
      <el-table-column
        prop="province"
        label="省份"
        width="120">
      </el-table-column>
      <el-table-column
        prop="city"
        label="市区"
        width="120">
      </el-table-column>
      <el-table-column
        prop="address"
        label="地址">
      </el-table-column>
      <el-table-column
        prop="zip"
        label="邮编"
        width="120">
      </el-table-column>
      <el-table-column
        fixed="right"
        label="操作"
        width="100">
        <template #default="scope">
          <el-dropdown trigger="click" @command="(command) => handleExeDetail(command,scope.row)">
            <span class="el-link--primary" style="cursor: pointer;">执行明细</span>
            <template #dropdown>
              <el-dropdown-menu>
                <el-dropdown-item command="apply" titles="查看所有申请">查看所有申请</el-dropdown-item>
                <el-dropdown-item command="purc">查看所有采购</el-dropdown-item>
                <el-dropdown-item command="loan">查看所有借款</el-dropdown-item>
                <el-dropdown-item command="reim">查看所有报销</el-dropdown-item>
                <el-dropdown-item command="cont">查看所有合同</el-dropdown-item>
              </el-dropdown-menu>
            </template>
          </el-dropdown>
        </template>
      </el-table-column>
    </el-table>
    <!--申请报销-->
    <el-dialog :title="title" v-model="applyReim" width="70%">
      <el-table :data="cData" border>
        <el-table-column type="index" label="序号" align="center" width="50">
        </el-table-column>
        <el-table-column property="transDate" label="支出日期">
        </el-table-column>
        <el-table-column property="expenseTypeName" label="费用类型">
          <template #default="scope">
            <span v-show="scope.row.billType === 'other'">{{scope.row.expenseTypeName}}</span>
            <span v-show="scope.row.billType !== 'other'">{{scope.row.billTypeName}}</span>
          </template>
        </el-table-column>
        <el-table-column property="checkAmt" align="right" label="金额">
          <template #default="scope">
            {{scope.row.checkAmt}}
          </template>
        </el-table-column>
        <el-table-column property="creatorName" label="经办人">
        </el-table-column>
        <el-table-column property="billNo" width="200" label="支出单号">
          <template #default="scope">
            <span class="bs-link-primary" @click="handleLinkTo(scope.row)">{{scope.row.billNo}}</span>
          </template>
        </el-table-column>
        <el-table-column property="billStatus" label="审批状态">
        </el-table-column>
        <el-table-column property="reason" label="摘要" show-overflow-tooltip>
        </el-table-column>
      </el-table>
    </el-dialog>
    <!--采购、合同、借款-->
    <el-dialog :title="title" v-model="dialogTableVisible"  width="70%">
      <el-table :data="cData">
        <el-table-column type="index" label="序号" align="center" width="50">
        </el-table-column>
        <el-table-column v-for="(e,i) in cTitle" :key="'loan' + i" :property="e.code" :label="e.name">
          <template #default="scope">
            <div v-if="e.code === 'billNo'">
              <span class="bs-link-primary" @click="handleLinkTo(scope.row)">{{scope.row.billNo}}</span>
            </div>
            <div v-if="e.code !== 'billNo'">
              <span v-if="e.type === 'amt'">{{scope.row[e.code]}}</span>
              <span v-if="e.type !== 'amt'">{{scope.row[e.code]}}</span>
            </div>
          </template>
        </el-table-column>
      </el-table>
    </el-dialog>
  </div>
</template>
<script>
import { reactive, toRefs } from 'vue'
export default {
  setup () {
    const state = reactive({
      cData: [
        {
          transDate: '2016-05-04',
          expenseTypeName: 'ces',
          billType: 'other',
          checkAmt: 100,
          creatorName: 'cesr',
          billNo: 1001,
          billStatus: '已审批',
          reason: '我要测试',
          billTypeName: '侧耳',
          agyName: '9002',
          departmentName: '饼干部门',
          nextAuditor: 'know',
          backAmt: 80,
          modeName: 'mode',
          typeName: '类型',
          purOrganization: '组织',
          contractName: '合同',
          usedAmt: 1000,
          billProgress: '30%',
          billCopeWith: 180,
          billUnpaid: 200
        },
        {
          transDate: '2016-09-05',
          expenseTypeName: 'ces1',
          billType: 'othe1r',
          checkAmt: 102,
          creatorName: 'cesr',
          billNo: 1002,
          billStatus: '未审批',
          reason: '我要测试啊',
          billTypeName: '侧耳',
          agyName: '9002',
          departmentName: '饼干部门',
          nextAuditor: 'know',
          backAmt: 80,
          modeName: 'mode',
          typeName: '类型',
          purOrganization: '组织',
          contractName: '合同',
          usedAmt: 1000,
          billProgress: '30%',
          billCopeWith: 180,
          billUnpaid: 200
        }
      ],
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1518 弄',
        zip: 200333
      }, {
        date: '2016-05-04',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1517 弄',
        zip: 200333
      }, {
        date: '2016-05-01',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1519 弄',
        zip: 200333
      }, {
        date: '2016-05-03',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1516 弄',
        zip: 200333
      }],
      applyReim: false,
      dialogTableVisible: false,
      cTitle: [],
      loanTitle: [
        { name: '单据编号', code: 'billNo', type: '' },
        { name: '借款日期', code: 'transDate', type: '' },
        { name: '单据类型', code: 'billTypeName', type: '' },
        { name: '单位', code: 'agyName', type: '' },
        { name: '申请人', code: 'creatorName', type: '' },
        { name: '部门', code: 'departmentName', type: '' },
        { name: '审批状态', code: 'billStatus', type: '' },
        { name: '待审批岗位', code: 'nextAuditor', type: '' },
        { name: '借款金额', code: 'checkAmt', type: 'amt' },
        { name: '已还款金额', code: 'backAmt', type: 'amt' },
        { name: '借款事由', code: 'reason', type: '' }
      ],
      purcTitle: [
        { name: '采购计划号', code: 'billNo', type: '' },
        { name: '申请日期', code: 'transDate', type: '' },
        { name: '采购方式', code: 'modeName', type: '' },
        { name: '采购类型', code: 'typeName', type: '' },
        { name: '采购组织形式', code: 'purOrganization', type: '' },
        { name: '单位', code: 'agyName', type: '' },
        { name: '申请人', code: 'creatorName', type: '' },
        { name: '部门', code: 'departmentName', type: '' },
        { name: '金额', code: 'checkAmt', type: 'amt' },
        { name: '事由', code: 'reason', type: '' }
      ],
      contTitle: [
        { name: '合同编号', code: 'billNo', type: '' },
        { name: '合同名称', code: 'contractName', type: '' },
        { name: '合同大类', code: 'billTypeName', type: '' },
        { name: '经办部门', code: 'departmentName', type: '' },
        { name: '合同状态', code: 'billStatus', type: '' },
        { name: '合同金额', code: 'checkAmt', type: 'amt' },
        { name: '已执行金额', code: 'usedAmt', type: 'amt' },
        { name: '合同支付进度', code: 'billProgress', type: '' },
        { name: '当年总应付(收)', code: 'billCopeWith', type: '' },
        { name: '当年应付(收)未付(收)', code: 'billUnpaid', type: '' }
      ],
      innerTable: {
        apply: [],
        purc: [],
        loan: [],
        reim: [],
        cont: []
      },
      title: '',
      titleC: ''
    })
    const handleLinkTo = () => {
      console.log('disnji')
    }
    const handleExeDetail = (command, row) => {
      if (command === 'apply') {
        state.title = '申请'
      } else if (command === 'purc') {
        state.title = '采购'
      } else if (command === 'loan') {
        state.title = '借款'
      } else if (command === 'reim') {
        state.title = '报销'
      } else if (command === 'cont') {
        state.title = '合同'
      }
      state.tableDatas = state.innerTable[command]
      if (command === 'apply' || command === 'reim') {
        state.applyReim = true
      } else {
        state.dialogTableVisible = true
        state.cTitle = state[command + 'Title']
      }
    }
    return {
      ...toRefs(state),
      handleLinkTo,
      handleExeDetail
    }
  }
}
</script>
<style lang="scss" scoped>

</style>
