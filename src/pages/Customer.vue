<template>
    <div class="customer">
        <h2>顾客管理</h2>
        <!-- 按钮 -->
        <div class="btns">
            <button @click="toAddHandler">添加</button>
            <button>批量删除</button>
        </div>
        <!-- 表单 -->
        <form action="" v-show="visible" @submit.prevent="submitHandler">
            姓名<input type="text" v-model="form.realname">
            手机<input type="text" v-model="form.telephone">
            <input type="submit" value="提交">
            <input type="reset" value="取消" @click="closeMo">
        </form>
        <!-- 表格 -->
        <table>
            <thead>
                <tr>
                    <th>编号</th>
                    <th>姓名</th>
                    <th>手机号</th>
                    <th>状态</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="c in customerStatusFilter()" :key="c.id">
                    <td>{{c.id}}</td>
                    <td>{{c.realname}}</td>
                    <td>{{c.telephone}}</td>
                    <td>{{c.status}}</td>

                    <td>
                        <a href="" @click.prevent="deleteHandler(c.id)">删除</a>
                        <a href="" @click.prevent="editHandler(c)">修改</a>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>  

<script>
import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'

    export default{
        data(){
            return {
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