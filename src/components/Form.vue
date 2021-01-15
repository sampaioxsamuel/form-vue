<template>
  <form id="form" :class="[classNameForm, { formError: error }]" @submit.prevent>
    <div v-for="{ type, id, mask, selectOptions, icon, className, label, placeholder} in options"
        :key="id" class="form-group">
        
      <template v-if="type !== 'select'">
        <input
          :type="type"
          :id="id"
          :name="id"
          v-mask="mask"
          :class="[className, { 'form-input-custom': icon, 'form-control': type !== 'checkbox' }]"
          :placeholder="placeholder"
          @change="handleFormValues"
        />

        <i v-if="icon" class="fas inputIcon" :class="icon"></i>
        <label v-if="type == 'checkbox'" class="text-secondary-login" :for="label">{{ label }}</label>
      </template>

      <template v-else>
        <select class="custom-select" @change="handleFormValues" :id="id">
          <option selected disabled>{{ placeholder }}</option>
          <option v-for="{ value, label } in selectOptions" :key="value" :value="value"> {{ label }} </option>
        </select>
      </template>
    </div>

    <slot name="button"></slot>

  </form>
</template>

<script>
import { mask } from "vue-the-mask";

export default {
  directives: {
    mask: (el, binding) => {
      if (!binding.value) return;
      mask(el, binding);
    },
  },
  props: {
    options: {
      type: Array,
      required: true,
    },
    error: {
      type: Boolean,
    },
    classNameForm: {},
    className: {},
  },
  data() {
    return {
      form: [],
    };
  },
  methods: {
    handleFormValues({ target }) {
      const { type, id, checked, value } = target;

      if (type == "checkbox") {
        this.form[id] = { id, value: checked };
      } else {
        this.form[id] = { id: id, value: value };
      }
      console.log(this.form);
      this.$emit("data", this.form);
    },
  },
};
</script>

<style lang="scss" scoped>

$error-color: #EA002A;
$lightcolor: #F2F2F2;
$unfocused-color: #a4a4a4;

.text-error-form {
  color: $error-color !important;
}

#form div {
  position: relative;
}

.form-input-custom {
  padding: 0 2.4rem;
  width: 100%;
  min-height: 100%;
}

.inputIcon {
  position: absolute;
  left: 1%;
  top: 25%;
  transition: 0.2s;
}


.formError {
  color: $error-color;

  input {
    border: 1px solid $error-color;
  }

  input:hover, input:focus {
    color: $error-color ;
    border: 1px solid $error-color;
  }
  input:hover + *, input:focus + *, input + * {
    color: $error-color !important;
  }
}

</style>
