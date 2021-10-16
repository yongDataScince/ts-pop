<template>
  <div class="input-container">
    <label class="input-label" :for="name">{{ label }}</label>
    <input type="text" 
           ref="inp"
           class="input"
           :id="name"
           :name="name" 
           @input="$emit('input', $event.target.value)"
           v-model="value"
          />
    <div class="error-message">
      {{ errorMessage }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      required: true
    },
    errorMessage: {
      type: String,
      required: true
    },
    name: {
      type: String,
      required: true
    }
  },
  data: () => ({
    value: ""
  }),
  watch: {
    value(e) {
      this.value = e.replace(/[^\d]+/g,'').replace(/\B(?=(\d{3})+(?!\d))/g, " ")
    }
  }
}
</script>