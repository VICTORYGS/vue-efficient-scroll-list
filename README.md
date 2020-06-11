# vue-efficient-scroll-list

## Usage
```bash
npm install vue-efficient-scroll-list --save
```
```vue
<template>
  <div class="home">
    <h2>scrollViewDemo</h2>
    <div style="width: 90vw;height:80vh;margin: auto">
      <effScrollView ref="scrollView"
              @arriveBottom="loading"
              @pullAndRelease="updated"
              :list="msgList"
              noMoreMsgTip="no more data..."
              pullDownTip="release to refresh..."
              :singleCount="10">
        <template v-slot="{list}">
          <div v-for="(v,i) in list" :key="i" class="msgItem">
            <img src="https://img.yzcdn.cn/vant/logo.png" width="100" height="100">
            <div>
              <b>title{{i}}</b>
              <div>{{'x'.repeat(60)}}</div>
            </div>
          </div>
        </template>
      </effScrollView>
    </div>
  </div>
</template>

<script>
import effScrollView from 'vue-efficient-scroll-list'
export default {
  name: 'home',
  components: {
    effScrollView
  },
  data(){
    return{
      msg:8000,
      msgList:new Array(10).fill(1),
      isNoMore:false
    }
  },
  methods: {
    loading() {
      // you should get data from backend actually
      setTimeout(d=>{
        if(this.msgList.length>60){
          // show noMore when backend haven't data anymore
          this.$refs.scrollView.noMore()
          // hide topTip and bottomTip after list updated
          this.$refs.scrollView.finishUpdate()
        }else {
          // add data
          this.msgList.push(...new Array(10).fill(null))
          // hide topTip and bottomTip after list updated
          this.$refs.scrollView.finishUpdate()
        }
      },800)
    },
    updated(){
      // you should get data from backend actually
      setTimeout(()=>{
        // update msgList
        this.msgList=new Array(10).fill(1)
        // hide topTip and bottomTip after list updated
        this.$refs.scrollView.finishUpdate()
      },800)
    }
  }
}
</script>
```

![image](https://github.com/VICTORYGS/vue-efficient-scroll-list/blob/master/1.png?raw=true)
![image](https://github.com/VICTORYGS/vue-efficient-scroll-list/blob/master/2.png?raw=true)
## Contributions

Welcome to improve this component with any issue, pull request or code review.


## Changelogs

Maintain and update occasionally, for changes see [release](https://github.com/VICTORYGS/vue-efficient-scroll-list/releases).


## License

[MIT License](https://github.com/VICTORYGS/vue-efficient-scroll-list/blob/master/LICENSE).
