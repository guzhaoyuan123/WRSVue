<template>
    <div>
        <div>
            <div style="display: flex;justify-content: space-between">
                <div>
                    <el-input placeholder="请输入员工名,工号，性别进行搜索，可以直接回车搜索..." prefix-icon="el-icon-search"
                              clearable
                              @clear="initEmps"
                              style="width: 350px;margin-right: 10px" v-model="keyword"
                              @keydown.enter.native="initEmps" :disabled="showAdvanceSearchView"></el-input>
                    <el-button icon="el-icon-search" type="primary" @click="initEmps" :disabled="showAdvanceSearchView">
                        搜索
                    </el-button>
                </div>
                <div>
                    <el-button type="primary" icon="el-icon-plus" @click="showAddEmpView">
                        添加
                    </el-button>
                </div>
            </div>

            <el-table :data="staperss" stripe border type="small" style="width: 100%">
                <el-table-column prop="id" label="员工编号" width="130"></el-table-column>
                <el-table-column prop="name" label="员工姓名" width="130"></el-table-column>
                <el-table-column prop="gender" label="性别" width="80"></el-table-column>
                <el-table-column prop="birthday" label="出生日期" width="180"></el-table-column>
                <el-table-column prop="idcard" label="身份证号" width="200"></el-table-column>
                <el-table-column prop="nation" label="民族" width="70"></el-table-column>
                <el-table-column prop="nativeplace" label="籍贯" width="130"></el-table-column>
                <el-table-column prop="politicid" label="政治面貌" width="130"></el-table-column>
                <el-table-column prop="email" label="邮箱" width="130"></el-table-column>
                <el-table-column prop="phone" label="电话号码" width="130"></el-table-column>
                <el-table-column prop="address" label="联系地址" width="130"></el-table-column>
                <el-table-column prop="departmentid" label="所属部门" width="130"></el-table-column>
                <el-table-column prop="joblevelid" label="职称Id" width="130"></el-table-column>
                <el-table-column prop="posid" label="职位Id" width="130"></el-table-column>
                <el-table-column prop="workid" label="工号" width="130"></el-table-column>
                <el-table-column prop="money" label="工资" width="100"></el-table-column>

                <el-table-column fixed="right" label="操作" width="200">
                    <template slot-scope="scope">
                        <el-button @click="showEditEmpView(scope.row)" style="padding: 3px" size="mini">编辑</el-button>
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

        <!--        对话框-->
        <el-dialog
                :title="title"
                :visible.sync="dialogVisible"
                width="80%">
            <div>
                <el-form :model="stapers" ref="stapers">
                    <!--                    第一行-->
                    <el-row>
                        <el-col :span="6">
                            <el-form-item label="姓名:" prop="name">
                                <el-input size="mini" style="width: 150px" prefix-icon="el-icon-edit"
                                          v-model="stapers.name"
                                          placeholder="请输入员工姓名"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="5">
                            <el-form-item label="性别:" prop="gender">
                                <el-radio-group v-model="stapers.gender">
                                    <el-radio label="男">男</el-radio>
                                    <el-radio label="女">女</el-radio>
                                </el-radio-group>
                            </el-form-item>
                        </el-col>
                        <el-col :span="6">
                            <el-form-item label="出生日期:" prop="birthday">
                                <el-date-picker
                                        v-model="stapers.birthday"
                                        size="mini"
                                        type="date"
                                        value-format="yyyy-MM-dd hh:hh"
                                        style="width: 150px;"
                                        placeholder="出生日期">
                                </el-date-picker>
                            </el-form-item>
                        </el-col>
                        <el-col :span="7">
                            <el-form-item label="政治面貌:" prop="politicid">
                                <el-input size="mini" style="width: 150px" prefix-icon="el-icon-edit"
                                          v-model="stapers.politicid"
                                          placeholder="请输入政治面貌"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>

                    <!--                    第二行-->
                    <el-row>
                        <el-col :span="6">
                            <el-form-item label="民族:" prop="nation">
                                <el-input size="mini" style="width: 150px" prefix-icon="el-icon-edit"
                                          v-model="stapers.nation"
                                          placeholder="请输入民族"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="5">
                            <el-form-item label="籍贯:" prop="nativeplace">
                                <el-input size="mini" style="width: 120px" prefix-icon="el-icon-edit"
                                          v-model="stapers.nativeplace" placeholder="请输入籍贯"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="6">
                            <el-form-item label="电子邮箱:" prop="email">
                                <el-input size="mini" style="width: 150px" prefix-icon="el-icon-message"
                                          v-model="stapers.email" placeholder="请输入电子邮箱"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="7">
                            <el-form-item label="联系地址:" prop="address">
                                <el-input size="mini" style="width: 200px" prefix-icon="el-icon-edit"
                                          v-model="stapers.address" placeholder="请输入联系地址"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>
                    <!--                    第三行-->
                    <el-row>
                        <el-col :span="6">
                            <el-form-item label="职位:" prop="posid">
                                <el-input size="mini" style="width: 150px" prefix-icon="el-icon-edit"
                                          v-model="stapers.posid"
                                          placeholder="请输入职位"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="5">
                            <el-form-item label="职称:" prop="joblevelid">
                                <el-input size="mini" style="width: 150px" prefix-icon="el-icon-edit"
                                          v-model="stapers.joblevelid"
                                          placeholder="请输入职称"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="6">
                            <el-form-item label="所属部门:" prop="departmentid">
                                <el-input size="mini" style="width: 150px" prefix-icon="el-icon-edit"
                                          v-model="stapers.departmentid"
                                          placeholder="请输入所属部门"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="7">
                            <el-form-item label="电话号码:" prop="phone">
                                <el-input size="mini" style="width: 200px" prefix-icon="el-icon-phone"
                                          v-model="stapers.phone" placeholder="电话号码"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col :span="6">
                            <el-form-item label="工号:" prop="workid">
                                <el-input size="mini" style="width: 150px" prefix-icon="el-icon-edit"
                                          v-model="stapers.workid" placeholder="工号"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="8">
                            <el-form-item label="身份证号码:" prop="idcard">
                                <el-input size="mini" style="width: 180px" prefix-icon="el-icon-edit"
                                          v-model="stapers.idcard" placeholder="请输入身份证号码"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="6">
                            <el-form-item label="工资:" prop="money">
                                <el-input size="mini" style="width: 180px" prefix-icon="el-icon-edit"
                                          v-model="stapers.money" placeholder="请输入工资"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>
                </el-form>
            </div>

            <span slot="footer" class="dialog-footer">
                <el-button @click="dialogVisible = false" size="small">取 消</el-button>
                <el-button type="primary" @click="doAddEmp('stapers')" size="small">确 定</el-button>
              </span>
        </el-dialog>

    </div>
