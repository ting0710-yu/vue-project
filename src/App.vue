<!-- 代码已包含 CSS：使用 TailwindCSS , 安装 TailwindCSS 后方可看到布局样式效果 -->
<template>
<div class="min-h-screen bg-gray-50">
<!-- 广告弹窗 -->
<div v-if="showAdModal" class="fixed inset-0 bg-black bg-opacity-70 z-[60] flex items-center justify-center">
<div class="bg-white w-10/12 max-w-lg rounded-xl overflow-hidden relative">
<div class="relative aspect-video">
<img src="https://ai-public.mastergo.com/ai/img_res/76c166e7556e8dd86d0ca0658d29264f.jpg"
class="w-full h-full object-cover" alt="promotion">
<button @click="closeAd" class="absolute top-4 right-4 rounded-full w-8 h-8 flex items-center justify-center bg-white/30 backdrop-blur-sm">
<i class="fas fa-times text-white text-xl"></i>
</button>
</div>
<div class="p-6 text-center bg-gradient-to-b from-white to-gray-50">
<div class="mb-3">
<img src="https://ai-public.mastergo.com/ai/img_res/fc5d5db3b76db3e26dac764d75484cb3.jpg"
class="h-8 mx-auto mb-2" alt="logo">
</div>
<h2 class="text-2xl font-medium mb-3">甜品大滿貫</h2>
<p class="text-gray-600 mb-4 text-sm">新鮮於你的甜蜜邂逅</p>
<div class="flex justify-center space-x-1 mt-4">
<div class="w-2 h-2 rounded-full bg-gray-800"></div>
<div class="w-2 h-2 rounded-full bg-gray-300"></div>
</div>
</div>
</div>
</div>
<!-- 用餐方式选择 -->
<div v-if="showDiningOption" class="fixed inset-0 bg-black bg-opacity-50 z-[60] flex items-center justify-center">
<div class="bg-white w-10/12 rounded-lg overflow-hidden">
<div class="relative h-40">
<img src="https://ai-public.mastergo.com/ai/img_res/795af837f3a10f6139138ca858a15792.jpg" 
class="w-full h-full object-cover" alt="dining">
<div class="absolute inset-0 bg-black/30 flex items-center justify-center">
<h2 class="text-2xl font-medium text-white">請選擇用餐方式</h2>
</div>
</div>
<div class="p-8 space-y-4">
<button
class="w-full py-6 bg-orange-500 text-white !rounded-button flex items-center justify-center space-x-3 relative overflow-hidden group hover:bg-orange-600 transition-colors"
@click="selectDiningOption('dineIn')">
<div class="absolute inset-0 bg-gradient-to-r from-orange-400 to-orange-500 opacity-0 group-hover:opacity-100 transition-opacity"></div>
<i class="fas fa-utensils text-xl relative z-10"></i>
<span class="text-lg relative z-10">內用</span>
</button>
<button
class="w-full py-6 bg-orange-500 text-white !rounded-button flex items-center justify-center space-x-3 relative overflow-hidden group hover:bg-orange-600 transition-colors"
@click="selectDiningOption('takeout')">
<div class="absolute inset-0 bg-gradient-to-r from-orange-400 to-orange-500 opacity-0 group-hover:opacity-100 transition-opacity"></div>
<i class="fas fa-shopping-bag text-xl relative z-10"></i>
<span class="text-lg relative z-10">外帶</span>
</button>
</div>
</div>
</div>
<!-- 店面地址选择 -->
<div v-if="showLocationSelect" class="fixed inset-0 bg-black bg-opacity-50 z-[60] flex items-center justify-center">
<div class="bg-white w-10/12 rounded-lg p-6 relative">
<button @click="showLocationSelect = false" class="absolute top-3 right-3 w-8 h-8 flex items-center justify-center text-gray-500">
<i class="fas fa-times text-lg"></i>
</button>
<h2 class="text-xl font-medium mb-6 text-center">請選擇店面地址</h2>
<div class="space-y-4">
<button
v-for="location in locations"
:key="location.id"
class="w-full py-4 px-4 bg-white border border-gray-200 !rounded-button flex items-center justify-between"
@click="selectLocation(location)">
<div class="flex items-center space-x-3">
<i class="fas fa-store text-orange-500"></i>
<div class="text-left">
<div class="font-medium">{{location.name}}</div>
<div class="text-sm text-gray-500">{{location.address}}</div>
</div>
</div>
<i class="fas fa-chevron-right text-gray-400"></i>
</button>
</div>
</div>
</div>
<!-- 人数选择 -->
<div v-if="showPartySize" class="fixed inset-0 bg-black bg-opacity-50 z-[60] flex items-center justify-center">
<div class="bg-white w-10/12 rounded-lg p-6 relative">
<button @click="showPartySize = false" class="absolute top-3 right-3 w-8 h-8 flex items-center justify-center text-gray-500">
<i class="fas fa-times text-lg"></i>
</button>
<h2 class="text-xl font-medium mb-6 text-center">請選擇用餐人數</h2>
<div class="flex justify-center items-center space-x-6">
<button @click="decreasePartySize" class="w-12 h-12 bg-gray-100 !rounded-button flex items-center justify-center">
<i class="fas fa-minus"></i>
</button>
<span class="text-2xl font-medium">{{partySize}}</span>
<button @click="increasePartySize" class="w-12 h-12 bg-gray-100 !rounded-button flex items-center justify-center">
<i class="fas fa-plus"></i>
</button>
</div>
<button
class="w-full py-4 bg-orange-500 text-white !rounded-button mt-6"
@click="confirmPartySize">
確認
</button>
</div>
</div>
<!-- 顶部导航栏 -->
<nav class="fixed top-0 w-full bg-white shadow-sm z-50 px-4 py-3 flex items-center justify-between">
<div class="text-xl font-medium">甜品坊</div>
<div class="relative">
<i class="fas fa-shopping-cart text-xl"></i>
<span v-if="cartCount" class="absolute -top-2 -right-2 bg-red-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">
{{cartCount}}
</span>
</div>
</nav>
<!-- 搜索栏 -->
<div class="mt-16 px-4 py-3">
<div class="relative">
<input
type="text"
placeholder="搜尋喜愛的甜品"
class="w-full pl-10 pr-4 py-2 bg-gray-100 !rounded-button"
>
<i class="fas fa-search absolute left-3 top-1/2 -translate-y-1/2 text-gray-400"></i>
</div>
</div>
<!-- 分类标签 -->
<div class="px-4 py-3">
<div class="flex space-x-4 overflow-x-auto hide-scrollbar">
<button
v-for="(category, index) in categories"
:key="index"
:class="{'bg-orange-500 text-white': currentCategory === index,
'bg-white text-gray-600': currentCategory !== index}"
class="px-4 py-2 whitespace-nowrap !rounded-button"
@click="currentCategory = index"
>
{{category}}
</button>
</div>
</div>
<!-- 横幅轮播 -->
<div class="w-full h-48 overflow-hidden">
<img src="https://ai-public.mastergo.com/ai/img_res/60eee8ea2953afe1a2af5a61b4b3eebf.jpg"
class="w-full h-full object-cover" alt="banner">
</div>
<!-- 双列布局：左侧分类 + 右侧商品列表 -->
<div class="flex">
<!-- 左侧分类菜单 -->
<div class="w-1/4 bg-gray-50 h-[calc(100vh-14rem)] overflow-y-auto">
<div v-for="(category, index) in categories"
:key="index"
:class="{'bg-white border-l-4 border-orange-500': currentCategory === index}"
class="py-4 px-2 text-center text-sm cursor-pointer"
@click="currentCategory = index">
{{category}}
</div>
</div>
<!-- 右侧商品列表 -->
<div class="w-3/4 h-[calc(100vh-14rem)] overflow-y-auto pb-24">
<div v-for="(item, index) in products"
:key="index"
class="flex p-4 border-b border-gray-100"
@click="showDetail(item)">
<div class="w-24 h-24 rounded-lg overflow-hidden flex-shrink-0">
<img :src="item.image" class="w-full h-full object-cover" :alt="item.name">
</div>
<div class="ml-4 flex-grow">
<h3 class="font-medium mb-1">{{item.name}}</h3>
<p class="text-sm text-gray-500 mb-2">{{item.description}}</p>
<div class="flex items-center justify-between">
<span class="text-orange-500">NT$ {{item.price}}</span>
<button class="bg-orange-500 text-white w-8 h-8 flex items-center justify-center !rounded-button"
@click.stop="addToCart(item)">
<i class="fas fa-plus"></i>
</button>
</div>
</div>
</div>
</div>
</div>
<!-- 底部导航 -->
<div class="fixed bottom-0 left-0 w-full bg-white shadow-lg px-4 py-3 flex items-center justify-between">
<div>
<div class="text-sm text-gray-500">總計</div>
<div class="text-lg font-medium">NT$ {{totalAmount}}</div>
</div>
<button class="bg-orange-500 text-white px-6 py-2 !rounded-button">
結帳
</button>
</div>
<!-- 商品详情弹窗 -->
<div v-if="showDetailModal" class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center" @click="showDetailModal = false">
<div class="bg-white w-11/12 rounded-lg max-h-[90vh] overflow-y-auto" @click.stop>
<img :src="selectedProduct.image" class="w-full aspect-video object-cover" :alt="selectedProduct.name">
<div class="p-4">
<h2 class="text-xl font-medium mb-2">{{selectedProduct.name}}</h2>
<p class="text-gray-600 mb-4">{{selectedProduct.description}}</p>
<div class="mb-4">
<h3 class="font-medium mb-2">甜度選擇</h3>
<div class="flex flex-wrap gap-2">
<button
v-for="(sweet, index) in sweetness"
:key="index"
:class="{'bg-orange-500 text-white': selectedSweet === index,
'bg-gray-100 text-gray-600': selectedSweet !== index}"
class="px-3 py-1 !rounded-button"
@click="selectedSweet = index"
>
{{sweet}}
</button>
</div>
</div>
<div class="flex items-center justify-between mt-4">
<div class="text-xl font-medium text-orange-500">
NT$ {{selectedProduct.price}}
</div>
<button class="bg-orange-500 text-white px-6 py-2 !rounded-button" @click="addToCart(selectedProduct)">
加入購物車
</button>
</div>
</div>
</div>
</div>
</div>
</template>
<script>
export default {
data() {
return {
showAdModal: true,
showDiningOption: false,
showLocationSelect: false,
showPartySize: false,
selectedDiningOption: null,
selectedLocation: null,
partySize: 1,
locations: [
{
id: 1,
name: '台北信義店',
address: '信義區松仁路 123 號'
},
{
id: 2,
name: '台北東區店',
address: '大安區忠孝東路四段 456 號'
},
{
id: 3,
name: '台北西門店',
address: '萬華區成都路 789 號'
}
],
cartCount: 2,
currentCategory: 0,
categories: ['熱門推薦', '傳統糖水', '特色甜品', '涼粉系列', '配料加購'],
products: [
{
name: '杏仁豆腐花',
description: '香滑細膩，搭配杏仁醬',
price: 60,
image: 'https://ai-public.mastergo.com/ai/img_res/8b7cd2d322160069a87139c83d5281ba.jpg'
},
{
name: '椰汁西米露',
description: '香濃椰奶，Q彈西米',
price: 65,
image: 'https://ai-public.mastergo.com/ai/img_res/778e99c7c09d99f8b726b1dc709d1b4f.jpg'
},
{
name: '薑汁撞奶',
description: '溫潤薑香，滑嫩牛奶',
price: 70,
image: 'https://ai-public.mastergo.com/ai/img_res/4b10c065c534ba28504fc04317119e0c.jpg'
},
{
name: '芒果西米露',
description: '新鮮芒果，香甜可口',
price: 75,
image: 'https://ai-public.mastergo.com/ai/img_res/31c029a429965659efed441171aa9cef.jpg'
}
],
showDetailModal: false,
selectedProduct: null,
selectedSweet: 1,
sweetness: ['無糖', '微糖', '正常', '全糖'],
totalAmount: 195
}
},
methods: {
closeAd() {
this.showAdModal = false;
this.showDiningOption = true;
},
selectDiningOption(option) {
this.selectedDiningOption = option;
this.showDiningOption = false;
this.showLocationSelect = true;
},
selectLocation(location) {
this.selectedLocation = location;
this.showLocationSelect = false;
this.showPartySize = true;
},
decreasePartySize() {
if (this.partySize > 1) {
this.partySize--;
}
},
increasePartySize() {
if (this.partySize < 10) {
this.partySize++;
}
},
confirmPartySize() {
this.showPartySize = false;
},
showDetail(product) {
this.selectedProduct = product;
this.showDetailModal = true;
},
addToCart(product) {
this.cartCount += 1;
// 添加成功提示可以在这里实现
}
}
}
</script>
<style scoped>
.hide-scrollbar {
-ms-overflow-style: none;
scrollbar-width: none;
}
.hide-scrollbar::-webkit-scrollbar {
display: none;
}
input {
outline: none;
}
</style>