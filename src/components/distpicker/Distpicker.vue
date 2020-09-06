<template>
  <div :class="wrapper">
    <el-col :span="hideArea ? 12: 8">
      <el-select
        @change="resetCities"
        v-model="currentProvinceCode"
        :disabled="disabled || provinceDisabled"
        placeholder="省"
        style="padding: 0px 5px;">
        <el-option value="--" disabled>
          <div class="text-center" style="width:100px; float: left;margin-left: -17px;">省份编号</div>
          <div class="text-center" style="width:100px; float: left;">省份简拼</div>
          <div class="text-center" style="width:200px; float: left;">省份名称</div>
        </el-option>
        <el-option
          v-for="(item, index) in provinces"
          :value="item.code"
          :label="item.name"
          :key="item.py+index">
          <div class="text-center" style="width:100px; float: left;margin-left: -17px;">{{item.code}}</div>
          <div class="text-center" style="width:100px; float: left;">{{item.py}}</div>
          <div class="text-center" style="width:200px; float: left;">{{item.name}}</div>
        </el-option>
      </el-select>
    </el-col>
    <template v-if="!onlyProvince">
      <el-col :span="hideArea ? 12: 9">
        <el-select
          @change="resetAreas"
          v-model="currentCityCode"
          :disabled="disabled || cityDisabled"
          placeholder="市"
          style="padding-right: 5px;">
          <el-option value="--" disabled>
            <div class="text-center" style="width:100px; float: left;margin-left: -17px;">城市编号</div>
            <div class="text-center" style="width:100px; float: left;">城市简拼</div>
            <div class="text-center" style="width:200px; float: left;">城市名称</div>
          </el-option>
          <el-option
            v-for="(item, index) in cities"
            :value="item.code"
            :label="item.name"
            :key="item.py+index"
            style="width: 460px;">
            <div class="text-center" style="width:100px; float: left;margin-left: -17px;">{{item.code}}</div>
            <div class="text-center" style="width:115px; float: left;">{{item.py}}</div>
            <div class="text-center" style="width:220px; float: left;">{{item.name}}</div>
          </el-option>
        </el-select>
      </el-col>
      <el-col :span="7" v-if="!hideArea">
        <el-select
          @change="onEmitArea"
          v-model="currentAreaCode"
          :disabled="disabled || areaDisabled"
          placeholder="区/县">
          <el-option value="--" disabled>
            <div class="text-center" style="width:100px; float: left;margin-left: -17px;">区县编号</div>
            <div class="text-center" style="width:100px; float: left;">区县简拼</div>
            <div class="text-center" style="width:200px; float: left;">区县名称</div>
          </el-option>
          <el-option
            v-for="(item, index) in areas"
            :value="item.code"
            :label="item.name"
            :key="item.py+index">
            <div class="text-center" style="width:100px; float: left;margin-left: -17px;">{{item.code}}</div>
            <div class="text-center" style="width:100px; float: left;">{{item.py}}</div>
            <div class="text-center" style="width:200px; float: left;">{{item.name}}</div>
          </el-option>
        </el-select>
      </el-col>
    </template>
  </div>
</template>

<script>
// 异步获取地址数据js
// import api from "@/http"
import dataSource from './provinces'
console.log(dataSource)
/**
  province
  city
  area
  三个入参代扩展
 */
