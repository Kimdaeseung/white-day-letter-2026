<template>
  <div class="page" @click="handleFirstTouch">

    <!-- BGM -->
    <audio ref="bgm" loop>
      <source src="/music.mp3" type="audio/mpeg" />
    </audio>

    <!-- 봉투 -->
    <div v-if="!opened" class="envelope-wrapper" @click.stop="openLetter">
      <div class="envelope">
        <div class="letter-icon">💌</div>
      </div>
      <p class="open-text">편지를 열어주세요</p>
    </div>

    <!-- 🌸 벚꽃 -->
    <div class="petals">
      <span v-for="n in 24" :key="n" class="petal"></span>
    </div>

    <!-- 편지 -->
    <main v-if="opened" class="container">

      <div v-if="!musicStarted" class="touch-hint-fixed">
        화면을 한 번 눌러줘
      </div>

      <section class="letter-card">

        <p class="date">2026.03.14 WHITE DAY</p>

        <h1 class="title">
          봄처럼 찾아온<br />
          소중한 내 수연이에게
        </h1>

        <p class="subtitle">
          벌써 만난지 1년이 지나,<br />
          두번째 화이트데이를 맞이하구 있어~ ㅎㅎ
        </p>

        <div class="line-deco">
          <span></span>
          <i>❁</i>
          <span></span>
        </div>

        <div class="letter-body">
          <p>
            오늘은 화이트데이라서,<br />
            평소에는 다 하지 못했던 말을 조용히 전하고 싶어~
          </p>

          <p>
            수연이와 함께 있으면 평범한 하루도 유난히 따뜻해지고,<br />
            아무렇지 않은 순간들조차 오래 기억하고 싶어져.
          </p>

          <p>
            앞으로도 웃을때 함께 옆에서 웃고싶구,<br />
            힘들때든~ 그게 어느때가 됐든 함께 할거야~
          </p>

          <p>
            언제나 당연하지 않게 더 소중하게,<br />
            더 예쁘게 아껴주고 싶어!
          </p>

          <p class="highlight">
            올봄에도, 그 다음 계절에도<br />
            수연이 곁에는 내가 있을거야~
          </p>

          <p class="highlight">
            내 사랑스러운 쪼꼬미 공쥬~<br />
            언제나 변함없이 너무너무 사랑해<br />
            언제까지나~
          </p>
        </div>

        <div class="signature">
          <p>With love,</p>
          <p class="name">너를 많이 아끼는 바봉이가</p>
        </div>

        <div class="button-row">
          <button class="love-button" @click.stop="toggleMessage">
            {{ showSecret ? "닫기" : "숨겨둔 한마디 보기" }}
          </button>
        </div>

        <transition name="soft-rise">
          <div v-if="showSecret" class="secret-message">
            <div class="secret-title">Forever 윤.수.연</div>
            <p>
              앞으로도 이쁜일만 가득할거구~<br />
              1년이라는 시간을 넘어 함께하면서 너무 행복했어~<br />
              앞으로 남은 생 마지막까지 잘 부탁해요~ 내사랑~
            </p>
          </div>
        </transition>

        <div class="bottom-deco">❀ ❁ ✿ ❁ ❀</div>

      </section>
    </main>
  </div>
</template>

<script setup>
import { ref } from "vue";

const opened = ref(false);
const musicStarted = ref(false);
const showSecret = ref(false);

const bgm = ref(null);

const openLetter = () => {
  opened.value = true;
};

function handleFirstTouch(){

  if(musicStarted.value) return;
  if(!bgm.value) return;

  bgm.value.volume = 0.35;

  bgm.value.play()
    .then(()=>{
      musicStarted.value = true
    })
    .catch(()=>{});

}

const toggleMessage = () => {
  showSecret.value = !showSecret.value;
};
</script>

<style scoped>

