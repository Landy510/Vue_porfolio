<template>
  <div>
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb bg-transparent border border-bottom">
        <li class="breadcrumb-item">
          <router-link to="/" class="text-muted">首頁</router-link>
        </li>
        <li class="breadcrumb-item active text-dark" aria-current="page">門市一覽</li>
      </ol>
    </nav>
    <div class="container-fluid">
      <div class="row">
      <div class="col-sm-4">
        <div class="form-group">
          <select class="form-control rounded-0" v-model="selectlocation" @change="PenTo(selectlocation)">
            <option value="" disabled>請選擇任一間門市</option>
            <option v-for="(item, key) in data" :key="key" :value="item">{{ item["場地名稱"] }}</option>
          </select>
        </div>
        <div>
          <h5 class="font-weight-bold">
            {{ selectlocation["場地名稱"] }}
          </h5>
          <small class="text-muted small-text">{{ selectlocation["地址"] }}</small>
          <br>
          <h5 class="mt-3" v-if="selectlocation.decription!==''">{{ selectlocation.decription }}</h5>
          <img :src="selectlocation.image" :alt="selectlocation['場地名稱']" class="img-thumbnail rounded-0" v-if="selectlocation.image!==''">
        </div>
      </div>
      <div class="col-sm-8">
        <div id="map" class="map-setting"></div>
      </div>
    </div>
    </div>
  </div>
</template>

<script>
import L from 'leaflet'
let osmMap = {}
const storeTile = ['Berserker 美麗華店', 'Berserker 南西店', 'Berserker 西門漢中店', 'Berserker 台中都會店', 'Berserker 佳瑪店', 'Berserker 大江購物店', 'Berserker 家樂福店', 'Berserker 勤美成品店']
const storeImage = ['https://i.postimg.cc/FHQ5LrTf/location1.jpg', 'https://i.postimg.cc/Kc6v0y9b/location2.jpg', 'https://i.postimg.cc/C1ZS4vkd/location3.jpg', 'https://i.postimg.cc/sXZCWMCh/location4.jpg',
  'https://i.postimg.cc/wBCbSFn1/location5.jpg', 'https://i.postimg.cc/L4Cj5h6T/location6.jpg', 'https://i.postimg.cc/SRJ2C7NG/location7.jpg', 'https://i.postimg.cc/d0wh4KV3/location8.jpg']
const storedecription = ['本店有提供飲食控制課程', '本店有提供最全面的重訓教學', '本店可以購買到各種重訓護具', '本店有提供大量的自由重量的訓練器材', '本店主要提供有氧的一些器材', '本店提供阻力訓練還有暴力上槓',
  '本店有提供讓各方健友一起聚聚', '本店主要是讓大家打打籃球和羽球啦']

export default {
  name: 'LocationInfo',
  data () {
    return {
      product_length: 0,
      data: [],
      selectlocation: '',
      isLoading: false
    }
  },
  methods: {
    CounterCoupute (cartTotalLength) {
      this.product_length = cartTotalLength
    },
    updateMap () {
      this.data.forEach(item => {
        L.marker([item['北緯'], item['東經']]).addTo(osmMap).bindPopup(`<strong>據點名稱:${item['場地名稱']}</strong><br>
          <strong>地址:<a href="https://www.google.com.tw/maps/place/${item['地址']}" target="_blank">${item['地址']}</a></strong><br>
          <strong>電話:${item['電話']}</strong>`)
      })
      osmMap.panTo(new L.LatLng(24.130993, 120.721122))
      L.marker([24.130993, 120.721122]).addTo(osmMap).bindPopup(`<strong>據點名稱:Berserker 南西店</strong><br>
          <strong>地址:<a href="https://www.google.com.tw/maps/place/臺中市太平區中興東路99號" target="_blank">臺中市太平區中興東路99號</a></strong><br>
          <strong>電話:04-22289111#51311</strong>`).openPopup()
      this.$store.state.isLoading = false
    },
    PenTo (location) {
      osmMap.panTo(new L.LatLng(location['北緯'], location['東經']))
      L.marker([location['北緯'], location['東經']]).addTo(osmMap).bindPopup(`<strong>據點名稱:${location['場地名稱']}</strong><br>
          <strong>地址:<a href="https://www.google.com.tw/maps/place/${location['地址']}" target="_blank">${location['地址']}</a></strong><br>
          <strong>電話:${location['電話']}</strong>`).openPopup()
    }
  },
  mounted () {
    const vm = this
    vm.axios.defaults.withCredentials = false
    const api = ' https://datacenter.taichung.gov.tw/swagger/OpenData/c60986c5-03fb-49b9-b24f-6656e1de02aa'
    vm.$store.state.isLoading = true
    vm.$http.get(api).then((Response) => {
      vm.data = Response.data.slice(0, 8)
      for (let i = 0; i !== vm.data.length; i++) {
        vm.data[i].image = storeImage[i]
        vm.data[i]['場地名稱'] = storeTile[i]
        vm.data[i].decription = storedecription[i]
      }
      vm.updateMap()
    })
    osmMap = L.map('map', {
      center: [24.130993, 120.721122],
      zoom: 10
    })
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '<a target="_blank" href="https://www.openstreetmap.org/">© OpenStreetMap 貢獻者</a>',
      maxZoom: 18
    }).addTo(osmMap)
  },
  create () {
    this.getCart()
  }
}
</script>

<style scoped>
#map {
  height: 100vh;
}
.small-text{
  font-size: 14px;
}
.map-setting {
  z-index: 2;
}
</style>
