<template>
  <div class="cart-item">
    <input type="checkbox" class="item-checkbox" v-model="item.selected" @change="toggleSelect">
    <div class="shop-info">
      <span class="shop-name">{{ item.shopName }}</span>
      <span class="shop-title">{{ item.shopTitle }}</span>
      <span class="shop-tag">🏆</span>
    </div>
    <div class="item-content">
      <div class="item-image">
        <img :src="item.image" alt="商品图片">
      </div>
      <div class="item-info">
        <div class="item-title">{{ item.title }}</div>
        <div class="item-attrs">
          <span>尺码: M码(常规型)</span>
          <span>颜色分类: 黑色西装</span>
          <span>半身裙长度: 中</span>
        </div>
        <div class="item-promos">
          <span class="promo-tag">消费券</span>
          <span class="promo-tag">官方立减31元</span>
          <span class="promo-tag">退货宝</span>
          <span class="promo-tag">大促价...</span>
        </div>
      </div>
      <div class="item-price">
        <div class="price-info">
          <span class="price-label">拼标价</span>
          <span class="price-value">{{ item.price }}</span>
        </div>
        <div class="original-price">{{ item.originalPrice }}</div>
        <div class="add-price">加入立减 ¥25</div>
      </div>
      <div class="item-quantity">
        <button class="quantity-btn minus" @click="decreaseQuantity">-</button>
        <input type="number" v-model="quantity" class="quantity-input">
        <button class="quantity-btn plus" @click="increaseQuantity">+</button>
      </div>
      <div class="item-actions">
        <button class="action-link" @click="moveToFavorites">移入收藏</button>
        <button class="action-link" @click="deleteItem">删除</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CartItem',
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      quantity: 1
    };
  },
  methods: {
    toggleSelect() {
      this.$emit('toggle-select', this.item);
    },
    decreaseQuantity() {
      if (this.quantity > 1) {
        this.quantity--;
      }
    },
    increaseQuantity() {
      this.quantity++;
    },
    moveToFavorites() {
      // 移入收藏逻辑
    },
    deleteItem() {
      // 删除逻辑
    }
  }
};
</script>

<style scoped>
.cart-item {
  display: flex;
  align-items: center;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-bottom: 20px;
}
.item-checkbox {
  margin-right: 10px;
}
.shop-info {
  display: flex;
  flex-direction: column;
  margin-right: 20px;
}
.shop-name {
  font-weight: bold;
}
.shop-title {
  color: #999;
}
.shop-tag {
  font-size: 20px;
}
.item-content {
  display: flex;
  flex: 1;
  gap: 20px;
}
.item-image {
  width: 100px;
  height: 100px;
}
.item-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 4px;
}
.item-info {
  flex: 1;
}
.item-title {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 10px;
}
.item-attrs {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
  color: #666;
}
.item-promos {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}
.promo-tag {
  padding: 2px 5px;
  background-color: #fff8e6;
  color: #ff5000;
  border-radius: 2px;
  font-size: 12px;
}
.item-price {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 5px;
}
.price-info {
  display: flex;
  align-items: center;
  gap: 5px;
}
.price-label {
  color: #999;
}
.price-value {
  font-weight: bold;
  color: #ff5000;
}
.original-price {
  text-decoration: line-through;
  color: #999;
}
.add-price {
  color: #ff5000;
  font-size: 12px;
}
.item-quantity {
  display: flex;
  align-items: center;
  gap: 5px;
}
.quantity-btn {
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 2px;
  cursor: pointer;
}
.quantity-input {
  width: 40px;
  text-align: center;
  border: 1px solid #ddd;
  border-radius: 2px;
}
.item-actions {
  display: flex;
  flex-direction: column;
  gap: 5px;
}
.action-link {
  color: #666;
  cursor: pointer;
}
.action-link:hover {
  color: #ff5000;
}
</style>