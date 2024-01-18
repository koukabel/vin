<script lang="ts" setup>
import Form from '@/components/Form.vue';
import ResultTable from '@/components/ResultTable.vue';
import type { VinDecoder } from '../communication/types';
import { computed, reactive, ref } from 'vue'
import { FormInstance } from 'element-plus'

const VinDecoder = reactive<VinDecoder>({ value: '', threshold: 0 })

const displayResultKtypynr = ref(false);

function handleFormSumission(formValue) {
  displayResultKtypynr.value = true
  console.log('Form submitted:', formValue)
}


const handleFormReset = async (formEl: FormInstance) => {
  if (!formEl) return;
  formEl.resetFields();
  displayResultKtypynr.value = false;
};

</script>

<template>
  <el-container>
    <el-header>
      <h3 class="title">🍷 VIN decoder 🚗</h3>
    </el-header>
    <el-main>
      <Form @submit="handleFormSumission" :vinDecoder="VinDecoder" @reset="handleFormReset" />
      <!-- pass the vin to this child -->
      <template v-if="displayResultKtypynr">
        <ResultTable :vinDecoder="VinDecoder" />
      </template>
    </el-main>
  </el-container>
</template>

<style>
.title {
  font-size: 2.2rem;
  font-family: monospace;
  text-align: center;
}
</style>