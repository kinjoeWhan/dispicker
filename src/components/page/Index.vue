<template>
  <div class="demo">
    <h3>三级地址demo</h3>
    <div class="demo1">
      <h4 class="title">栗子一：</h4>
      <div class="picker">
        <district-picker></district-picker>
      </div>
    </div>
  </div>
</template>
<script>
import DistrictPicker from '@/components/distpicker/Distpicker'
export default {
  name: "Index",
  data() {
    return {

    }
  },
  components: {
    'district-picker': DistrictPicker
  }
};
</script>
<style lang="less" scoped>
.demo {
  text-align: left;
  max-width: 980px;
  margin: 0 auto;

  .picker{
    max-width: 800px;
  }
}
</style>