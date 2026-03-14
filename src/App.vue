<template>
  <div class="page">

    <!-- BGM -->
    <audio ref="bgm" loop>
      <source src="./music.mp3" type="audio/mpeg" />
    </audio>

    <!-- 봉투 화면 -->
    <div v-if="!opened" class="envelope-wrapper" @click.stop="openLetter">
      <div class="envelope">
        <div class="flap"></div>
        <div class="letter-icon">💌</div>
      </div>

      <p class="open-text">편지를 열어주세요</p>
    </div>

    <!-- 편지 -->
    <main v-if="opened" class="container" @click="startMusic">

      <!-- BGM 안내 -->
      <transition name="fade">
        <div v-if="!musicStarted" class="bgm-guide">
          🎵 배경을 클릭하면 BGM이 실행됩니다
        </div>
      </transition>

      <div class="petals">
        <span v-for="n in 18" :key="n" class="petal"></span>
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

        <!-- 숨겨진 메세지 -->
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
const showSecret = ref(false);
const musicStarted = ref(false);

const bgm = ref(null);

const openLetter = () => {
  opened.value = true;
};

const toggleMessage = () => {
  showSecret.value = !showSecret.value;
};

const startMusic = () => {

  if (musicStarted.value) {
    return;
  }

  if (bgm.value) {
    bgm.value.play()
      .then(() => {
        musicStarted.value = true;
      })
      .catch((e) => {
        console.log("audio blocked", e);
      });
  }

};
</script>

<style scoped>

.page{
min-height:100vh;
background:linear-gradient(180deg,#fff7fb,#fff0f6);
}

/* 안내 문구 */

.bgm-guide{
position:fixed;
top:50%;
left:50%;
transform:translate(-50%,-50%);
background:rgba(0,0,0,0.65);
color:white;
padding:14px 24px;
border-radius:30px;
font-size:15px;
z-index:10;
backdrop-filter:blur(6px);
animation:pulse 1.5s infinite;
}

@keyframes pulse{
0%{opacity:.6}
50%{opacity:1}
100%{opacity:.6}
}

.fade-enter-active,
.fade-leave-active{
transition:opacity .6s;
}

.fade-enter-from,
.fade-leave-to{
opacity:0;
}

/* 봉투 */

.envelope-wrapper{
position:fixed;
inset:0;
background:linear-gradient(180deg,#fff7fb,#ffeaf3);
display:flex;
flex-direction:column;
justify-content:center;
align-items:center;
cursor:pointer;
}

.envelope{
width:200px;
height:130px;
background:#ffb3c7;
position:relative;
border-radius:10px;
box-shadow:0 10px 40px rgba(0,0,0,0.15);
animation:float 2s ease-in-out infinite;
}

.flap{
position:absolute;
top:-65px;
border-left:100px solid transparent;
border-right:100px solid transparent;
border-bottom:65px solid #ff9fb7;
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

@keyframes float{
0%{transform:translateY(0)}
50%{transform:translateY(-10px)}
100%{transform:translateY(0)}
}

/* 편지 */

.container{
display:flex;
justify-content:center;
padding:30px 15px;
}

.letter-card{
width:100%;
max-width:720px;
padding:40px 28px;
border-radius:28px;
background:rgba(255,255,255,0.9);
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
font-size:38px;
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
margin:24px 0;
}

.line-deco span{
width:100px;
height:1px;
background:#efb6ca;
}

.line-deco i{
margin:0 10px;
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

/* 꽃잎 */

.petals{
position:fixed;
inset:0;
pointer-events:none;
}

.petal{
position:absolute;
width:14px;
height:20px;
background:pink;
border-radius:50%;
animation:fall linear infinite;
}

@keyframes fall{
from{transform:translateY(-10vh)}
to{transform:translateY(110vh)}
}

</style>