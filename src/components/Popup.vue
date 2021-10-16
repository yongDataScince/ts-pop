<template>
  <div class="popup"
       :style="`max-height: ${476 + 60 * checks.length}px;`">
    <div class="popup-container">
      <button class="close-btn" @click="close">
        <i class="icon icon-close"></i>
      </button>
      <div class="popup__title">Налоговый вычет</div>
      <div class="popup__text">
        Используйте налоговый вычет чтобы погасить ипотеку досрочно. 
        Размер налогового вычета составляет не более 13% от своего официального годового дохода.
      </div>
      <div class="input-group">
        <Input v-model="price"
               label="Ваша зарплата в месяц"
               errorMessage="Поле обязательно для заполнения"
               name="price"
               />
        <button class="text-btn" 
                @click="computeTax">
          Рассчитать
        </button>
      </div>
      <div class="computed-block"
           :style="`max-height: ${60 * checks.length}px;`">

        <div class="computed-block__title">Итого можете внести в качестве досрочных:</div>
        <ul class="computed-block__list">
          <li class="computed-block__list--item" 
              v-for="(c, n) in checks"
              :key="n">
            <input type="checkbox"
                   :name="`check_${n}`" 
                   :id="`check_${n}`"
                   class="input-checkbox">
            <label :for="`check_${n}`" class="checkbox-label">
              {{ c }} рублей 
              <span class="check-span">в {{ n + 1 }}-{{ declOfNum(n+1) }} год</span>
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
import Input from "./Input.vue"
export default {
  data: () => ({
    tags: [
      {id: 0, isActive: true, text: "Платёж"},
      {id: 1, isActive: false, text: "Срок"},
    ],
    price: "",
    show: false,
    checks: []
  }),
  methods: {
    choiseTag(id) {
      this.tags.map(t => {
        if(t.id === id) t.isActive = true
        else t.isActive = false
        t
      })
    },
    computeTax() {
      if(this.price == "") this.$children[0].$refs.inp.classList.add('error')
      else {
        this.show = false
        this.checks = []
        let formatPrice = this.price.replace(" ", "")
        let pricePerYear = 12 * formatPrice
        let taxPerYear = 0.13 * pricePerYear 
        let sum = 0
        while (260_000 - sum > taxPerYear) {
          sum += taxPerYear
          this.checks.push(taxPerYear)
        }
        this.checks.push(260_000 - sum)
        this.show = true
      }
    },
    close() {
      this.$emit('close')
    },
    declOfNum(number) {  
      let cases = [2, 0, 1, 1, 0, 1];
      let end = number % 100 == 5 ? "ый" : 
                ["ый", "ой", "ий", "ый"][ (number%100==3) ? 2 : cases[number%100<5 ? number%100 : 5] ]
      return end
    }
  },
  watch: {
    price(e) {
      if (e != "") this.$children[0].$refs.inp.classList.remove('error')
    }
  },
  components: {
    Input
  }
}
</script>

<style lang="scss" scoped>
  .input-group {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-start;
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
    margin-top: 20px;
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
      // margin-bottom: 16px;
    }

    &__list {
      width: 100%;
      padding: 0;
      margin: 0;
      list-style: none;
      &--item {
        width: 100%;
        padding: 16px 0;
        border-bottom: 1px solid #DFE3E6;
      }
    }

    &.show {
      max-height: 250px;
    }
  }
  .btn {
    margin-top: 16px;
    width: 100%;
  }
</style>