</template>

<script>
    export default {
        name: 'StaPers',
        data() {
            return {
                keyword: '',
                loading: false,
                pageSize: '',
                total: '',
                showAdvanceSearchView: false,
                title: '',
                stapers: {
                    name: "",
                    gender: "",
                    birthday: "",
                    idcard: "",
                    nation: "",
                    nativeplace: "",
                    politicid: "",
                    email: "",
                    phone: "",
                    address: "",
                    departmentid: "",
                    joblevelid: "",
                    posid: "",
                    workid: "",
                    money: ""
                },
                staperss: [{
                    name: "javaboy",
                    gender: "男",
                    birthday: "1989-12-31",
                    idcard: "610122199001011256",
                    nation: "汉族",
                    nativeplace: "陕西",
                    politicid: "普通公民",
                    email: "laowang@qq.com",
                    phone: "18565558897",
                    address: "深圳市南山区",
                    departmentid: "总办",
                    joblevelid: "教授",
                    posid: "技术总监",
                    workid: "00000057",
                    money: 20000
                }],

                updatePos: {
                    name: '',
                    enabled: true
                },
                dialogVisible: false,
                multipleSelection: []
            }
        },
        methods: {
            emptyEmp() {
                this.stapers = {
                    name: "",
                    gender: "",
                    birthday: "",
                    idcard: "",
                    nation: "",
                    nativeplace: "",
                    politicid: "",
                    email: "",
                    phone: "",
                    address: "",
                    departmentid: null,
                    joblevelid: "",
                    posid: "",
                    workid: "",
                    money: "",
                }
            },
            handleDelete(index, data) {
                this.$confirm('此操作将永久删除' + data.name + '职位, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                    this.deleteRequest('/sta/stapers/deleteById/' + data.id).then(resp => {
                        this.page2()
                    })
                }).catch(() => {
                    this.$message({
                        type: 'info',
                        message: '已取消操作'
                    });
                });
            },
            showEditEmpView(data) {
                this.title = '编辑员工信息';
                Object.assign(this.stapers, data)
                this.dialogVisible = true;
            },
            doAddEmp(formName) {
                if (this.stapers.id) {
                    const _this = this
                    this.$refs[formName].validate(valid => {
                        if (valid) {
                            axios.put('/sta/stapers/update', this.stapers).then(function (resp) {
                                if (resp) {
                                    _this.$message('修改成功');
                                    _this.dialogVisible = false;
                                    _this.page2();
                                }
                            })
                        }
                    });
                } else {
                    const _this = this
                    this.$refs[formName].validate(valid => {
                        if (valid) {
                            axios.post('/sta/stapers/save', this.stapers).then(function (resp) {
                                if (resp) {
                                    _this.$message('添加成功');
                                    _this.dialogVisible = false;
                                    _this.page2();
                                }

                            })
                        }
                    });
                }
            },
            showAddEmpView() {
                this.emptyEmp();
                this.title = '添加员工';
                this.dialogVisible = true;
            },

            page(currentPage) {
                const _this = this;
                axios.get('sta/stapers/findAll/' + (currentPage - 1) + '/7').then(function (resp) {
                    _this.staperss = resp.data.content
                    _this.pageSize = resp.data.size
                    _this.total = resp.data.totalElements
                })
            },
            page2() {
                const _this = this;
                axios.get('sta/stapers/findAll/0/7').then(function (resp) {
                    _this.staperss = resp.data.content
                    _this.pageSize = resp.data.size
                    _this.total = resp.data.totalElements
                })
            },

            initEmps() {
                const _this = this;
                let k = this.keyword;
                if (this.keyword) {
                    axios.get('sta/stapers/findByNameLike/' + k + '').then(function (resp) {
                        if (resp.data.length) {
                            _this.staperss = resp.data;
                            _this.total = resp.total;
                        }else if(1){
                            axios.get('sta/stapers/findByGenderLike/' + k + '').then(function (resp2) {
                                if (resp2.data.length){
                                    _this.staperss = resp2.data;
                                    _this.total = resp2.total;
                                }else {
                                    axios.get('sta/stapers/findByIdLike/' + k + '').then(function (resp3) {
                                        if (resp3.data.length){
                                            _this.staperss = resp3.data;
                                            _this.total = resp3.total;
                                        }
                                    })
                                }
                            })
                        }
                    });
                } else {
                    this.page2()
                }
            }
        },
        created() {
            const _this = this;
            axios.get('sta/stapers/findAll/0/7').then(function (resp) {
                _this.staperss = resp.data.content
                _this.pageSize = resp.data.size
                _this.total = resp.data.totalElements
            })
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