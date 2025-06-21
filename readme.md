# 步骤

视频教程: https://www.bilibili.com/video/BV1i94y147Zg/?vd_source=de81995ce6bebaf25b1f4c54e95a1f2f

1. 访问网页版pdd
2. 登录
3. 打开订单页面
4. F12 打开Chrome控制台 
5. 导航到"源代码标签页",在指定代码处设置断点(注1)
6. 网页上使用空格加载所有订单(此时代码暂停到断点处)
7. 控制台运行下面代码(注2)


## 1. 访问网页版pdd

https://mobile.pinduoduo.com/

显示手机登录

## 2. 登录

点击手机登录

输入手机号码

点击发送验证码按钮

输入验证码

勾选同意

点击登录

## 3. 打开订单页面

点击我的订单右侧 "查看全部"

## 4. F12 打开Chrome控制台 

点击鼠标右键,选择"检查"选项,或则按快捷键F12


## 5. 导航到"源代码标签页",在指定代码处设置断点(注1)

点击"源代码"标签页

选择左侧 "static.pddpic.com" -> "assets" -> "js" -> "react_orders_xxxxx"

使用快捷键 Ctrl+F 搜索 `this.log("list has loaded all items")`

点击左侧 `-`, 点击后显示蓝色箭头

上下文代码见(注1)

## 6. 网页上使用空格加载所有订单(此时代码暂停到断点处)

在网页上向下滑动加载所有订单

也可以使用空格键快速下滑

待所有订单加载完成之后js程序会暂停运行,并且停在刚才的断点处.

整行代码呈现淡蓝色底色,表示当前代码执行的位置.

## 7. 控制台运行下面代码(注2)

导航到"控制台"标签页.

复制/粘贴 注2 的代码. 按键盘回车键.

控制台显示结果.

点击右下角"复制",保存到文本文件或者excel.



# 脚注
## 注1: 加载完所有订单断点

```
t.prototype.checkListViewLoadedAll = function() {
	!this.hasCalledLoadedAll && this.props.show && this.loadStatus === w.LOADED_ALL && this.state.list.length >= this.itemsStore.length && (this.hasCalledLoadedAll = !0,
	this.state.showLoading && this.setState({
		showLoading: !1,
		hasLoadedAll: !0
	}),
	this.log("list has loaded all items"),  // <<<<<<<< 断点
	"function" == typeof this.props.viewLoadedAllCallback && this.props.viewLoadedAllCallback())
}
```

## 注2: 控制台导出代码
```

out="" // 删除用变量

for (let i = 0; i < this.itemsStore.length; i++) {


	order_sn=this.itemsStore[i].orderSn // 订单ID
	buyUrl="https://mobile.pinduoduo.com/goods.html?goods_id="
	if(this.itemsStore[i].type==1){ // 一般订单
		name=this.itemsStore[i].orderGoods[0].goodsName
		spec=this.itemsStore[i].orderGoods[0].spec
		buyUrl+=this.itemsStore[i].orderGoods[0].goodsId
		goodsPrice=this.itemsStore[i].orderGoods[0].goodsPrice
		goodsQty=this.itemsStore[i].orderGoods[0].goodsNumber
		orderAmount = this.itemsStore[i].orderAmount
		
		// 整理创建时间格式
		create_at = this.itemsStore[i].orderTime

	
	}else if(this.itemsStore[i].type==2){ // 多多买菜
		name=this.itemsStore[i].orders[0].orderGoods[0].goodsName
		spec=this.itemsStore[i].orders[0].orderGoods[0].spec
		buyUrl="多多买菜" // @todo 我也弄不来...
		goodsPrice=this.itemsStore[i].orders[0].orderGoods[0].goodsPrice/100
		goodsQty=this.itemsStore[i].orders[0].orderGoods[0].goodsNumber
		orderAmount = this.itemsStore[i].displayAmount/100
		
		// 整理创建时间格式
		create_at = parseInt(this.itemsStore[i].sortId.slice(0, 10))
	}else{
		console.log("未知类型: "+i)
	}
	
	create_at = new Date(create_at*1000)
	create_at = [
	  create_at.getFullYear(),
	  (create_at.getMonth() + 1).toString().padStart(2, '0'),
	  create_at.getDate().toString().padStart(2, '0'),
	].join('-') +
	' ' +
	[
	  create_at.getHours().toString().padStart(2, '0'),
	  create_at.getMinutes().toString().padStart(2, '0'),
	  create_at.getSeconds().toString().padStart(2, '0'),
	].join(':')

		
	orderStatusPrompt = this.itemsStore[i].orderStatusPrompt
	mall_name = this.itemsStore[i].mall.mallName
	// 调试用
	//console.log(create_at)
	//console.log(name)
	//console.log(spec)
	//console.log(orderStatusPrompt)
	//console.log(orderAmount)

  // 整理输出记录格式(每条) (按照自己需要的格式)
	out=out
		+name+"\t"
		+goodsPrice+"\t"
		+goodsQty+"\t"
		+orderAmount+"\t"
		+spec+"\t"
		+buyUrl+"\t"
		+create_at+"\t"
		+order_sn+"\t"
		+mall_name+"\t"
		+orderStatusPrompt+"\t"
		+"\r\n"
}

// 输出到控制台
console.log(out)

```
