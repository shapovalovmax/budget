<template>
  <ElCard class="form-card">
    <ElForm :model="formData" ref="addItemForm" :rules="rules" label-position="top">
      <ElFormItem label="Type" prop="type">
        <ElSelect class="type-select" v-model="formData.type" @change="onTypeChange" placeholder="Choose type...">
          <ElOption label="Income" value="INCOME"/>
          <ElOption label="Outcome" value="OUTCOME"/>
        </ElSelect>
      </ElFormItem>
      <ElFormItem label="Comments" prop="comment">
        <ElInput v-model="formData.comment"/>
      </ElFormItem>
      <ElFormItem label="Value" prop="value">
        <ElInput v-model.number="formData.value" @change="onValueInput"/>
      </ElFormItem>
      <ElButton @click="onSubmit" type="primary">Submit</ElButton>
    </ElForm>
  </ElCard>
</template>
<script>
export default {
  name: 'TheForm',
  data: () => ({
    formData: {
      type: 'INCOME',
      comment: '',
      value: 0,
    },
    rules: {
      type: [{required: true, message: 'Please select type', trigger: 'blur'}],
      comment: [{required: true, message: 'Please input comment', trigger: 'change'}],
      value: [
        {required: true, message: 'Please input value', trigger: 'change'},
        {type: 'number', message: 'Value must be a number', trigger: 'change'},
        {
          validator: (rule, value, callback) => {
            if (value === 0) {
              callback(new Error('Value cannot be 0'));
            } else {
              callback();
            }
          },
          trigger: 'change'
        }
      ],
    },
  }),
  methods: {
    onSubmit() {
      this.$refs.addItemForm.validate((valid) => {
        if (valid) {
          this.$emit('submitForm', {...this.formData});
          this.$refs.addItemForm.resetFields();
        }
      });
    },
    onTypeChange(value) {
      if (value === 'OUTCOME') {
        this.formData.value = -Math.abs(this.formData.value);
      } else {
        this.formData.value = Math.abs(this.formData.value);
      }
    },
    onValueInput(value) {
      if (this.formData.type === 'OUTCOME') {
        this.formData.value = -Math.abs(value);
      }
    },
  },
}
</script>
<style scoped>
.form-card {
  max-width: 500px;
  margin: auto;
}

.type-select {
  width: 100%;
}
</style>