<template>
	<view class="content">
		<view v-if="list.length != 0" class="header">
			<view class="header-left">
				<text class="active-text">{{text}}</text>
				<text>{{listData.length}}条</text>
			</view>
			<view class="header-right">
				<view class="header-right-item " :class="{'active-tab':activeIndex===0}" @click="tab(0)">全部</view>
				<view class="header-right-item" :class="{'active-tab':activeIndex===1}" @click="tab(1)">待办</view>
				<view class="header-right-item" :class="{'active-tab':activeIndex===2}" @click="tab(2)">已完成</view>
			</view>
		</view>
		<view v-if="list.length == 0" class="default">
			<view class="image-default">
				<text class="iconfont icon-list1152921easyiconnet" style="font-size: 200px; opacity: 0.1; justify-content: center;"></text>
			</view>
			<view class="default-info">
				<view class="default-info-text">您还没有创建任何待办事项</view>
				<view class="default-info-text">点击下方“+”创建</view>
			</view>
		</view>
		<view v-else class="todo-content">
			<view class="todo-list" :class="{'todo-finish':item.checked}" v-for="(item,index) of listData" :key="index" @click="finish(item.id)">
				<view class="todo-list-checkbox">
					<view class="checkbox"></view>
				</view>
				<view class="todo-list-content">
					{{item.content}}
				</view>
			</view>
		</view>
		
		
	<view class="create-todo" @click="create">
		<text class="iconfont icon-add"></text>
	</view>
	<view class="create-content" v-if="active">
		<view class="create-input">
			<input type="text" v-model="value" placeholder="请输入要增加的待办事项"/>
		</view>
		<view class="create-button" @click="add">
			创建
		</view>
	</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list:[],
				active:false,
				value:'',
				activeIndex:0,
				text:'全部'
			}
		},
		onLoad() {

		},
		computed:{
			listData(){
				let list = JSON.parse(JSON.stringify(this.list))
				let newList = []
				if(this.activeIndex===0){
					this.text = '全部'
					return list
				}
				if(this.activeIndex===1){
					list.forEach((item)=>{
						if(!item.checked){
							newList.push(item)
						}	
					})
					this.text = '待办'
					return newList
				}
				if(this.activeIndex===2){
					list.forEach((item)=>{
						if(item.checked){
							newList.push(item)
						}	
					})
				}
				this.text = '已完成'
				return newList
			}
		},
		methods: {
			create(){
			this.active = !this.active
			},
			add(){
				if(this.value === ''){
					uni.showToast({
						title:"请输入内容",
						icon:"none",
					})
					return
				}
				this.list.unshift({
					id:'id'+new Date().getTime(),
					content:this.value,
					checked:false
				})
				this.value = ''
				this.active = false
			},
			finish(id){
				
				let index = this.list.findIndex((item)=>item.id === id)
				console.log(this.list[index])
				this.list[index].checked = !this.list[index].checked
			},
			tab(index){
				this.activeIndex = index
			}
		}
	}
</script>

<style>
	@import '../../common/icon.css';
	@import '../../common/default.css';
	.header{
		position: fixed;
		top: 0px;
		width: 100%;
		z-index: 10;
		display: flex;
		align-items: center;
		height: 45px;
		color: #333333;
		font-size: 12px;
		padding: 0 15px;
		box-shadow: -1px 1px 5px 0 rgba(0,0,0,0.3);
		background-color: #fff;
		box-sizing: border-box;
	}
	.active-text{
		font-size: 14px;
		color: #279abf;
		padding-right: 10px;
	}
	.header-left{
		width: 100%;
	}
	.header-right{
		flex-shrink: 0;
		display: flex;
	},
	.header-right-item{
		padding: 0 5px;
	}
	.active-tab{
		color: #279abf;
	}
	.todo-content{
		position: relative;
		padding-top: 50px;
		padding-bottom: 80px;
	}
	.todo-list{
		display: flex;
		align-items: center;
		padding: 15px;
		margin: 15px;
		box-shadow: -1px 1px 5px 0 rgba(0,0,0,0.1),-1px 2px 1px 0 rgba(255,255,255) inset;
		border-radius: 10px;
		color: #0c3854;
		background-color: #cfebfd;
		font-size: 14px;
		position: relative;
		overflow: hidden;
	}
	.todo-list::after{
		position: absolute;
		content:'' ;
		top: 0;
		left: 0;
		width: 5px;
		background-color: #91d1e8;
		bottom: 0;
	}
	.todo-list-checkbox{
		padding-right: 15px;
	}
	.checkbox{
		width: 20px;
		height: 20px;
		border-radius: 50%;
		background-color: #FFFFFF;
		box-shadow: 0 0 5px 1px rgba(0,0,0,0.1);
	}

	.todo-finish .checkbox{
		background-color: #eee;	
		position: relative;
	}
	.todo-finish .checkbox::after{
		position: absolute;
		content: '';
		width: 10px;
		height: 10px;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		margin: auto;
		background-color: #cfebfd;
		border-radius: 50%;
		box-shadow: 0 0 2px 0px rgba(0,0,0,0.2) inset;
	}
	.todo-finish .todo-list-content{
		color: #999999;
	}
	.todo-finish.todo-list::after{
		background-color: #999999;
	}
	.todo-finish.todo-list::before{
		content: '';
		position: absolute;
		top: 0;
		bottom: 0;
		left: 40px;
		right: 10px;
		height: 2px;
		margin: auto;
		background-color: #bdcdb8;
	}
	.create-todo{
		display: flex;
		justify-content: center;
		position: fixed;
		align-items: center;
		bottom: 20px;
		left: 0;
		right: 0;
		width: 50px;
		height: 50px;
		margin: 0 auto;
		border-radius: 50%;
		background-color: #deeff5;
		box-shadow: -1px 1px 5px 2px rgba(0,0,0,0.1),-1px 1px 1px 0 rgba(255,255,255)inset;
	}
	.icon-add{
		color: #edd8e6;
		font-size: 25px;
	}
	.create-content{
		position: fixed;
		bottom: 95px;
		left: 20px;
		right: 20px;
		padding:0 15px;
		padding-right: 0;
		border-radius: 50px;
		background-color: #fff;
		box-shadow: -1px 1px 5px 2px rgba(0,0,0,0.1);
		display: flex;
		align-items: center;
		
	}
	.create-input{
		width: 100%;
		padding-right: 15px;
	}
	.create-button{
		display: flex;
		justify-content: center;
		align-items: center;
		flex-shrink: 0;
		width: 80px;
		height: 50px;
		border-radius: 50px;
		font-size: 16px;
		color: #88d4ec;
		box-shadow: -2px 0px 2px 1px rgba(0,0,0,0.1);
	}
	.default{
		padding-top: 100px;
	}
	.image-default{
		display: flex;
		justify-content: center;
	}
	.default-info{
		text-align: center;
		font-size: 14px;
		opacity: 0.3;
	}
</style>
