<template>
  <el-container style="height: 100%; border: 1px solid #eee">
    <el-container>

      <el-header style="text-align: right; font-size: 12px">
        <el-dropdown>
          <i class="el-icon-setting" style="margin-right: 15px"></i>
          <el-dropdown-menu slot="dropdown">
            <span>王小虎</span>
            <el-dropdown-item>退出</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>

      </el-header>

      <el-main>
        <div style="padding: 10px 0">
          <el-button type="primary" @click="handleAdd">新增</el-button>
        </div>
        <el-table :data="tableData" border stripe :heade-row-name="headerBg" style="width: 100%">
          <el-table-column prop="username" label="姓名" width="200"></el-table-column>
          <el-table-column prop="address" label="地址" width="400"></el-table-column>
          <el-table-column label="操作" width="600" align="center">
            <template slot-scope="scope">
              <el-button type="success" @click="handleEdit(scope.row)">编辑<i class="el-icon-edit"></i></el-button>
              <el-popconfirm
                  class="ml-5"
                  confirm-button-text='确定'
                  cancel-button-text='取消'
                  icon="el-icon-info"
                  icon-color="red"
                  title="确定删除？"
                  @confirm="del(scope.row.id)"
              >
                <el-button type="danger" slot="reference">删除<i class="el-icon-remove-outline"></i></el-button>
              </el-popconfirm>
            </template>
          </el-table-column>
        </el-table>

        <div style="padding: 10px 0">
          <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="pageNum"
              :page-sizes="[2, 4, 5, 6]"
              :page-size="pageSize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="total">
          </el-pagination>
        </div>

        <el-dialog title="用户信息" :visible.sync="dialogFormVisible" width="30%">
          <el-form label-width="88px" size="small">
            <el-form-item label="用户名">
              <el-input v-model="form.username" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="地址">
              <el-input v-model="form.address" autocomplete="off"></el-input>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary" @click="save">确 定</el-button>
          </div>
        </el-dialog>

      </el-main>
    </el-container>
  </el-container>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  data() {
    return {
      tableData: [],
      total: 0,
      msg: "hell yi",
      headerBg: 'headerBg',
      pageNum: 1,
      pageSize: 5,
      username: "",
      address: "",
      form: {},
      dialogFormVisible: false,
    }
  },

  created() {
    this.load()
  },

  methods: {
    load() {
      this.request.get("/user/page", {
        params: {
          pageNum: this.pageNum,
          pageSize: this.pageSize,
          username: this.username,
          address: this.address
        }
      }).then(res => {
        console.log(res)
        this.tableData = res.records
        this.total = res.total
      })
    },
    save() {
      this.request.post("/user", this.form).then(res => {
        if (res) {
          this.$message.success("保存成功")
          this.dialogFormVisible = false
          this.load()
        } else {
          this.$message.error("保存失败")
        }
      })
    },
    handleAdd() {
      this.dialogFormVisible = true
      this.form = {}
    },
    handleEdit(row) {
      this.form = row
      this.dialogFormVisible = true
    },
    del(id) {
      this.request.delete("/user/" + id).then(res => {
        if (res) {
          this.$message.success("删除成功")
          this.load()
        } else {
          this.$message.error("删除失败")
        }
      })
    },
    handleSizeChange(pageSize) {
      console.log(pageSize)
      this.pageSize = pageSize
      this.load()
    },
    handleCurrentChange(pageNum) {
      console.log(pageNum)
      this.pageNum = pageNum
      this.load()
    },
  },
}
</script>

<style>
.headerBg {
  background-color: #ccc;
}
</style>
