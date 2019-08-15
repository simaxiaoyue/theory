# 购物车小项目

> 目标
>
> 1.能使用jQuery的方式生成所有手机的商品列表
>
> 2.能根据链接的id参数获取商品并在详情页展示
>
> 3.能使用localStorage把购物车商品列表数据存储越来
>
> 4.能从localStorage里面计算出购物车商品的数量并显示
>
> 5.能实现购物车数据在购物车页面的显示
>
> 6.能实现商品的数量加减功能
>
> 7.能实现购物车中勾选商品的总价计算



## 项目初始化

在公司里面做开发，每次做项目，都是需要进行版本控制的，就需要使用git 了

第一步：在项目的根目录下面，右键，选择打开git的命令窗口(git bush here)

第二步：通过命令对项目的仓库进行初始化

```
git init
```

就会在当前目录下，创建一个.git文件夹，所有的版本信息都在里面

第三步：先进行第一个版本的管理，需要先暂存，再提交

```
git add . 
git commit -m '项目初始化的备注'
```



## 先实现列表页

实际开发中，列表页的商品数据都是从服务器获取的，但是现在没有服务器，需要我们自己构建一些假数据，先把代码写了

需要自己根据需求分析

分析出来的数据结构

```js
{
  // 每个数据的唯一标识
 	pID : 数字,
  // 商品的名字
  name : 字符串,
  // 图片路径 
  imgSrc : 字符串,
  // 价格
  price : 数字,
	// 已售出的百分比  
  percent : 数字,
  // 库存
  letf : 数字
}
```

就自己模拟了一个假数据的数组，在data.js里面

也就是说我们的数据已经有了(假装从服务器获取了数据)，需要使用动态生成元素的方式把商品展示给用户

遍历数组，生成结构

```js
let html = '';
phoneData.forEach(e=>{
  //使用html的方式生成结构
  html += `<li class="goods-list-item">
    <a href="detail.html?id=${e.pID}">
      <div class="item-img">
        <img src="${e.imgSrc}" alt="">
      </div>
      <div class="item-title">
        ${e.name}
      </div>
      <div class="item-price">
        <span class="now">¥${e.price}</span>
      </div>
      <div class="sold">
        <span> 已售 <em>${e.percent}% </em></span>
        <div class="scroll">
          <div class="per" style="width:${e.percent}%"></div>
        </div>
        <span>剩余<i>${e.left}</i>件</span>
      </div>
    </a>
    <a href="#" class="buy">
      查看详情
    </a>
  </li>`;
})
// 在循环结束之后，把html格式的字符串变成结构
$('.good-list>ul').html(html);
```



## 在详情页，根据id获取产品的信息，把对应的文字进行修改

获取浏览器地址栏里面的id，简称url里面的id

浏览器的地址栏，归location对象管，里面有一个属性，专门负责管理?后面的数据，?后面的数据我们称为url参数

属性名叫做： location.search

```js
console.log(location.search);
```

发现location.search都是一个格式： ?id=数字

我们需要根据数字，到数组中读取出对应的数据

```js
let id = parseInt(location.search.substring(4));
// 遍历数组，获取元素
let obj = phoneData.find(e=>{
  return e.pID === id;// 其实在开发里面，更加推荐使用 === ，更加严谨
})
```

之后就根据数据，进行修改信息

```js
// 更改产品的名字
$(".sku-name").text(obj.name);
// 改图片
$('.preview-img>img').attr('src',obj.imgSrc);
// 价格
$('.summary-price em').text('￥'+obj.price);
```



## 实现点击加入购物车

点击加入按钮在detail页面，而所有的购物车里面的商品在cart.html要么

就需要在两个不同的页面之间进行数据的传递，需要把商品的数据存储本地存储里面，然后在cart.html里面再进行读取

点击按钮，把对应的商品的信息，存储到localStorage

给加入购物车按钮注册点击事件，然后把对应的商品的信息，以数组的方式存储到本地

