<script setup lang="ts">
import { computed, reactive, ref } from 'vue'
import type { VinDecoder } from '../communication/types'
import type { FormInstance, FormRules } from 'element-plus'

interface Props {
  vinDecoder: VinDecoder
}

interface Emits {
  (event: 'submit', value: VinDecoder): void
  (event: 'reset', formEl: FormInstance): void;
}

const props = defineProps<Props>()
const emit = defineEmits<Emits>()

const mode = ref<'advanced' | 'simple'>('simple')

const form = props.vinDecoder
const formRef = ref<FormInstance>()
const rules = reactive<FormRules<VinDecoder>>({
  value: [
    { required: true, message: 'Ce champ ne peut pas être vide', trigger: 'blur' },
    { min:0, max: 17, message: 'Ce champ ne peut pas être vide', trigger: 'blur' }
  ]
})

const isVinValid = computed(() => form.value.length === 17)

const arePowerAndCapacityDisabled = computed(() => !isVinValid.value)

const areDimensionsDisabled = computed(() => !form.power || !form.engine_capacity)

const areSeatsAndCo2Disabled = computed(() => !form.height || !form.width || !form.length)

async function handleValidate() {
  const isFormValid = await formRef.value?.validate()
  if (isFormValid) {
    emit('submit', form)
  }
}

function handleReset(formEl: FormInstance | undefined) {

  emit('reset', formEl);
}
</script>

<template>
  <el-form class="vinForm" ref="formRef" :model="form" :rules="rules">
    <el-radio-group v-model="mode">
      <el-radio-button label="simple"> Mode simple </el-radio-button>
      <el-radio-button label="advanced"> Mode avancé </el-radio-button>
    </el-radio-group>
    <el-form-item class="simple_search" label="VIN" prop="value">
      <el-input
        size="large"
        v-model="form.value"
        required
        minlength="17"
        maxlength="17"
        show-word-limit
      />
    </el-form-item>
    <template v-if="mode === 'advanced'">
      <div class="advanced_search">
        <el-form-item class="div1" label="Power" label-position="top" prop="power">
          <el-input-number
            class="input-item"
            type="number"
            min= 0
            size="large"
            v-model="form.power"
            :controls="false"
            :disabled="arePowerAndCapacityDisabled"
          >
            <!-- <template #prependRight >KW</template>  -->
          </el-input-number>
        </el-form-item>
        <el-form-item class="div2" label="Engine capacity" prop="engine_capacity">
          <el-input-number
            class="input-item"
            type="number"
            min= 0
            size="large"
            v-model="form.engine_capacity"
            :controls="false"
            :disabled="arePowerAndCapacityDisabled"
          />
        </el-form-item>
        <el-form-item class="div3" label="Height" prop="height">
          <el-input-number
            class="input-item"
            type="number"
            min= 0
            size="large"
            v-model="form.height"
            label="height"
            :controls="false"
            :disabled="areDimensionsDisabled"
          />
        </el-form-item>
        <el-form-item class="div4" label="Width" prop="width">
          <el-input-number
            class="input-item"
            type="number"
            min= 0
            size="large"
            v-model="form.width"
            label="width"
            :controls="false"
            :disabled="areDimensionsDisabled"
          />
        </el-form-item>
        <el-form-item class="div5" label="Length" prop="length">
          <el-input-number
            class="input-item"
            type="number"
            min= 0           size="large"
            v-model="form.length"
            label="length"
            :controls="false"
            :disabled="areDimensionsDisabled"
          />
        </el-form-item>
        <el-form-item class="div6" label="Nb seats" prop="Nb_seats">
          <el-input-number
            class="input-item"
            type="number"
            min= 0
            size="large"
            v-model="form.Nb_seats"
            label="Nb_seats"
            :controls="false"
            :disabled="areSeatsAndCo2Disabled"
          />
        </el-form-item>
        <el-form-item class="div7" label="CO2" prop="CO2">
          <el-input-number
            class="input-item"
            type="number"
            min= 0
            size="large"
            v-model="form.CO2"
            label="CO2"
            :controls="false"
            :disabled="areSeatsAndCo2Disabled"
          />
        </el-form-item>
        <el-form-item class="div8" label="Threshold" prop="threshold">
          <el-input-number
            v-model="form.threshold"
            :step="1"
            :max="100"
            :min= 0
            label="threshold"
          />
        </el-form-item>
      </div>
    </template>
    <el-form-item>
      <el-button type="primary" @click="handleValidate">Valider</el-button>
      <el-button @click="handleReset(formRef)">Réinitialiser</el-button>
    </el-form-item>
  </el-form>
</template>

<style>
.form_wrapper {
  /* margin-top: 3vh;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 50px;
  align-items: normal;
  flex-wrap: nowrap; */
}

.vinForm {
  display: flex;
  height: auto;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 30px 30px;
  /* margin: auto; */
  box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
  border-radius: 10px;
  gap: 10px;
}

.advanced_search {
  display: grid;
  grid-template-rows: repeat(6, 1fr);
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
  row-gap: 20px;
  max-height: 28vh;
}

.div1,
.div2 {
  grid-column: span 6;
}

.div3,
.div4,
.div5 {
  grid-column: span 4;
}

.div6,
.div7 {
  grid-column: span 6;
}

.div8 {
  grid-column: span 12;
}
</style>
