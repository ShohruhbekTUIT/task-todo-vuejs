<template>
  <li class='site-main__item' :class="[{ checked: item.isCompleted }]">
    <div className='site-main__item-info'>
      <input type='checkbox' class="site-main__item-input" v-model="item.isCompleted" :onChange="[handleCompleted]" />
      <p>{{ item.title }}</p>
    </div>
    <span class="item-time">{{ item.time }}</span>
  </li>
</template>
<script>


export default {
  props: {
    item: {
      typeof: Object,
      required: true,
    },
    handleCompleted: {
      typeof: Object,
      required: true,
    }
  }
}
</script>
<style scoped>
.site-main__item {
  max-width: 500px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.site-main__item-info {
  display: flex;
  align-items: center;
}

.site-main__item-input {
  margin-right: 25px;
}


.checked {
  opacity: 0.6;
  text-decoration: line-through;
}
</style>