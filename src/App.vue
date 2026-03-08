<template>
  <div class="container">
    <!-- 头部 -->
    <header class="header">
      <h1 class="logo">淘宝<span class="cart-text">购物车</span></h1>
    </header>

    <!-- 筛选栏 -->
    <div class="filter-bar">
      <div class="filter-tabs">
        <span class="filter-tab active">全部商品(4)</span>
        <span class="filter-tab">消费券</span>
        <span class="filter-tab">官方立减</span>
        <span class="filter-tab">降价</span>
      </div>
    </div>

    <!-- 操作栏 -->
    <div class="action-bar">
      <div class="action-left">
        <input type="checkbox" class="select-all" v-model="selectAll" @change="toggleSelectAll">
        <label>全选</label>
        <button class="action-btn" @click="moveToFavorites">移入收藏</button>
        <button class="action-btn" @click="deleteSelected">删除</button>
      </div>
      <div class="action-right">
        <select class="category-select">
          <option>分类</option>
        </select>
        <select class="status-select">
          <option>状态</option>
        </select>
        <div class="search-cart">
          <input type="text" placeholder="搜索您购物车内商品" class="cart-search-input">
          <button class="cart-search-btn">🔍</button>
        </div>
      </div>
    </div>

    <!-- 优惠券提示 -->
    <div class="coupon-alert">
      <span class="coupon-icon">🎫</span>
      <span>您有2张共65元消费券，可尽快使用</span>
      <span class="coupon-countdown" id="countdown">距离结束 01:15</span>
    </div>

    <!-- 分隔线 -->
    <div class="divider"></div>

    <!-- 商品列表 -->
    <div class="cart-items">
      <CartItem v-for="item in cartItems" :key="item.id" :item="item" @toggle-select="toggleSelect" />
    </div>

    <!-- 猜你喜欢 -->
    <div class="recommendations">
      <h3 class="recommend-title">猜你喜欢</h3>
      <div class="recommend-items">
        <RecommendItem v-for="recommendation in recommendations" :key="recommendation.id" :recommendation="recommendation" />
      </div>
    </div>

    <!-- 结算侧边栏 -->
    <div class="checkout-sidebar">
      <!-- 搜索框 -->
      <div class="sidebar-search">
        <input type="text" placeholder="搜索商品" class="sidebar-search-input">
        <button class="sidebar-search-btn">🔍</button>
      </div>
      <!-- 结算块 -->
      <div class="checkout-block">
        <div class="block-header">
          <h3>结算明细</h3>
        </div>
        <div class="block-content">
          <div class="empty-cart" v-if="selectedItems.length === 0">
            <div class="empty-icon">🛒</div>
            <div class="empty-hint">选择商品查看实际支付价格</div>
          </div>
          <div class="selected-items" v-else>
            <div class="selected-item-preview" v-for="item in selectedItems" :key="item.id">
              <img :src="item.image" alt="商品图片" />
              <div class="item-title">{{ item.title }}</div>
              <div class="item-price">{{ item.price }}</div>
            </div>
          </div>
          <div class="checkout-details" v-if="selectedItems.length > 0">
            <div class="price-details">
              <div class="price-item">
                <span>商品总价</span>
                <span class="price-value">{{ subtotal }}</span>
              </div>
              <div class="price-item">
                <span>已选优惠</span>
                <span class="discount-value">-{{ discount }}</span>
              </div>
              <div class="price-item">
                <span>平台优惠</span>
                <span class="discount-value">-{{ platformDiscount }}</span>
              </div>
              <div class="price-item">
                <span>红包</span>
                <span class="discount-value">-{{ coupon }}</span>
              </div>
              <div class="price-item">
                <span>实付金额</span>
                <span class="price-value">{{ total }}</span>
              </div>
            </div>
          </div>
        </div>
        <div class="block-footer">
          <div class="total-price">
            <span>合计:</span>
            <span class="total-value">{{ total }}</span>
          </div>
          <button class="checkout-btn">结算({{ selectedItems.length }})</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CartItem from './components/CartItem.vue';
import RecommendItem from './components/RecommendItem.vue';

