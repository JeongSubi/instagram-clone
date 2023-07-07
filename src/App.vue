<template>
  <div class="header">
    <ul class="header-button-left">
      <li v-if="step !== 0" @click="cancel">Cancel</li>
    </ul>
    <ul class="header-button-right">
      <li v-if="step == 1" @click="step++">Next</li>
      <li v-if="step == 2" @click="publish">발행</li>
      <li v-if="step == 0" @click="getFollow">팔로우</li>
    </ul>
    <img src="./assets/logo.png" class="logo" />
  </div>

<!--<h4>안녕 {{$store.state.name}}</h4>-->
<!--<button @click="$store.commit('이름변경')">버튼</button>-->
<!--  <h4>나이:  {{$store.state.age}}</h4>-->
<!--  <button @click="$store.commit('나이증가', 10)">버튼</button>-->
  <!--vuex 사용법-->

<!--  <p>{{$store.state.more}}</p>-->
<!--  <button @click="$state.dispatch('getData')">더보기버튼</button>-->

  <ContainerBox @write="작성한글 = $event" :이미지 = "이미지" :인스타데이터들="인스타데이터들" :step="step" :선택한필터="선택한필터"/>
  <button @click="more">더보기</button>

  <div class="footer">
    <ul class="footer-button-plus">
<!--      <input @change="upload" type="file" id="file" class="inputfile" />-->
      <input @change="upload" multiple accept="image/*" type="file" id="file" class="inputfile" />
      <label for="file" class="input-plus">+</label>
    </ul>
  </div>

<!--  <div v-if="step == 0">내용0</div>-->
<!--  <div v-if="step == 1">내용1</div>-->
<!--  <div v-if="step == 2">내용2</div>-->
<!--  <button @click="step = 0">버튼0</button>-->
<!--  <button @click="step = 1">버튼1</button>-->
<!--  <button @click="step = 2">버튼2</button>-->
<!--  <div style="margin-top : 500px;"></div>-->


</template>

<script>

import ContainerBox from "@/components/ContainerBox";
import postData from "@/data/postdata";
import axios from 'axios';
import {mapActions, mapMutations, mapState} from "vuex";


export default {
  name: 'App',
  data() {
    return {
      작성한글:'',
      이미지: '',
      step: 0,
      인스타데이터들: postData,
      더보기: 0,
      선택한필터:""
    }
  },

  mounted() {
    this.emitter.on('박스클릭함', (a) => {
      this.선택한필터 = a;
    })
  },

  components: {
    ContainerBox: ContainerBox
  },

  computed:{
    now2(){
      return new Date();
      // 사용해도 실행되지 않습니다. 처음 실행하고 값을 간직함
    },
    name() {
      return this.$store.state.name;
    },
    age() {
      return this.$store.state.name;
    },
    ...mapState(['name','age','likes']),
    ...mapState({작명: 'name',작명2:'age',작명3:'likes'})
  },
  methods: {
    ...mapActions('getData'),
    ...mapMutations('setMore','좋아요','나이증가'),
    now(){
      return new Date();
      // 사용할때마다 실행됨
    },
    publish() {
      const 내게시물 =   {
        name: 'Kim Hyun',
        userImage: 'https://picsum.photos/100?random=1',
        postImage: this.이미지,
        likes: 36,
        date: 'May 15',
        liked: false,
        content: this.작성한글,
        filter: this.선택한필터,
      };
      this.인스타데이터들.unshift(내게시물);
      this.step = 0;
    },
    getFollow() {
      this.step = 3;
    },
    cancel() {
      if (this.step === 3) {
        this.step = 0;
      } else {
        this.step--;
      }
    },
    more() {
      // axios.post('URL',{name:'kim'}).then().catch((e) => {
      // console.log(e)})
      axios.get(`https://codingapple1.github.io/vue/more${this.더보기}.json`)
          .then((data) => {
            this.인스타데이터들.push(data.data)
            this.더보기++;
          })
    },
    upload(e) {
      let 파일 = e.target.files;
      console.log(파일[0].type);
      let url = URL.createObjectURL(파일[0]);
      this.이미지 = url;
      this.step++;
    }
  }
}
</script>

<style>
@import  'style.css';

</style>
