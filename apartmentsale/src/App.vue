<template>
  <div class="test-area">
    <div class="apartlist">
      <ul class="apart_list">
        <li class="thead">
          <strong>거래금액</strong>
          <strong>건축년도</strong>
          <strong>도로명</strong>
          <strong>전용면적</strong>
          <strong>층수</strong>
          <strong>중개사소재지</strong>
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
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  components: {},
  data() {
    return {
      items: [],
    };
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
};
</script>

<style></style>
