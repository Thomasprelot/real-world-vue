<template>
  <div v-on="$listeners">
    <label v-if="label">{{ label }}</label>
    <select
      v-on="$listeners"
      type="text"
      :value="value"
      @change="updateValue"
      v-bind="$attrs"
    >
      <option
        v-for="option in options"
        :value="option"
        :key="option.id"
        :selected="option === value"
        >{{ option }}
      </option>
    </select>
  </div>
</template>

<script>
import { formFieldMixin } from '@/mixins/formFieldMixin.js'
export default {
  mixins: [formFieldMixin],
  props: {
    options: {
      type: Array,
      required: true
    }
  }
}
</script>

<style lang="scss" scoped></style>