```js
$(".addshopcar").on('click',function(){
  let number = parseInt($('.choose-number').val());
  let arr = [];
  let good = {
    pID : obj.pID,
    name : obj.name,
    price : obj.price,
    imgSrc : obj.imgSrc,
    number : number
  }
  arr.push(good);
  let jsonStr = JSON.stringify(arr);
  localStorage.setItem('shopCartData',jsonStr);
})
```



但是我们此时点击加入购物车是会覆盖的，需要把新旧数据合并，先把旧的数据读取出来，此时读取出来的数据，可能是一个null，如果是null就给空数组，否则就转换为数组再操作

```js
$(".addshopcar").on('click',function(){
  // 把当前对应的商品的信息加入购物车
  // 把那些信息存到本地存储里面
  // 图片、名字、单价、数量、pID
  // 只有数量是未知，需要获取
  let number = parseInt($('.choose-number').val());
  // 需要把每个数据存储到localStorage里面，又因为可能有多个商品，所以我们要以数组的形式存
  // let arr = [];
  // 先从本地存储里面读取旧的数据，然后把新旧数据叠加
  let jsonStr = localStorage.getItem('shopCartData');
  let arr;
  //需要判断到底是否有数据
  if(jsonStr === null){
    arr = [];
  }else {
    arr = JSON.parse(jsonStr);
  }  
  let good = {
    pID : obj.pID,
    name : obj.name,
    price : obj.price,
    imgSrc : obj.imgSrc,
    number : number
  }
  arr.push(good);
  // 把数组变成json格式的字符串，存储到localStorage里面
  jsonStr = JSON.stringify(arr);
  localStorage.setItem('shopCartData',jsonStr);
})
```



但是有发现，如果同一个商品，点击了多次，会在本地存储中出现多个，这样是不合理的，因为我们在购物车页面，会根据数组，遍历生成购物车列表，同样的商品，应该只出现一次，如果有多个，应该是数量叠加，我们无需要在存储之前，先判断，旧数据里面是否已经存在了该产品

```js
// find 方法，如果找到了元素，就会返回该元素，但是如果没找到，会返回undefined
let isExit = arr.find(e=>{
  return e.pID === id;
});
// console.log(isExit);
// 如果isExit 是undefined，就是没有
if (isExit !== undefined) {
  // 把数量叠加
  isExit.number += number;
} else {
  // 如果没有出现过，才是新增一个
  let good = {
    pID: obj.pID,
    name: obj.name,
    price: obj.price,
    imgSrc: obj.imgSrc,
    number: number
  }
```



## 实现购物车的页面

两个页面之间传递数据的思路是： 在详情页点击加入购物车，就把数据，存储到本地数据里面，然后就可以在购物车页面，访问本地数据，读取出来，生成结构

如果打开购物车页面，一开始里面没东西，会显示一个提示，自己的页面里面没有，需要自己写一个

```html
<div class="empty-tip tc">
  你的购物车空空如也，快去<a href="./list.html">购物</a>吧！！！
</div>
```

把这个结构放在main这个div的最前面

一开始没有商品，下面的总计也不应该出现，给总计的盒子，加一个hidden的类名，把总结的div先隐藏

之后再从本地读取数据

在cart.html里面读取本地数据

```js
let jsonStr = localStorage.getItem('shopCartData');
console.log(jsonStr);
```

把本地数据判断一下，如果有，就生成结构

