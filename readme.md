# 步骤


1. 访问网页版pdd
1. 登录
1. 打开订单页面
1. F12 打开Chrome控制台 
1. 导航到"源代码标签页",在指定代码处设置断点(注1)
1. 网页上使用空格加载所有订单(此时代码暂停到断点处)
1. 控制台运行下面代码(注2)


# 注1: 加载完所有订单断点

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

# 注2: 控制台导出代码
```

out="" // 删除用变量

for (i = 0; i < this.itemsStore.length; i++) {

  // 整理创建时间格式
	create_at = this.itemsStore[i].groupOrder.createAt
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

	order_sn=this.itemsStore[i].orderSn // 订单ID
	buyUrl="https://mobile.pinduoduo.com/goods.html?goods_id="
	if(this.itemsStore[i].type==1){ // 一般订单
		name=this.itemsStore[i].orderGoods[0].goodsName
		spec=this.itemsStore[i].orderGoods[0].spec
		buyUrl+=this.itemsStore[i].orderGoods[0].goodsId
		goodsPrice=this.itemsStore[i].orderGoods[0].goodsPrice
		goodsQty=this.itemsStore[i].orderGoods[0].goodsNumber
		orderAmount = this.itemsStore[i].orderAmount
	}else if(this.itemsStore[i].type==2){ // 多多买菜
		name=this.itemsStore[i].orders[0].orderGoods[0].goodsName
		spec=this.itemsStore[i].orders[0].orderGoods[0].spec
		buyUrl="多多买菜" // @todo 我也弄不来...
		goodsPrice=this.itemsStore[i].orders[0].orderGoods[0].goodsPrice/100
		goodsQty=this.itemsStore[i].orders[0].orderGoods[0].goodsNumber
		orderAmount = this.itemsStore[i].displayAmount/100
	}else{
		console.log("未知类型: "+i)
	}
	
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
