<template>
  <div class="container">
    <div v-for="category in categories"
         :key="category.id"
         @drop="onDrop($event, category.id)"
         :style="category.color"
         class="droppable"
         @dragover.prevent
         @dragenter.prevent>
      <h4>{{ category.title }}</h4>
      <div v-for="item in items.filter(x => x.categoryId === category.id)"
           @dragstart="onDragStart($event, item)"
           class="draggable"
           draggable="true">
        <h5>{{ item.title }}</h5>
      </div>
    </div>

  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'App',
  setup() {
    const items = ref([
      {
        id: 0,
        title: 'Сделать страницу регистрации/авторизации',
        categoryId: 0
      },
      {
        id: 1,
        title: 'Переделать все страницы согласно макета и правкам заказчика',
        categoryId: 0
      },
      {
        id: 2,
        title: 'Изменить стили кнопок во всем проекте',
        categoryId: 1
      },
    ])
    const categories = ref([
      {
        id: 0,
        title: 'Ожидают выполнения',
        color: "background: #B89C50"
      },
      {
        id: 1,
        title: 'В работе',
        color: "background: #D14152"
      },
      {
        id: 2,
        title: 'Выполненные',
        color: "background: #6A93B0"
      },
    ])

    function onDragStart(e, item) {
      e.dataTransfer.dropEffect = 'move'
      e.dataTransfer.effectAllowed = 'move'
      e.dataTransfer.setData('itemId', item.id.toString())
    }

    function onDrop(e, categoryId) {
      const itemId = parseInt(e.dataTransfer.getData('itemId'))
      items.value = items.value.map(x => {
        if (x.id == itemId)
          x.categoryId = categoryId
        return x
      })
    }

    return {
      items,
      categories,
      onDragStart,
      onDrop
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
}
.droppable {
  padding: 15px;
  border-radius: 5px;
  background: #2c3e50;
  margin: 10px;
  min-width: 250px;
}

.droppable h4 {
  color: white;
}

.draggable {
  background: white;
  padding: 5px;
  border-radius: 5px;
  margin-bottom: 5px;
  width: 250px
}

.draggable h5 {
  margin: 0;
}
.form {
  text-align: left;
    padding: 15px;
}
.btn {
  background: #48A43F
}
</style>