```js
if(jsonStr !== null){
    let arr = JSON.parse(jsonStr);
    // 遍历数组，生成结构
    let html = '';
    arr.forEach(e => {
      html += `<div class="item" data-id="6">
      <div class="row">
        <div class="cell col-1 row">
          <div class="cell col-1">
            <input type="checkbox" class="item-ck" checked="">
          </div>
          <div class="cell col-4">
            <img src="${e.imgSrc}" alt="">
          </div>
        </div>
        <div class="cell col-4 row">
          <div class="item-name">${e.name}</div>
        </div>
        <div class="cell col-1 tc lh70">
          <span>￥</span>
          <em class="price">${e.price}</em>
        </div>
        <div class="cell col-1 tc lh70">
          <div class="item-count">
            <a href="javascript:void(0);" class="reduce fl">-</a>
            <input autocomplete="off" type="text" class="number fl" value="${e.number}">
            <a href="javascript:void(0);" class="add fl">+</a>
          </div>
        </div>
        <div class="cell col-1 tc lh70">
          <span>￥</span>
          <em class="computed">${e.price * e.number}</em>
        </div>
        <div class="cell col-1">
          <a href="javascript:void(0);" class="item-del">从购物车中移除</a>
        </div>
      </div>
    </div>`;
    });
    // 把html格式的字符串，放到div里面
    $(".item-list").html(html)
    // 把空空如也隐藏
    $(".empty-tip").hide();
    // 把表头+总计显示出来
    $('.cart-header').removeClass('hidden');
    $('.total-of').removeClass('hidden');
  }
```

记得把空空如也，隐藏，把表头和总计的div显示出来



## 实现总计的计算

要计算总件数和总价，我们页面中的数据是从本地存储中获取的，我们也可以在计算的时候从本地存储中获取

有一个问题要解决：怎么知道本地存储中的哪个数据是勾选的

解决方法： 在多选框上面，用自定义属性存储一个和该多选框对应的商品的id，得到勾选的多选框，得到对应的id，去本地存储里面，对应的找id对应的商品，然后把他们的总数量和总的价格加起来

在生成结构的时候，就需要先把id存储到对应多选框上面，但是考虑到删除也要用到，我们把该id存储到两个元素共有的前代元素  div.item 上

```js
部分代码
<div class="item" data-id="${e.pID}">
```

接着看看那些多选框是选中的，然后从选中的多选框身上得到这些id，之后就判断，这些id在本地存储里面是否有，如果有，就需要参与总数和总价的计算

```js
// 算出总计里面的总数量和总价
// 根据选中的多选框，得到选中的商品的id
let totalCount = 0;
let totalMoney = 0;
$(".item-list input[type=checkbox]:checked").each((i,e)=>{
  // console.log(e);
  let id = parseInt($(e).parents('.item').attr('data-id'));
  // console.log(id)
  arr.forEach(e=>{
    if(id === e.pID){
      // 勾选的在本地存储中的数据
      totalCount += e.number;
      totalMoney += e.number * e.price;
    }
  })
});
// 修改数量和总价
$('.selected').text(totalCount);
$('.total-money').text(totalMoney);
```





## 实现勾选的总价和数量的动态改变

和我们以前一样实现全选和单选

然后把之前写过的总价计算，封装成为一个函数，每次点击多选框，都要调用一次



## 实现数量的加减

获取按钮，注册点击事件

如果点击的是加号，让数量+1

给加号按钮注册点击事件，获取输入框的内容，让其+1,

```js
// 使用委托的方式实现加减
  $('.item-list').on('click','.add',function(){
    // 点击加号，把对应的输入框的文字进行+1
    // 得到旧的数据
    let oldVal = parseInt($(this).siblings('input').val());
    // console.log(oldVal);
    oldVal++;
    // 设置回去
    $(this).siblings('input').val(oldVal);
  })
```

要记得把数据同步到本地存储里面，因为，每次数量的增减都要保存下来

```js
// 把本地存储里面的数据，更新
// 判断依据是 点击的按钮对应的商品的id
let id = parseInt($(this).parents('.item').attr('data-id'));
let obj = arr.find(e=>{
  return e.pID === id;
});
// 更新对应的数据
obj.number = oldVal;
// 还要覆盖回本地数据
let jsonStr = JSON.stringify(arr);
localStorage.setItem('shopCartData',jsonStr);
// 重新计算总数和总价
computedCountAndMoney();
```

记得重新计算总数和总价

如果点击的是减号，让数量-1