export default {
  name: 'App',
  components: {
    CartItem,
    RecommendItem
  },
  data() {
    return {
      selectAll: false,
      cartItems: [
        {
          id: 1,
          shopName: '天猫',
          shopTitle: '依筱女装旗舰店',
          title: '38焕新周 黑色西装外套女2026新款韩版套装女支付',
          price: '¥102',
          originalPrice: '¥238',
          image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=black%20suit%20women%20formal%20wear%20professional%20outfit&image_size=square',
          selected: false
        },
        {
          id: 2,
          shopName: '淘宝',
          shopTitle: '豆互衣铺',
          title: '38焕新周 高级感西装套装女通勤职业正装',
          price: '¥174',
          originalPrice: '¥198',
          image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=high%20quality%20women%20suit%20professional%20business%20attire&image_size=square',
          selected: false
        },
        {
          id: 3,
          shopName: '天猫',
          shopTitle: '语术花旗舰店',
          title: '38焕新周 法式蓝色职业衬衫女秋季新款叠穿',
          price: '¥76.5',
          originalPrice: '¥87',
          image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=women%20professional%20blouse%20office%20wear%20formal&image_size=square',
          selected: false
        }
      ],
      recommendations: [
        {
          id: 1,
          title: '高级感西装套装女通勤职业正装',
          price: '¥174',
          image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=high%20quality%20women%20suit%20professional%20business%20attire&image_size=square'
        },
        {
          id: 2,
          title: 'Youthit优思益针叶素护眼',
          price: '¥97',
          image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=nutritional%20supplements%20vitamin%20C%20bottle&image_size=square'
        },
        {
          id: 3,
          title: '面试正装套装女春秋小个子大学生',
          price: '¥60.7',
          image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=women%20formal%20wear%20office%20attire&image_size=square'
        },
        {
          id: 4,
          title: '韩版宽松休闲小西装外套女春秋',
          price: '¥79',
          image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=professional%20women%20suit%20with%20blazer&image_size=square'
        }
      ],
      subtotal: '¥0',
      discount: '¥0',
      platformDiscount: '¥0',
      coupon: '¥0',
      total: '¥0'
    };
  },
  computed: {
    selectedItems() {
      return this.cartItems.filter(item => item.selected);
    }
  },
  methods: {
    toggleSelectAll() {
      this.cartItems.forEach(item => {
        item.selected = this.selectAll;
      });
      this.updateTotal();
    },
    toggleSelect(item) {
      item.selected = !item.selected;
      this.selectAll = this.cartItems.every(item => item.selected);
      this.updateTotal();
    },
    moveToFavorites() {
      // 移入收藏逻辑
    },
    deleteSelected() {
      // 删除逻辑
    },
    updateTotal() {
      // 更新总价逻辑
    }
  }
};
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}
.header {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}
.logo {
  font-size: 24px;
  font-weight: bold;
}
.cart-text {
  color: #ff5000;
}
.filter-bar {
  margin-bottom: 20px;
}
.filter-tabs {
  display: flex;
  gap: 20px;
}
.filter-tab {
  cursor: pointer;
  padding: 5px 10px;
  border-radius: 4px;
}
.filter-tab.active {
  background-color: #ff5000;
  color: white;
}
.action-bar {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}
.action-left, .action-right {
  display: flex;
  align-items: center;
  gap: 10px;
}
.select-all {
  margin-right: 5px;
}
.action-btn {
  padding: 5px 10px;
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
}
.cart-search-input {
  padding: 5px 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}
.cart-search-btn {
  padding: 5px 10px;
  background-color: #ff5000;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.coupon-alert {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
  padding: 10px;
  background-color: #fff8e6;
  border-radius: 4px;
}
.coupon-icon {
  font-size: 20px;
}
.divider {
  height: 1px;
  background-color: #ddd;
  margin-bottom: 20px;
}
.cart-items {
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin-bottom: 20px;
}
.recommendations {
  margin-bottom: 20px;
}
.recommend-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
}
.recommend-items {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}
.checkout-sidebar {
  position: fixed;
  right: 20px;
  top: 20px;
  width: 300px;
  background-color: white;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 20px;
}
.sidebar-search {
  display: flex;
  margin-bottom: 20px;
}
.sidebar-search-input {
  flex: 1;
  padding: 5px 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}
.sidebar-search-btn {
  padding: 5px 10px;
  background-color: #ff5000;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.checkout-block {
  border-top: 1px solid #ddd;
  padding-top: 20px;
}
.block-header {
  margin-bottom: 20px;
}
.block-header h3 {
  font-size: 18px;
  font-weight: bold;
}
.empty-cart {
  text-align: center;
  padding: 20px;
}
.empty-icon {
  font-size: 40px;
  margin-bottom: 10px;
}
.empty-hint {
  color: #999;
}
.selected-items {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.selected-item-preview {
  display: flex;
  align-items: center;
  gap: 10px;
}
.selected-item-preview img {
  width: 50px;
  height: 50px;
  object-fit: cover;
  border-radius: 4px;
}
.price-details {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-top: 20px;
}
.price-item {
  display: flex;
  justify-content: space-between;
}
.price-value, .discount-value {
  font-weight: bold;
}
.block-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
  padding-top: 20px;
  border-top: 1px solid #ddd;
}
.total-price {
  font-size: 18px;
  font-weight: bold;
}
.checkout-btn {
  padding: 10px 20px;
  background-color: #ff5000;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>