export default {
  name: 'v-distpicker',
  props: {
    provinceCode: {
      type: String,
      default: ''
    },
    province: {
      type: Object,
      default() {
        return {}
      }
    },
    cityCode: {
      type: String,
      default: ''
    },
    city: {
      type: Object,
      default() {
        return {}
      }
    },
    countryCode: {
      type: String,
      default: ''
    },
    area: {
      type: Object,
      default() {
        return {}
      }
    },
    hideArea: {
      type: Boolean,
      default: false
    },
    onlyProvince: {
      type: Boolean,
      default: false
    },
    placeholders: {
      type: Object,
      default() {
        return {
          province: {
            code: "",
            name: "省",
            py: "",
            cities: []
          },
          city: {
            code: "",
            name: "市",
            py: "",
            districts: []
          },
          area: {
            code: "",
            name: "区/县",
            py: "",
            zipcode: ""
          }
        }
      }
    },
    disabled: {
      type: Boolean,
      default: false
    },
    provinceDisabled: {
      type: Boolean,
      default: false
    },
    cityDisabled: {
      type: Boolean,
      default: false
    },
    areaDisabled: {
      type: Boolean,
      default: false
    },
    wrapper: {
      type: String,
      default: 'distpicker-address-wrapper'
    }
  },
  data() {
    /*
    * 异步获取地址js数据时data数据
    return {
      provinces: [],
      cities: [],
      areas: [],
      currentProvinceCode: this.placeholders.province.code,
      currentProvince: this.placeholders.province,
      currentCityCode: this.placeholders.city.code,
      currentCity: this.placeholders.city,
      currentAreaCode: this.placeholders.area.code,
      currentArea: this.placeholders.area,
    }*/
    // provinces数据在当前组件目录时
    console.log(this.getProvinceByCode(this.provinceCode) || this.placeholders.province)
    return {
      provinces: dataSource,
      cities: [],
      areas: [],
      currentProvinceCode: this.provinceCode,
      currentProvince: this.getProvinceByCode(this.provinceCode) || this.placeholders.province,
      currentCityCode: this.cityCode,
      currentCity: this.getCityByCode(this.cityCode) || this.placeholders.city,
      currentAreaCode: this.countryCode,
      currentArea: this.getAreaByCode(this.countryCode) || this.placeholders.area
    }
  },
  created() {
    // this.initProvinces() 异步获取地址数据时用到
  },
  watch: {
    /*currentProvince(value) {
      this.$emit('province', value)
      // if (this.onlyProvince) this.emit('selected')
      this.emit('selected')
    },
    currentCity(value) {
      this.$emit('city', value)
      if (value != this.placeholders.city && this.hideArea) this.emit('selected')
      this.emit('selected')
    },
    currentAreaCode(value) {
      this.currentArea = this.getAreaByCode(value)
      this.$emit('area', this.currentArea)
      this.emit('selected')
    },*/
    provinceCode(value) {
      if(this.provinces.length === 0) {
        return
      }
      this.currentProvinceCode = value
      this.currentProvince = this.getProvinceByCode(value) || this.placeholders.province
      this.cities = this.currentProvince.cities
    },
    cityCode(value) {
      if(this.provinces.length === 0) {
        return
      }
      this.currentCityCode = value
      this.currentCity = this.getCityByCode(value) || this.placeholders.city
      this.areas = this.currentCity.districts
    },
    countryCode(value) {
      if(this.provinces.length === 0) {
        return
      }
      this.currentAreaCode = value
      this.currentArea = this.getAreaByCode(value) || this.placeholders.area
    }
  },
  methods: {
    initProvinces() {
      api.resource.getProvinces().then( res => {
        this.provinces = res
        this.currentProvinceCode = this.provinceCode
        this.currentProvince = this.getProvinceByCode(this.provinceCode) || this.placeholders.province
        this.currentCityCode = this.cityCode
        this.cities = this.currentProvince.cities
        this.currentCity = this.getCityByCode(this.cityCode) || this.placeholders.city
        this.currentAreaCode = this.countryCode
        this.areas = this.currentCity.districts
        this.currentArea = this.getAreaByCode(this.countryCode) || this.placeholders.area
      })
    },
    getProvinceByCode(code) {      
      let province
      if(!code) {
        return;
      }
      this.provinces.forEach((item, index) => {
        if(item.code === code) {
          province = item;
        }
      });
      return province;
    },
    getCityByCode(code) {
      let city
      if(!code) {
        return;
      }
      let isFound = false
      this.provinces.some((item, index) => {
        item.cities.some( (cityItem, cityIndex) => {
          if(cityItem.code === code) {
            city = cityItem
            isFound = true
            return true
          }
        })
        if(isFound) {
          return true
        }
      });
      return city;
    },
    getAreaByCode(code) {
      let area
      if(!code) {
        return;
      }
      let isFound = false
      this.provinces.some((item, index) => {
        item.cities.some( (cityItem, cityIndex) => {
          cityItem.districts.some((districtItem, districtIndex) => {
            if(districtItem.code === code) {
              area = districtItem
              isFound = true
              return true
            }
          })
          if(isFound) {
            return true
          }
        })
        if(isFound) {
          return true
        }
      });
      return area;
    },
    emit(name) {
      let data = {
        province: this.currentProvince
      }

      if (!this.onlyProvince) {
        this.$set(data, 'city', this.currentCity)
      }

      if (!this.onlyProvince || this.hideArea) {
        this.$set(data, 'area', this.currentArea)
      }

      this.$emit(name, data)
    },
    resetCities() {
      this.currentProvince = this.getProvinceByCode(this.currentProvinceCode)
      this.cities = this.currentProvince.cities
      this.currentCity = this.placeholders.city
      this.currentCityCode = ""
      this.currentArea = this.placeholders.area
      this.currentAreaCode = ""
      this.areas = []
      // if (this.cities.length === 0) {
        this.emit('selected')
      // }
    },
    resetAreas() {
      this.currentCity = this.getCityByCode(this.currentCityCode)
      this.currentArea = this.placeholders.area
      this.currentAreaCode = ""
      this.areas = this.currentCity.districts
      // if (this.areas.length === 0) {
        this.emit('selected')
      // }
    },
    onEmitArea(value) {
      this.currentArea = this.getAreaByCode(value)
      this.$emit('area', this.currentArea)
      this.emit('selected')
    }
  }
}
</script>

<style lang="less">
.distpicker-address-wrapper {
  color: #9caebf;
  ul {
    margin: 0;
    padding: 0;

    li {
      list-style: none;
    }
  }

}
</style>