点击减号和加号逻辑几乎是一致的，但是需要注意的是，我们点击减号，只能减到1，如果再点，就不能点了



## 实现从购物车删除

我们要有一些页面交互的常识

比如要删除数据，这种危险的操作，需要让用户进行确认

弹出一个提示框让他确认

在原生的js里面，有一个确认的弹框

```js
confirm('你确认要删除吗？');
```

该函数的返回值是true和false，如果点击的是确定，就返回true，否则返回false

但是一般在开发里面不会用这个东西，因为丑。而且在不同的浏览器里面，长得还不一样，一般不会使用原生的弹出框，会自己写一个，或者用别人写好的。

使用jqueryUI里面提供的弹出确认框实现

使用步骤和以前一样，直接引入三个必要的文件，直接调用对应的方法即可

```js
 $( "#dialog-confirm" ).dialog({
    resizable: false,
    height:140,
    modal: true,
    buttons: {
      "确认": function() {
        $( this ).dialog( "close" );
        // 把对应的商品删除
      },
      "取消": function() {
        $( this ).dialog( "close" );
      }
    }
  });
```

需要在点击移除的时候，才确认

给删除按钮注册点击事件，但是因为所有的购物车里面的列表都是动态生成的，需要使用委托来注册事件

```js
  $('.item-list').on('click', '.item-del', function () {});
```

在点击的时候，需要先保存一个this，因为我们点击确认删除，是另外一个函数了，如果是另外一个函数，this就不是删除按钮，因为作用域链，我们在删除按钮的点击事件里面存储的变量可以被确认的点击事件访问，需要在外面先保存一个按钮的this

```js
$('.item-list').on('click', '.item-del', function () {
  let _this = this;
  // 弹窗，在确认框里面写删除的逻辑
});
```

接着需要弹出一个确认框

```js
$("#dialog-confirm").dialog({
  resizable: false,
  height: 140,
  modal: true,
  buttons: {
    "确认": function () {
      $(this).dialog("close");
      // 把对应的结构移除
      $(_this).parents('.item').remove();
      // 把本地数据移除
      // 我们现在需要根据id获取本地存储里面的数据
      let id = parseInt($(_this).parents('.item').attr('data-id'));
      // 把对应id的数据读取出来
      let obj = arr.find(e => {
        return e.pID === id;
      });
      // 把对应的id的数据从本地存储里面移除
      // arr.splice(从哪里开始删除,总共删除多少个);
      let index = arr.indexOf(obj);
      arr.splice(index, 1);
      // 把数据覆盖回本地
      let jsonStr = JSON.stringify(arr);
      localStorage.setItem('shopCartData', jsonStr);
    },
    "取消": function () {
      $(this).dialog("close");
    }
```

我们是在确认的处理程序里面把数据删掉的，记得要覆盖回本地



### 拓展一个新的数组的方法

数组.findIndex(function(e,i){	

​	e 是数组里面的元素

​	i 是索引

})

作用： 返回数组里面某个满足条件的元素的索引，是一个数字，如果有就返回正常的索引，如果没有就返回-1

回调函数要求要返回一个条件



## 实现头部的购物车的数量计算

头部购物车的数量多个页面都要使用，所以把它单独抽离称为一个公共的js文件，在每个需要它的地方直接引入即可

思路就是直接从本地数据中读取数组，遍历数组，把所有的数量累加就行

```js
$(() => {
  //计算购物车里面的商品总数就属于在多个页面都用的代码 - 提取到一个新的js里面
  // 读取本地数据里面的商品信息，计算出一个总数，修改购物车总的商品数量
  // 根据键从本地数据中读取出字符串
  let arr = kits.loadArray('shopCartData');
  //  console.log(arr);
  // 直接计算出总的数量，设置给红色的泡泡
  let total = 0;
  arr.forEach(e => {
    total += e.number
  });
  // console.log(total);
  // 修改到头部的购物车里面
  $('.count').text(total);
});
```

记得把那些可以封装的代码都封装起来，尤其是一些重复使用比较多











