<template>
  <div>
    <!--    面包屑导航区-->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item>排班日程</el-breadcrumb-item>
    </el-breadcrumb>
    <!--    卡片视图-->
    <el-card>
      <!--      添加角色按钮区-->
      <el-row>
        <el-col>
          <el-button type="primary" @click="addDialogVisible = true">添加职工</el-button>
        </el-col>
      </el-row>

      <!--      角色列表区域-->
      <el-table :data="staffInfoList" strip>
        <!--        添加索引列-->
        <el-table-column type="index" label="序列" width="70px"></el-table-column>
        <el-table-column label="员工编号" prop="staffId"></el-table-column>
        <el-table-column label="员工名称" prop="staffName" width="80px"></el-table-column>
        <el-table-column label="性别" prop="staffSex" width="70px"></el-table-column>
        <el-table-column label="联系方式" prop="staffTel"></el-table-column>
        <el-table-column label="员工职称" prop="staffPos" width="60px"></el-table-column>
        <el-table-column label="入职时间" prop="staffEntry"></el-table-column>
        <el-table-column label="操作" width="210px">
          <template slot-scope="scope">
            <el-button type="primary" size="mini" icon="el-icon-edit" @click="showEditDialog(scope.$index)">编辑信息
            </el-button>
            <el-button type="danger" size="mini" icon="el-icon-delete" @click="deleteStaff(scope.$index)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>

      <!--    添加职工对话框-->
      <el-dialog
          title="添加职员信息"
          :visible.sync="addDialogVisible"
          width="50%"
          :label-position="'left'">
        <!--      内容主体区-->
        <el-form :model="addStaffInfoForm"
                 :inline="true"
                 :rules="staffFormRules"
                 ref="addStaffInfoRef">
          <el-row :grunt="24">
            <el-col :span="12">
              <el-form-item label="职员编号" prop="staffId">
                <el-input v-model="addStaffInfoForm.staffId"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="职员名称" prop="staffName">
                <el-input v-model="addStaffInfoForm.staffName"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :grunt="24">
            <el-col :span="12">
              <el-form-item label="职员性别" prop="staffSex">
                <el-radio-group v-model="addStaffInfoForm.staffSex">
                  <el-radio-button label="男"></el-radio-button>
                  <el-radio-button label="女"></el-radio-button>
                </el-radio-group>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="联系方式" prop="staffTel">
                <el-input v-model="addStaffInfoForm.staffTel"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :grunt="24">
            <el-col :span="12">
              <el-form-item label="职员职位" prop="staffPos">
                <el-input v-model="addStaffInfoForm.staffPos"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="入职时间" prop="staffEntry">
                <el-date-picker
                    v-model="addStaffInfoForm.staffEntry"
                    format="yyyy-MM-d"
                    value-format="yyyy-MM-dd"
                    placeholder="选择时间">
                </el-date-picker>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :grunt="24">
            <el-col :span="12">
              <el-form-item label="密码">
                <el-input v-model="addStaffInfoForm.password" type="password" style="width: 100%"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="介绍">
                <el-input v-model="addStaffInfoForm.describe" type="textarea" style="width: 100%"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <!--      底部区域-->
        <span slot="footer">
          <el-button @click="cancelAdd">取 消</el-button>
          <el-button type="primary" @click="addStaff">确 定</el-button>
        </span>
      </el-dialog>

      <!--      修改职员界面-->
      <el-dialog
          title="修改职员信息"
          :visible.sync="editDialogVisible"
          width="50%">

        <el-form :model="editStaffInfoForm"
                 :inline="true"
                 :rules="staffFormRules"
                 ref="editStaffInfoRef">

          <el-form-item label="职员编号" prop="staffId">
            <el-input :disabled="true" v-model="editStaffInfoForm.staffId"></el-input>
          </el-form-item>

          <el-form-item label="职员名称" prop="staffName">
            <el-input v-model="editStaffInfoForm.staffName"></el-input>
          </el-form-item>
          <br>

          <!--            性别选择器代码-->
          <el-form-item label="职员性别" prop="staffSex">
            <el-radio-group v-model="editStaffInfoForm.staffSex">
              <el-radio-button label="男"></el-radio-button>
              <el-radio-button label="女"></el-radio-button>
            </el-radio-group>
          </el-form-item>
          <br>


          <el-form-item label="联系方式" prop="staffTel">
            <el-input v-model="editStaffInfoForm.staffTel"></el-input>
          </el-form-item>
          <el-form-item label="职员职位" prop="staffPos">
            <el-input v-model="editStaffInfoForm.staffPos"></el-input>
          </el-form-item>


          <!--            日历选择器代码-->
          <el-form-item label="入职时间" prop="staffEntry">
            <el-date-picker
                v-model="editStaffInfoForm.staffEntry"
                format="yyyy-MM-d"
                value-format="yyyy-MM-dd"
                placeholder="选择时间">
            </el-date-picker>


          </el-form-item>
          <el-form-item label="介绍">
            <el-input v-model="editStaffInfoForm.describe" type="textarea" style="width: 100%"></el-input>
          </el-form-item>
        </el-form>

        <span slot="footer" class="dialog-footer">
          <el-button @click="cancelEdit">取 消</el-button>

          <!--            修改职员信息对话框确定按钮-->
          <el-button type="primary" @click="upDateEdit">确 定</el-button>
        </span>
      </el-dialog>
    </el-card>


  </div>
</template>

<script>
import {request} from "../../network/request";

