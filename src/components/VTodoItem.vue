<template>
  <li class="v-todo-item" :class="{'is-checked': checked}">
    <button @click="sendCheckStatus">{{ checkText }}</button>

    <span>{{ text }}</span>

    <button @click="sendRemove">Supprimer</button>
  </li>
</template>

<script>
import { computed } from 'vue'

export default {
  props: {
    text: {
      type: String,
      required: true
    },
    checked: {
      type: Boolean,
      required: true
    }
  },

  setup (props, { emit }) {
    const sendCheckStatus = () => {
      emit('check')
    }

    const sendRemove = () => {
      emit('remove')
    }

    const checkText = computed(() => {
      return props.checked ? 'Uncheck' : 'Check'
    })

    return {
      sendCheckStatus,
      sendRemove,
      checkText
    }
  }
}
</script>

<style lang="scss" scoped>
.v-todo-item {
  padding: 8px;
  display: flex;
  justify-content: space-between;
  background: white;
  border-radius: 3px;
  background: antiquewhite;

  &.is-checked {
    filter: grayscale(1);

    span {
      text-decoration: line-through;
    }
  }
}
</style>