.page{
min-height:100vh;
background:linear-gradient(180deg,#fff7fb,#fff0f6);
overflow:hidden;
}

.touch-hint-fixed{
position:fixed;
bottom:40px;
left:50%;
transform:translateX(-50%);
background:rgba(0,0,0,0.7);
color:white;
padding:12px 20px;
border-radius:30px;
font-size:14px;
z-index:20;
animation:pulse 1.6s infinite;
}

@keyframes pulse{
0%{opacity:.5}
50%{opacity:1}
100%{opacity:.5}
}

.envelope-wrapper{
position:fixed;
inset:0;
background:linear-gradient(180deg,#fff7fb,#ffeaf3);
display:flex;
flex-direction:column;
justify-content:center;
align-items:center;
cursor:pointer;
z-index:10;
}

.envelope{
width:200px;
height:130px;
background:#ffb3c7;
border-radius:10px;
position:relative;
}

.letter-icon{
position:absolute;
font-size:42px;
top:40px;
left:50%;
transform:translateX(-50%);
}

.open-text{
margin-top:20px;
font-size:18px;
color:#c75c85;
font-weight:bold;
}

.container{
display:flex;
justify-content:center;
padding:30px 15px;
position:relative;
z-index:2;
}

.letter-card{
width:100%;
max-width:720px;
padding:40px 28px;
border-radius:28px;
background:rgba(255,255,255,0.92);
backdrop-filter:blur(6px);
box-shadow:0 20px 60px rgba(0,0,0,0.12);
}

.date{
text-align:center;
font-size:14px;
letter-spacing:2px;
color:#c47796;
}

.title{
text-align:center;
font-size:36px;
color:#cf5f88;
line-height:1.4;
}

.subtitle{
text-align:center;
margin-top:10px;
}

.line-deco{
display:flex;
justify-content:center;
align-items:center;
margin:24px 0;
}

.line-deco span{
width:100px;
height:1px;
background:#efb6ca;
}

.line-deco i{
margin:0 10px;
font-size:18px;
line-height:1;
display:flex;
align-items:center;
}

.letter-body{
line-height:1.9;
text-align:center;
}

.highlight{
font-weight:bold;
color:#c95f86;
}

.signature{
margin-top:30px;
text-align:right;
}

.name{
font-weight:bold;
}

.button-row{
display:flex;
justify-content:center;
margin-top:20px;
}

.love-button{
padding:12px 22px;
border-radius:30px;
border:none;
background:#ff86a8;
color:white;
font-weight:bold;
cursor:pointer;
}

.secret-message{
margin-top:20px;
padding:20px;
border-radius:20px;
background:#fff0f6;
text-align:center;
}

.bottom-deco{
text-align:center;
margin-top:20px;
}

/* 🌸 벚꽃 개선 */

.petals{
position:fixed;
inset:0;
pointer-events:none;
z-index:5;
}

.petal{
position:absolute;
top:-30px;
width:12px;
height:18px;
background:rgba(255,182,193,0.65);
border-radius:60% 60% 60% 60% / 70% 70% 40% 40%;
animation:fall linear infinite;
transform-origin:center;
filter:blur(0.3px);
}

.petal:nth-child(odd){
transform:rotate(25deg);
}

.petal:nth-child(even){
transform:rotate(-20deg);
}

.petal:nth-child(1){left:5%;animation-duration:12s}
.petal:nth-child(2){left:12%;animation-duration:10s}
.petal:nth-child(3){left:18%;animation-duration:14s}
.petal:nth-child(4){left:25%;animation-duration:11s}
.petal:nth-child(5){left:32%;animation-duration:13s}
.petal:nth-child(6){left:40%;animation-duration:9s}
.petal:nth-child(7){left:48%;animation-duration:12s}
.petal:nth-child(8){left:56%;animation-duration:10s}
.petal:nth-child(9){left:63%;animation-duration:14s}
.petal:nth-child(10){left:70%;animation-duration:11s}
.petal:nth-child(11){left:78%;animation-duration:13s}
.petal:nth-child(12){left:85%;animation-duration:10s}
.petal:nth-child(13){left:92%;animation-duration:12s}
.petal:nth-child(14){left:15%;animation-duration:13s}
.petal:nth-child(15){left:35%;animation-duration:9s}
.petal:nth-child(16){left:55%;animation-duration:11s}
.petal:nth-child(17){left:75%;animation-duration:12s}
.petal:nth-child(18){left:95%;animation-duration:14s}

@keyframes fall{
0%{
transform:translateY(-10vh) rotate(0deg);
}
50%{
transform:translateY(50vh) translateX(20px) rotate(180deg);
}
100%{
transform:translateY(110vh) translateX(-20px) rotate(360deg);
}
}

@media (max-width:640px){

.letter-card{
padding:30px 20px;
}

.title{
font-size:30px;
}

}

</style>