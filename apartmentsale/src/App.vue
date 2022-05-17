<template>
  <div class="test-area">
    <div class="apartlist">
      <ul class="apart_list">
        <li class="thead">
          <strong class="price">거래금액</strong>
          <strong class="year">건축년도</strong>
          <strong class="roadName">도로명</strong>
          <strong class="size">전용면적</strong>
          <strong class="floor">층수</strong>
          <strong class="brooker">중개사소재지</strong>
        </li>
        <li v-if="items.length === 0">아파트 정보가 없습니다.</li>
        <li v-for="(item, i) in items" :key="i">
          <div class="price">{{ item.거래금액 }}</div>
          <div class="year">{{ item.건축년도 }}</div>
          <div class="roadName">{{ item.도로명 }}</div>
          <div class="size">{{ item.전용면적 }}</div>
          <div class="floor">{{ item.층 }}</div>
          <div class="brooker">{{ item.중개사소재지 }}</div>
        </li>
      </ul>
    </div>
    <div class="apartArea">
      <GoogleMap
        api-key="AIzaSyC7gZadc_Ii_L7aW_U-vOAiYJ-_yDYY4ig"
        style="width: 100%; height: 500px"
        :center="center"
        :zoom="15"
      >
        <Marker :options="markerOptions" />
      </GoogleMap>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { defineComponent } from "vue";
import { GoogleMap, Marker } from "vue3-google-map";
export default defineComponent({
  name: "App",
  data() {
    return {
      items: [],
    };
  },
  components: { GoogleMap, Marker },
  setup() {
    const center = { lat: 40.689247, lng: -74.044502 };
    const markerOptions = {
      position: center,
      label: "L",
      title: "LADY LIBERTY",
    };

    return { center, markerOptions };
  },
  async mounted() {
    await this.getApartList();
  },
  methods: {
    async getApartList() {
      // 인증키 에러로 인해 추후 수정 예정
      // const serviceURl =
      //   "http://openapi.molit.go.kr/OpenAPI_ToolInstallPackage/service/rest/RTMSOBJSvc/getRTMSDataSvcAptTradeDev";
      // // const EncodingKey =
      // //   "v7Q%2BHMf84a7hq3VskJsc1k99ULmz%2BPyh%2FqQ3ZSF5hnd7a4PHgWt6EKK10%2Bd2AlgZYhSTLXs1d9q827VtJXf%2FZA%3D%3D";
      // // const DecodingKey =
      // //   "v7Q+HMf84a7hq3VskJsc1k99ULmz+Pyh/qQ3ZSF5hnd7a4PHgWt6EKK10+d2AlgZYhSTLXs1d9q827VtJXf/ZA==";
      // const serveiceKey = "v7Q%2BHMf84a7hq3VskJsc1k99ULmz%2BPyh%2FqQ3ZSF5hnd7a4PHgWt6EKK10%2Bd2AlgZYhSTLXs1d9q827VtJXf%2FZA%3D%3D"

      // let params = {
      //   serviceKey: serveiceKey,
      //   LAWD_CD: "11110",
      //   DEAL_YMD: "201512",
      // };

      // await axios.get(serviceURl, {
      //     params,
      //   })
      //   .then((response) => {
      //     console.log(response);
      //     var xml = response.data;
      //   });

      // await axios.get('http://openapi.molit.go.kr/OpenAPI_ToolInstallPackage/service/rest/RTMSOBJSvc/getRTMSDataSvcAptTradeDev?serviceKey=v7Q%2BHMf84a7hq3VskJsc1k99ULmz%2BPyh%2FqQ3ZSF5hnd7a4PHgWt6EKK10%2Bd2AlgZYhSTLXs1d9q827VtJXf%2FZA%3D%3D&pageNo=1&numOfRows=10&LAWD_CD=11110&DEAL_YMD=201512')
      await axios
        .get(
          "http://openapi.molit.go.kr/OpenAPI_ToolInstallPackage/service/rest/RTMSOBJSvc/getRTMSDataSvcAptTradeDev?serviceKey=v7Q%2BHMf84a7hq3VskJsc1k99ULmz%2BPyh%2FqQ3ZSF5hnd7a4PHgWt6EKK10%2Bd2AlgZYhSTLXs1d9q827VtJXf%2FZA%3D%3D&numOfRows=20&LAWD_CD=11110&DEAL_YMD=201512"
        )
        .then((ele) => {
          ele.data.response.body.items.item.forEach((e) => {
            this.items.push(e);
          });
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
});
</script>

<style scoped>
.apart_list {
  position: relative;
}
.apart_list > li {
  display: table;
  width: 50%;
  border-bottom: 1px solid #ebebeb;
  table-layout: fixed;
  text-align: center;
}
.apart_list > li.thead {
  background-color: #f5f5f5;
}
.apart_list > li > div,
.apart_list > li > strong {
  display: table-cell;
  padding: 10px;
}
.apart_list > li > .brooker {
  width: 130px;
}
</style>