export default {
  name: "Staff",
  data() {
    //验证职员电话
    let checkTel = (rule, value, callback) => {
      const mobileReg = /^1[3|4|5|7|8][0-9]{9}$/;
      if (mobileReg.test(value)) {
        return callback();
      }
      callback(new Error("请输入正确的号"))
    };
    return {
      staffInfoList: [],
      //添加职员控制变量
      addDialogVisible: false,

      //修改职员控制变量
      editDialogVisible: false,
      //添加数据的表单对象
      addStaffInfoForm: {
        staffId: '',
        staffName: '',
        password: '',
        staffSex: '男',
        staffPos: '',
        staffTel: '',
        staffEntry: '',
        describe: ''
      },

      //给 修改数据的表单对象 初始值
      editStaffInfoForm: {
        staffId: '',
        staffName: '',
        staffSex: '',
        staffPos: '',
        staffTel: '',
        staffEntry: '',
        describe: ''
      },

      //添加表单的验证规则对象
      staffFormRules: {
        staffId: [
          {required: true, message: '请输入职员编号', trigger: 'blur'},
          {min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur'}
        ],
        staffName: [
          {required: true, message: '请输入职员名称', trigger: 'blur'},
          {min: 2, max: 6, message: '长度在 2 到 6 个字符', trigger: 'blur'}
        ],
        staffSex: [
          {required: true, message: '选择职员性别', trigger: 'blur'},
        ],
        staffTel: [
          {required: true, message: '请输入号码', trigger: 'blur'},
          {validator: checkTel, trigger: 'blur'}
        ],
        staffPos: [
          {required: true, message: '请输入职位', trigger: 'blur'},
        ],
        staffEntry: [
          {required: true, message: '请选择时间', trigger: 'blur'},
        ]
      }
    }
  },

  //页面创建，获取初始化数据
  activated() {
    console.log("职员组件初始化");
    this.initStaffInfoList()
  },

  //存放所有调用的函数
  methods: {

    initStaffInfoList() {
      request({
        url: '/home/staff/staffListInit'
      }).then(responseData => {
        let data = responseData.data;
        if (data.status === 200) {
          this.staffInfoList = data.result.data;
          console.log(this.staffInfoList);
          this.$message({
            type: 'success',
            message: '职员信息初始化成功!'
          });
        } else {
          this.$message({
            type: 'error',
            message: '职员信息初始化失败!'
          });
        }
      }).catch(err => {
        console.log(err);
        this.$message({
          type: 'error',
          message: '因网络波动,操作失败!'
        });
      })
    },



    //取消添加职员
    cancelAdd() {
      this.$confirm('确认取消添加操作?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$refs.addStaffInfoRef.resetFields();
        this.addDialogVisible = false;
        this.$message({
          type: 'success',
          message: '操作成功!'
        });
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消'
        });
      });
    },


    //添加职员
    addStaff() {
      let staffInfo = JSON.stringify(this.addStaffInfoForm);
      console.log(staffInfo);
      request({
        url: '/home/staff/addStaff',
        method: 'post',
        data: {
          staffInfo: staffInfo
        }
      }).then(responseData => {
        let data = responseData.data;
        if (data.status === 200) {
          this.$message({
            type: 'success',
            message: '职员添加成功!'
          });
          this.initStaffInfoList()
          this.emptyAddForm()
        } else {
          this.$message({
            type: 'error',
            message: '职员添加失败!'
          });
        }
      }).catch(err => {
        console.log(err);
        this.$message({
          type: 'error',
          message: '因网络波动,操作失败!'
        });
      })
    },

    //展示编辑职员的对话框
    showEditDialog(index) {
      this.editDialogVisible = true;

      //讲想要修改的数据(多个属性)赋值给editStaffInfoForm对象
      this.editStaffInfoForm = this.staffInfoList[index];
      console.log(this.editStaffInfoForm)
    },

    emptyAddForm() {
      this.addDialogVisible = false;
      this.addStaffInfoForm.staffId = '';
      this.addStaffInfoForm.staffName = '';
      this.addStaffInfoForm.staffSex = '男';
      this.addStaffInfoForm.staffPos = '';
      this.addStaffInfoForm.staffTel = '';
      this.addStaffInfoForm.staffEntry = '';
      this.addStaffInfoForm.describe = '';
      this.addStaffInfoForm.password = '';
    },


    //更新数据库职员信息代码
    upDateEdit() {

      //将前端对象转化成json字符串
      let editStaff = JSON.stringify(this.editStaffInfoForm);
      request({
        url: '/home/staff/editStaff',
        method: 'post',
        data: {
          editStaff: editStaff
        }
      }).then(responseData => {
        let data = responseData.data;
        if (data.status === 200) {
          this.$message({
            type: 'success',
            message: '职员修改成功!'
          });
          this.editDialogVisible = false;
        } else {
          this.$message({
            type: 'error',
            message: '职员修改失败!'
          });
        }
        this.initStaffInfoList()
      }).catch(err => {
        this.$message({
          type: 'error',
          message: '因网络波动,操作失败!'
        });
      })
    },


    deleteStaff(index) {
      this.$confirm('确认取消修改职员操作?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        let staffId = this.staffInfoList[index].staffId;
        console.log(staffId);
        request({
          url: '/home/staff/deleteStaffById',
          method: 'post',
          data: {
            staffId: staffId
          }
        }).then(responseData => {
          let data = responseData.data;
          if (data.status === 200) {
            this.staffInfoList.splice(index, 1);
            this.$message({
              type: 'success',
              message: '职员删除成功!'
            });
          }
        }).catch(err => {
          this.$message({
            type: 'error',
            message: '因网络波动,操作失败!'
          });
        });
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消'
        });
      });
    },

    //取消职员编辑
    cancelEdit() {
      this.$confirm('确认取消修改职员操作?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$refs.editStaffInfoRef.resetFields();
        this.editDialogVisible = false;
        this.$message({
          type: 'success',
          message: '操作成功!'
        });
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消'
        });
      });
    }
  }
}
</script>

<style scoped>

</style>
