<template>
  <div style="padding : 10px">
    <h4>팔로워</h4>
    <input placeholder="?" @input="search($event.target.value)"/>
    <div class="post-header" v-for="(a, i) in follower" :key="i">
      <div class="profile" :style="`background-image:url(${a.image})`"></div>
      <span class="profile-name">{{ a.name }}</span>
    </div>
  </div>
</template>

<script>
// import { onMounted, reactive, ref, toRefs } from "vue";
// import axios from "axios";
// import {useStore} from "vuex";

import {onMounted, ref, toRefs} from "vue";
import axios from "axios";

export default {
  name: 'MyPage',
  props:{
    one: Number
   },
  setup(props, context){
    // created 라는 라이프 사이클 훅과 유사
    // 컴포넌트를 만들기 전에 이 코드먼저 시작해주세요 라는 코드
    // props를 이 안에서 사용하려면 param에 담아줘야함. => setup이 created hook 이기 때문에
    context.constructor;
    let {one} = toRefs(props);
    one.value;

    // watch(one,() => {
    //   // one이라는 props가 변경될때마가 실행되는 코드 작성
    // })

    // let 결과 = computed(() => {
    //   return 10
    // })

    // let store = useStore();
    // console.log(store.state.name);

    let follower = ref([]);
    let followerOriginal = ref([]);
    // 그나머지 자료형(primitive: 0, 1, 등등) 을 쓸때 사용

    onMounted(()=>{
      axios.get('/follower.json').then((a)=>{
        follower.value = a.data;
        followerOriginal.value = [...a.data];
      }).catch((e) => {console.log(e)})
    });

    // let test = reactive({name:'kim'});
    // // 보통 array, object 집어 넣음
    // test;
    function search(검색어) {
      let newFollower = followerOriginal.value.filter((a)=>{
        return a.name.indexOf(검색어) != -1
      });
      follower.value = [...newFollower]
    }

    return {follower, search};
  },
}
</script>

<style>
</style>
