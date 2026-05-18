<template>
  <div class="search" v-if="!selectedItem"><!--디테일창일땐 숨김 -->
    <form @submit.prevent="newsList">
      <input type="text" placeholder="뉴스 키워드를 입력해 주세요." v-model="keyword"/>
      <Search @click.prevent="newsList" style="cursor:pointer; margin-left:10px; margin-top:10px" />
    </form>
  </div>
  <div class="home" v-if="!selectedItem"><!--디테일창일땐 숨김 -->
  <button 
    v-for="k in keywords" 
    :key="k"
    :class="{ active: keyword === k }"
    @click="keywordChange(k)"
  >{{ k }}</button>
  </div>
  
  <div class="newsList" v-if="!selectedItem"><!--디테일창일땐 숨김 -->
    <div v-if="loading" class="loading">
      <div class="spinner"></div>
      <p>불러오는 중...</p>
    </div>
    <ul v-else>
      <li v-for="(item,idx) in data" :key="idx">
        <a :href="item.link" @click.prevent>
          <p><img :src="item.image" @click.prevent="openDetail(item)"/></p>
          <div class="newsTxt">
            <b v-html="item.title" @click.prevent="openDetail(item)"></b>
            <div class="newsDS">
              <p class="newsDate">{{ formatDate(item.pubDate) }}</p>
            <!--  <p v-html="item.description"></p> -->
              <div class="scrap">
                <p @click.prevent="toggleScrap(item)"
                :class="{ active: isScraped(item) }"
                >
                  {{ isScraped(item) ? '★' : '☆' }}
                </p>
              </div>
            
            </div>
          </div>  
        </a>
      </li>
    </ul>
  </div>

  <!-- 기사 상세 -->
  <div class="detail" v-if="selectedItem">
    <p class="back" @click="closeDetail"><ArrowLeft /></p>
    <h2 v-html="selectedItem.title"></h2>
    <p class="newsDTDate">{{ formatDate(selectedItem.pubDate) }}</p>
    <img :src="selectedItem.image"/>
    <p class="cont">{{ selectedItem.content }}</p>
  </div>
</template>

<script>
import { ArrowLeft, Search } from 'lucide-vue-next'//vue 3 아이콘 라이브러리
export default {
  components: { ArrowLeft, Search },
   data(){
    return {
      data: [],
      keyword: '경제',  // 기본값 경제
      keywords: ['경제', '정치', '생활', '시사'], // 버튼 목록
      scrappedList: [],  // 스크랩 목록
      loading: false,
      selectedItem:null // 선택된 기사
    }
  },
  methods:{
    keywordChange(value){
      this.keyword=value;
      this.newsList();
    },
    newsList(){
      this.loading = true  // 시작
      fetch(`https://react-todolist0326.vercel.app/news?keyword=${this.keyword}`)
      .then(res=>res.json())
      .then(res=>{
        this.data=res
        this.loading = false  // 완료
      })
    },
    // 스크랩 여부 확인
    isScraped(item){
      return this.scrappedList.some(s => s.link === item.link)
    },
    // 스크랩 토글
    toggleScrap(item){
      if(this.isScraped(item)){
        this.scrappedList = this.scrappedList.filter(s => s.link !== item.link)
      } else {
         this.scrappedList.push({ ...item, category: this.keyword })  // 현재 키워드 같이 저장
      }
      localStorage.setItem('scrappedList', JSON.stringify(this.scrappedList))
    },
    formatDate(dateStr){
      const date = new Date(dateStr)
      return `${date.getFullYear()}년 ${date.getMonth()+1}월 ${date.getDate()}일 ${String(date.getHours()).padStart(2,'0')}:${String(date.getMinutes()).padStart(2,'0')}`
    },
    // 기사 상세보기
    async openDetail(item){
      console.log('item.link:', item.link)
      const res=await fetch(`https://react-todolist0326.vercel.app/news/detail?url=${encodeURIComponent(item.link)}`)
      const detail = await res.json()
      console.log('detail:', detail)
      this.selectedItem = {...item,...detail}
    },
    //기사 상세보기 닫기
    closeDetail(){
      this.selectedItem = null  
    }
  },
  mounted(){
    this.newsList();
    // 저장된 스크랩 불러오기
    const saved = localStorage.getItem('scrappedList')
    if(saved) this.scrappedList = JSON.parse(saved)
  },
  name: 'HomeView'
}
</script>

<style scoped lang="scss">

.home{
  padding-bottom:10px;
  text-align: left;
  button{
    margin: 10px 20px 10px 0;
    padding: 5px 20px;
    font-size: 16px;
    font-weight: 400;
    color: #a8a8a8;;
    border: 1px solid #ccc;;
    background-color: #ffffff;
    
    border-radius: 10px;
    cursor: pointer;
    &:hover{
      color: #42b983;
      border: 1px solid #42b983;
    }
     &.active {
    color: #42b983;
    border: 1px solid #42b983;
    font-weight: 600;
  }
  }
}
.search{
  margin: 20px 0 20px 0;
  form{
    display: flex;
    justify-content: left;
    width: 390px;
    input{
      width:100%;
      padding: 10px;
      font-size: 18px;
      border: 1px solid #ccc;
      border-radius: 5px;
      &:focus{
        outline: none;
        border-color: #42b983;
      }
    }
  }
}
.newsList{
  //로딩화면
  .loading{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 200px;
    .spinner{
      width: 40px;
      height: 40px;
      border: 4px solid #e0e0e0;
      border-top-color: #42b983;
      border-radius: 50%;
      animation: spin 0.8s linear infinite;
    }
    p{
      color: #a8a8a8;
      font-size: 16px;
    }
  }
  @keyframes spin {
  to { transform: rotate(360deg); }
  }
  ul{
    list-style-type: none;
    padding: 0;
  }
  li{
    display: flex;
    margin: 20px 0;
    a{
      display: flex;
      text-decoration: none;
      color: #2c3e50;
      cursor:default;
      p{
          margin: 0;
        img{
          width: 150px;
          height: 100px;
          object-fit: cover;
          margin-right: 20px;
          border-radius: 10px;
          cursor: pointer;
        }
      }
      .newsTxt{
        text-align: left;
        b{
          font-size: 18px;
          font-weight: bold;
          margin-bottom: 10px;
          text-align:center;
          cursor: pointer;
          &:hover{
            text-decoration: underline;
          }
        }
        .newsDS{
          display: flex;
          padding-top:7px;
          justify-content: space-between;
          .newsDate{
            font-size: 14px;
            color: #6e6e6e;
          }
          .scrap{
            margin-top: 10px;
            text-align: right;
            p{
              font-size: 22px;
              color: #ccc;
              line-height: 0.1;
              cursor: pointer;
              transition: color 0.2s;
              &:hover{ color: #42b983; }
              &.active{ color: #42b983; }  // ★일때
            }}

        }
      }
    }
  }
}

.detail{
  text-align: left;
  .newsDTDate{
    font-size: 14px;
    color: #6e6e6e;
  }
  .back{
    font-size: 22px;
    text-align:right;
    color: #2c3e50;
    cursor: pointer;
    margin: 0 0 10px 0;
    position: absolute;
    top: 40px;
    right: 20px;
  }
  h2{
    font-size: 24px;
    margin-bottom: 15px;
  }
  img{
    width:100%;
    max-height:400px;
    object-fit: cover;
    border-radius:10px;
    margin-bottom:10px;
  }
  .cont{
    font-size:16px;
  }
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
