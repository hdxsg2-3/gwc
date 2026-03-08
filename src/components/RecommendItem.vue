<template>
  <div class="recommend-item">
    <div class="recommend-image">
      <img :src="recommendation.image" alt="推荐商品">
    </div>
    <div class="recommend-info">
      <div class="recommend-title">{{ recommendation.title }}</div>
      <div class="recommend-price">{{ recommendation.price }}</div>
      <div class="recommend-sales">1000人购买</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RecommendItem',
  props: {
    recommendation: {
      type: Object,
      required: true
    }
  }
};
</script>

<style scoped>
.recommend-item {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
}
.recommend-item:hover {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
.recommend-image {
  width: 100%;
  height: 120px;
}
.recommend-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 4px;
}
.recommend-info {
  display: flex;
  flex-direction: column;
  gap: 5px;
}
.recommend-title {
  font-size: 14px;
  font-weight: bold;
}
.recommend-price {
  color: #ff5000;
  font-weight: bold;
}
.recommend-sales {
  color: #999;
  font-size: 12px;
}
</style>