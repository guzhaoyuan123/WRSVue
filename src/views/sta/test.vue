<template>
    <div>
        <div>
            <el-input size="small" class="input_type" placeholder="添加员工信息..."
                      prefix-icon="el-icon-plus" @keydown.enter.native="addPosition" v-model="staperss.name">
            </el-input>
            <el-button type="primary" icon="el-icon-plus" size="small" @click="addPosition">
                添加
            </el-button>
            <el-table :data="stapers" stripe border type="small" style="width: 70%" @selection-change="handleSelectionChange">
                <el-table-column prop="id" label="员工编号" width="56"></el-table-column>
                <el-table-column prop="name" label="员工姓名" width="180"></el-table-column>
                <el-table-column prop="gender" label="性别" width="130"></el-table-column>
                <el-table-column prop="birthday" label="出生日期" width="130"></el-table-column>
                <el-table-column prop="idCard" label="身份证号" width="130"></el-table-column>
                <el-table-column prop="nation" label="民族" width="130"></el-table-column>
                <el-table-column prop="nativePlace" label="籍贯" width="130"></el-table-column>
                <el-table-column prop="politicId" label="政治面貌" width="130"></el-table-column>
                <el-table-column prop="email" label="邮箱" width="130"></el-table-column>
                <el-table-column prop="phone" label="电话号码" width="130"></el-table-column>
                <el-table-column prop="address" label="联系地址" width="130"></el-table-column>
                <el-table-column prop="departmentId" label="所属部门" width="130"></el-table-column>
                <el-table-column prop="jobLevelId" label="职称Id" width="130"></el-table-column>
                <el-table-column prop="posId" label="职位Id" width="130"></el-table-column>
                <el-table-column prop="workId" label="工号" width="130"></el-table-column>
                <el-table-column prop="money" label="工资" width="130"></el-table-column>

                <el-table-column fixed="right" label="操作">
                    <template slot-scope="scope">
                        <el-button size="mini" @click="showEditDialog(scope.$index, scope.row)">编辑</el-button>
                        <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>
            <el-pagination
                    background
                    layout="prev, pager, next"
                    :page-size="pageSize"
                    :total="total"
                    @current-change="page">
            </el-pagination>
        </div>
        <el-dialog title="修改职位" :visible.sync="dialogVisible" width="30%">
            <div>
                <el-tag>职位名称</el-tag>
                <el-input class="update_input" size="small" v-model="updatePos.name"></el-input>
            </div>
            <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false" size="small">取 消</el-button>
        <el-button type="primary" @click="doUpdate" size="small">确 定</el-button>
      </span>
        </el-dialog>
    </div>
</template>

<script>
    export default {
        name: 'StaPers',
        data() {
            return {
                pageSize: '',
                total: '',
                stapers: {
                    name: "javaboy",
                    gender: "男",
                    birthday: "1989-12-31",
                    idCard: "610122199001011256",
                    nation: "汉族",
                    nativePlace: "陕西",
                    politicId: "普通公民",
                    email: "laowang@qq.com",
                    phone: "18565558897",
                    address: "深圳市南山区",
                    departmentId: "总办",
                    jobLevelId: "教授",
                    posId: "技术总监",
                    workID: "00000057",
                    money:20000
                },
                staperss: [],

                updatePos: {
                    name: '',
                    enabled:true
                },
                dialogVisible: false,
                multipleSelection: []
            }
        },
        methods: {
            page(currentPage) {
                const _this = this;
                axios.get('sta/stapers/findAll/' + (currentPage - 1) + '/5').then(function (resp) {
                    _this.staperss = resp.data.content
                    _this.pageSize = resp.data.size
                    _this.total = resp.data.totalElements
                })
            }
        },
    }
</script>

<style scoped>
    .input_type {
        width: 300px;
        margin-right: 8px;
        margin-bottom: 16px;
    }
    .update_input {
        width: 200px;
        margin-left: 8px;
    }
</style>