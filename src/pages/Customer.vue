<template>
    <div class="customer">
        <h2>顾客管理</h2>
        <!-- 按钮 -->
        <div class="btns">
            <el-button @click="toAddHandler" type="primary" size="small">添加</el-button>
            <el-button type="danger" size="small">批量删除</el-button>
        </div>
        <!-- 表单 -->
        <form action="" v-show="visible" @submit.prevent="submitHandler">
            姓名<input type="text" v-model="form.realname">
            手机<input type="text" v-model="form.telephone">
            <input type="submit" value="提交">
            <input type="reset" value="取消" @click="closeMo">
        </form>

        <el-button type="text" @click="dialogFormVisible = true">打开嵌套表单的 Dialog</el-button>

        <el-dialog title="添加顾客信息" :visible.sync="visible">
        <el-form :model="form">
            <el-form-item label="姓名" label-width="100px;">
                <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="手机号" label-width="100px;">
                <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="活动区域" :label-width="formLabelWidth">
            <el-select v-model="form.region" placeholder="请选择活动区域">
                <el-option label="区域一" value="shanghai"></el-option>
                <el-option label="区域二" value="beijing"></el-option>
            </el-select>
            </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
        </div>
        </el-dialog>
        <!-- 表格 -->
        <el-table :data="customers">
            <el-table-column type="selection"  width="55"></el-table-column>
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="realname" label="姓名"></el-table-column>
            <el-table-column prop="telephone" label="手机号"></el-table-column>
            <el-table-column prop="status" label="状态"></el-table-column>
            <el-table-column  label="操作">
                <template #default="record">
                    <a href="" @click.prevent="deleteHandler(record.row.id)">删除</a>
                    <a href="" @click.prevent="deleteHandler(recprd.row)">修改</a>
                </template>
            </el-table-column>
        </el-table>
        <!-- <el-table :data="customers" size="mini"  @selection-change="handleSelectionChange">
        <el-table-column type="selection" width="55"></el-table-column>
        <el-table-column prop="id" label="编号"></el-table-column>
        <el-table-column prop="realname" label="姓名"></el-table-column>
        <el-table-column prop="telephone" label="手机号"></el-table-column>
        <el-table-column prop="status" label="状态"></el-table-column>
        <el-table-column label="操作">
          <template #default="record">
              <i class="el-icon-delete" href="" @click.prevent="deleteHandler(record.row.id)"></i> &nbsp;
							<i class="el-icon-edit-outline" href="" @click.prevent="editHandler(record.row)"></i>
          </template>
        </el-table-column>
      </el-table> -->
        
    </div>
</template>  

<script>
import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'

    export default{
        data(){
            return {
                customer:{},
                form:{}
            }
        },
        created() {
            this.findAllCustomers();
        },
        computed: {
            // 映射，将vuex state中的状态映射为vue中属性
            ...mapState("customer",["customers","visible"]),
            ...mapGetters("customer",["countCustomers","customerStatusFilter"])
            //普通属性
        },
        methods: {
         	//映射，将action中得到动作映射为vue中的方法
            ...mapActions("customer",["findAllCustomers","deleteCustomerById","saveOrUpdateCustomer"]),
            ...mapMutations("customer",["showMo","closeMo"]),
            //普通方法
            toAddHandler(){
            	//	
            	this.showMo();
            },
            submitHandler() {
            	//表单验证
            	//提交表单
            	this.saveOrUpdateCustomer(this.form)
            	.then((response)=>{
            		alert(response.statusText);
            	})
            	console.log("vue",this.form);
            },
            deleteHandler(id) {
            	let promise = this.deleteCustomerById(id);
            	promise.then((response)=>{
            		alert(response.statusText);
            	});
            	console.log("promise",promise);
            	
            },
            editHandler(row) {
            	this.form = row;
            	this.showMo();
            }
        }
    }
</script>
<style scoped>

</style>