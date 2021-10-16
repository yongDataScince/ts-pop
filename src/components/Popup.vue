<template>
  <div class="popup">
    <div class="popup-container">
      <button class="close-btn">
        <i class="icon icon-close"></i>
      </button>
      <div class="popup__title">Налоговый вычет</div>
      <div class="popup__text">
        Используйте налоговый вычет чтобы погасить ипотеку досрочно. 
        Размер налогового вычета составляет не более 13% от своего официального годового дохода.
      </div>
      <div class="input-group">
        <label class="input-label" for="price">Ваша зарплата в месяц</label>
        <input type="text" class="input" id="price" name="price" v-model="price">
        <button class="text-btn" @click="showComputes">
          Рассчитать
        </button>
      </div>
      <div class="computed-block" :class="{ show }">
        <div class="computed-block__title">Итого можете внести в качестве досрочных:</div>
        <ul class="computed-block__list">
          <li class="computed-block__list--item">
            <input type="checkbox" name="check" class="input-checkbox">
            <label for="check">
              78 000 рублей 
              <span>в 1-ый год</span>
            </label>
          </li>
        </ul>
      </div>
      <div class="choise-group">
        <div class="choise-group__text">
          Что уменьшаем?
        </div>
        <button class="tag"
                :class="{ active: tag.isActive }"
                v-for="tag in tags"
                :key="tag.id"
                @click="choiseTag(tag.id)">
          {{ tag.text }}
        </button>
      </div>
      <button class="btn big-btn normal">
        Добавить
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    tags: [
      {id: 0, isActive: true, text: "Платёж"},
      {id: 1, isActive: false, text: "Срок"},
    ],
    price: "",
    show: false
  }),
  methods: {
    choiseTag(id) {
      this.tags.map(t => {
        if(t.id === id) t.isActive = true
        else t.isActive = false
        t
      })
    },
    showComputes() {
      this.show = true
    }
  },
  watch: {
    price(e) {
      this.price = e.replace(/[^\d]+/g,'').replace(/\B(?=(\d{3})+(?!\d))/g, " ")
      // "₽"
    }
  }
}
</script>

<style lang="scss" scoped>
  .input-group {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-start;
    margin-bottom: 24px;
    .input {
      margin-bottom: 8px;
    }
  }
  .choise-group {
    width: 100%;
    height: 36px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    margin-bottom: 40px;
    &__text {
      font-style: normal;
      font-weight: 500;
      font-size: 14px;
      line-height: 24px;
      color: #000000;
      margin-right: 32px;
    }
    .tag {
      margin-right: 16px;
      border-radius: 50px;
    }
  }
  .computed-block {
    height: 100%;
    overflow: hidden;
    transition: .2s;
    max-height: 0;

    &__title {
      font-weight: 500;
      font-size: 14px;
      line-height: 24px;
      margin-bottom: 16px;
    }

    &__list {
      width: 100%;
      padding: 0;
      margin: 0;
      list-style: none;
    }

    &.show {
      max-height: 200px;
    }
  }
  .btn {
    width: 100%;
  }
</style>