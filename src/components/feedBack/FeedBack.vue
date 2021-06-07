<template>
  <div>
    <el-card class="box-card">
      <div slot="header" class="head-text">
        <span>病例反馈</span>
      </div>
      <el-table
          :default-sort="{prop: 'createTime'}"
          :data="caseHistoryList"
          border
          style="width: 100%">
        <el-table-column type="expand">
          <template slot-scope="props">
            <el-card>
              <div slot="header" class="head-text">
                <span>病例详情</span>
              </div>
              <el-form label-position="left" border class="demo-table-expand">
                <el-row>
                  <el-col :span="12">
                    <el-form-item label="科室: ">
                      <el-tag type="success">{{ props.row.cliName }}</el-tag>
                    </el-form-item>
                  </el-col>
                  <el-col :span="12">
                    <el-form-item label="医生姓名: ">
                      <el-tag type="success">{{ props.row.staffName }}</el-tag>
                    </el-form-item>
                  </el-col>
                </el-row>
                <el-row>
                  <el-col :span="12">
                    <el-form-item label="病症详情: ">
                      <el-input
                          type="textarea"
                          disabled
                          :rows="3"
                          style="width: 50%"
                          v-model="props.row.userIllness">
                      </el-input>
                    </el-form-item>
                  </el-col>
                </el-row>
                <el-row>
                  <el-col :span="24">
                    <el-form-item label="药品列表: ">
                      <el-table
                          width="100%"
                          :border="true"
                          :data="props.row.medicListJson">
                        <el-table-column
                            prop="medicName"
                            label="药品名称"
                            width="180">
                        </el-table-column>
                        <el-table-column
                            prop="medicNum"
                            label="药品数量(个)"
                            width="180">
                        </el-table-column>
                        <el-table-column
                            prop="medicPrice"
                            label="药品价格(元)">
                        </el-table-column>
                      </el-table>
                    </el-form-item>
                  </el-col>
                </el-row>
                <el-row>
                  <el-col :span="12">
                    <el-form-item label="诊断反馈: ">
                      <el-input
                          type="textarea"
                          disabled
                          :rows="3"
                          style="width: 50%"
                          v-model="props.row.feedBack">
                      </el-input>
                    </el-form-item>
                  </el-col>
                </el-row>
              </el-form>
            </el-card>
          </template>
        </el-table-column>
        <el-table-column
            sortable
            prop="createTime"
            label="日期"
            width="400">
        </el-table-column>
        <el-table-column
            prop="caseId"
            label="病例编号"
            width="400">
        </el-table-column>
        <el-table-column
            prop="userName"
            label="患者姓名"
            width="400">
        </el-table-column>
        <el-table-column
            sortable
            prop="totalPrice"
            label="总价格(元)"
            width="300">
        </el-table-column>
      </el-table>
    </el-card>

  </div>
</template>

<script>
import {request} from "@/network/request";

export default {
  name: "FeedBack",
  data() {
    return {
      //从病例查询
      caseHistoryList: [],

    }
  },
  activated() {
    this.getCaseHistoryList()
  },
  methods: {
    handleClick() {
      this.getCaseHistoryList()
    },

    //获取病例单缴费记录
    getCaseHistoryList() {
      let staffId = this.$store.state.staffId
      request({
        url: '/home/case/history?staffId=' + staffId,
        method: 'get',
        headers: {
          'Content-Type': 'application/json'
        }
      }).then(res => {
        let resData = res.data;
        console.log(resData);
        if (resData.status === 200) {
          this.caseHistoryList = resData.result.data;
        }
      })
    },

  }
}
</script>

<style scoped>
.box-card {
  margin: 0px;
}
</style>