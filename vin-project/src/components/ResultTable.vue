<script setup lang="ts">
import { VinDecoder } from '../communication/types'
interface Props {
  vinDecoder: VinDecoder
}
const props = defineProps<Props>()
</script>
<template>
  <div class="table_wrapper" >
    <el-table :data="[props.vinDecoder]" border>
      <el-table-column prop="value" label="Vin" width="" />
      <el-table-column prop="" label="KTYPNR" width="" />
      <el-table-column prop="" label="CONFIDENCE" />
    </el-table> 
  </div> 

</template>

<style>
.table_wrapper{
  margin: 40px;
  max-width: 80vw;
  height: auto;
}

.el-table .cell {
  text-align: center;
}

.el-table th.el-table__cell {
  background-color: var(--el-color-info-light-9);
}
</style>
