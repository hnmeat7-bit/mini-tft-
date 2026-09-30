[index.html](https://github.com/user-attachments/files/32842814/index.html)
# mini-tft-<!doctype html>
<html lang="ko">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>Mini TFT Pixel Arena</title>
<style>
*{box-sizing:border-box}
html,body{margin:0;width:100%;height:100%;overflow:hidden;background:#07100d;color:#fff;font-family:system-ui,-apple-system,"Noto Sans KR",sans-serif;touch-action:none}
button{font:inherit;color:#fff;border:1px solid #725a2a;background:linear-gradient(#26342d,#101913);border-radius:8px;padding:9px 12px;cursor:pointer}
button:active{transform:translateY(1px)}
#app{width:100%;height:100%;position:relative;overflow:hidden}
#game{position:absolute;inset:0;width:100%;height:100%;image-rendering:pixelated}
.top{position:absolute;left:10px;right:10px;top:8px;height:52px;display:flex;align-items:center;justify-content:space-between;gap:8px;pointer-events:none}
.panel{background:rgba(7,13,12,.9);border:1px solid #80642d;box-shadow:0 0 0 1px #211b0e inset,0 6px 20px #0008;border-radius:10px}
.stats{display:flex;gap:7px;align-items:center;padding:6px 10px;pointer-events:auto}
.badge{padding:5px 8px;border:1px solid #3b5147;border-radius:7px;background:#101b17;font-size:13px;white-space:nowrap}
.gold{color:#ffd85a}.xp{color:#70d7ff}.hp{color:#ff7272}
.actions{display:flex;gap:6px;pointer-events:auto}
#leftTraits{position:absolute;left:10px;top:72px;width:154px;padding:10px}
.trait{display:flex;justify-content:space-between;border-bottom:1px solid #27352f;padding:7px 2px;font-size:13px}
.trait b{color:#ffd866}
#rightInfo{position:absolute;right:10px;top:72px;width:180px;padding:10px}
#targetName{font-weight:800;color:#ffd45d;margin-bottom:5px}
.bar{height:8px;background:#141c18;border:1px solid #34443c;border-radius:6px;overflow:hidden}
.bar i{display:block;height:100%;width:70%;background:#51c65b}
#damage{margin-top:9px;font-size:11px;color:#b9c7c0;line-height:1.6}
#bottom{position:absolute;left:10px;right:10px;bottom:8px;display:flex;gap:8px;align-items:end}
#controls{width:155px;padding:9px}
#controls button{width:100%;margin-top:5px}
#shop{flex:1;min-width:0;padding:8px;display:flex;gap:7px;justify-content:center}
.shopCard{width:112px;min-width:78px;height:128px;border:1px solid #51645a;background:linear-gradient(#18251f,#0c1411);border-radius:9px;display:flex;flex-direction:column;align-items:center;justify-content:space-between;padding:7px;cursor:pointer;position:relative}
.shopCard:hover{border-color:#d2ad49}
.shopCard .name{font-size:13px;font-weight:800}
.shopCard .cost{color:#ffd85a;font-size:12px}
.shopCard canvas{width:66px;height:66px;image-rendering:pixelated}
#bench{position:absolute;left:180px;right:10px;bottom:146px;height:62px;display:flex;gap:5px;justify-content:center;pointer-events:none}
.benchSlot{width:58px;height:58px;background:#101916dd;border:1px solid #3a4b42;border-radius:7px;display:grid;place-items:center}
.benchSlot canvas{width:50px;height:50px;image-rendering:pixelated}
#notice{position:absolute;left:50%;top:62px;transform:translateX(-50%);background:#0b1712e8;border:1px solid #80642d;border-radius:999px;padding:6px 14px;font-size:12px;opacity:0;transition:.2s;pointer-events:none}
#result{position:absolute;inset:0;display:none;align-items:center;justify-content:center;background:#0008;z-index:20}
.resultBox{width:min(600px,88vw);padding:30px;text-align:center;border-radius:20px;border:2px solid #c59a35;background:radial-gradient(circle at center,#332711ee,#090b0aee 70%);box-shadow:0 0 80px #000}
.resultBox h1{font-size:64px;margin:0 0 8px;text-shadow:4px 4px 0 #21150a}
.resultBox p{margin:6px;color:#ddd}
.resultBox button{margin-top:15px;font-size:17px;padding:12px 28px}
#help{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);z-index:30;width:min(500px,88vw);padding:22px;background:#07100fee;border:2px solid #8b6b2d;border-radius:16px;display:none}
#help h2{margin-top:0;color:#ffd45d}
#help li{margin:7px 0;color:#d3ddd7}
@media(max-width:850px){
 .top{height:45px}.stats{padding:4px 6px}.badge{font-size:11px;padding:4px 5px}
 #leftTraits{width:125px;top:60px;padding:6px;font-size:11px}
 #rightInfo{width:145px;top:60px;padding:7px;font-size:11px}
 #bottom{bottom:6px}.shopCard{height:105px}.shopCard canvas{width:52px;height:52px}.shopCard .name{font-size:11px}
 #controls{width:110px;padding:5px}.controls button{padding:6px}
 #bench{left:10px;right:10px;bottom:116px;height:49px}.benchSlot{width:45px;height:45px}.benchSlot canvas{width:40px;height:40px}
}
</style>
</head>
<body>
<div id="app">
<canvas id="game"></canvas>

<div class="top">
  <div class="panel stats">
    <div class="badge">STAGE <b id="stage">1-1</b></div>
    <div class="badge gold">🪙 <b id="gold">10</b></div>
    <div class="badge xp">XP <b id="xp">0/6</b></div>
    <div class="badge">LV <b id="level">1</b></div>
    <div class="badge hp">♥ <b id="life">100</b></div>
  </div>
  <div class="actions">
    <button onclick="buyXP()">XP 구매 <span class="gold">4G</span></button>
    <button onclick="reroll()">새로고침 <span class="gold">2G</span></button>
    <button onclick="toggleHelp()">?</button>
  </div>
</div>

<div id="leftTraits" class="panel">
  <div style="font-weight:800;color:#ffd45d;margin-bottom:4px">시너지</div>
  <div class="trait"><span>🛡 기사</span><b id="tKnight">0/2</b></div>
  <div class="trait"><span>🔮 마법사</span><b id="tMage">0/2</b></div>
  <div class="trait"><span>🏹 궁수</span><b id="tArcher">0/2</b></div>
  <div class="trait"><span>🗡 암살자</span><b id="tAssassin">0/2</b></div>
</div>

<div id="rightInfo" class="panel">
  <div id="targetName">적 정보</div>
  <div class="bar"><i id="targetHP"></i></div>
  <div id="damage">전투 대기 중</div>
</div>

<div id="notice"></div>
<div id="bench"></div>

<div id="bottom">
  <div id="controls" class="panel">
    <button onclick="startBattle()">⚔ 전투 시작</button>
    <button onclick="saveGame()">💾 저장</button>
    <button onclick="loadGame()">📂 불러오기</button>
  </div>
  <div id="shop" class="panel"></div>
</div>

<div id="result">
  <div class="resultBox">
    <h1 id="resultTitle">승리</h1>
    <p id="resultText"></p>
    <p id="resultReward"></p>
    <button onclick="closeResult()">다음 라운드</button>
  </div>
</div>

<div id="help">
<h2>🎮 MINI TFT 조작법</h2>
<ul>
<li>상점 캐릭터를 클릭하면 벤치에 영입됩니다.</li>
<li>벤치 캐릭터를 드래그해서 전투판에 배치합니다.</li>
<li>같은 캐릭터 3개가 모이면 자동으로 2성 합성됩니다.</li>
<li>2성 3개가 모이면 3성으로 강화됩니다.</li>
<li>D 키: 상점 새로고침</li>
<li>F 키: 경험치 +4</li>
<li>전투 시작 → 자동 전투 → 승리/패배 → 다음 라운드</li>
<li>저장 데이터는 이 브라우저의 LocalStorage에 저장됩니다.</li>
</ul>
<button onclick="toggleHelp()">닫기</button>
</div>
</div>

<script>
const canvas=document.getElementById('game'),ctx=canvas.getContext('2d');
let W=innerWidth,H=innerHeight,dpr=Math.min(devicePixelRatio||1,2);
function resize(){W=innerWidth;H=innerHeight;canvas.width=W*dpr;canvas.height=H*dpr;canvas.style.width=W+'px';canvas.style.height=H+'px';ctx.setTransform(dpr,0,0,dpr,0,0)}
addEventListener('resize',resize);resize();

const TYPES={
 warrior:{name:'검사',role:'warrior',color:'#2674d8',cost:1,hp:100,atk:16,range:110,skill:'검풍'},
 knight:{name:'기사',role:'knight',color:'#c79a30',cost:1,hp:155,atk:10,range:75,skill:'철벽'},
 mage:{name:'마법사',role:'mage',color:'#9a4fe3',cost:3,hp:85,atk:23,range:170,skill:'화염구'},
 archer:{name:'궁수',role:'archer',color:'#43a54b',cost:2,hp:90,atk:18,range:210,skill:'연사'},
 assassin:{name:'암살자',role:'assassin',color:'#7b315f',cost:3,hp:80,atk:27,range:90,skill:'그림자'}
};
const typeKeys=Object.keys(TYPES);
let game={
 state:'prepare',gold:10,level:1,xp:0,round:1,life:100,
 units:[],enemies:[],shop:[],effects:[],particles:[],drag:null,
 nextId:1,battleTimer:0
};

function unit(type,star=1){
 const t=TYPES[type], scale=Math.pow(1.55,star-1);
 return {id:game.nextId++,type,star,hp:t.hp*scale,maxhp:t.hp*scale,atk:t.atk*scale,range:t.range,skillCD:0,x:0,y:0,target:null,bench:true,dead:false};
}
function toast(s){const n=document.getElementById('notice');n.textContent=s;n.style.opacity=1;clearTimeout(toast.t);toast.t=setTimeout(()=>n.style.opacity=0,1300)}

function makeShop(){
 game.shop=Array.from({length:5},()=>typeKeys[Math.floor(Math.random()*typeKeys.length)]);
 renderShop();
}
function renderShop(){
 const el=document.getElementById('shop');el.innerHTML='';
 game.shop.forEach((type,i)=>{
  const t=TYPES[type],d=document.createElement('div');d.className='shopCard';
  const cv=document.createElement('canvas');cv.width=cv.height=80;drawSprite(cv.getContext('2d'),type,1,40,43,1.25);
  d.append(cv);d.insertAdjacentHTML('beforeend',`<div class="name">${t.name}</div><div class="cost">🪙 ${t.cost}</div>`);
  d.onclick=()=>buyUnit(i);el.appendChild(d);
 });
}
function buyUnit(i){
 if(game.state!=='prepare'){toast('전투 중에는 영입할 수 없습니다.');return}
 const type=game.shop[i],t=TYPES[type];
 if(game.gold<t.cost){toast('골드가 부족합니다.');return}
 game.gold-=t.cost;
 const u=unit(type);game.units.push(u);u.bench=true;
 game.shop[i]=typeKeys[Math.floor(Math.random()*typeKeys.length)];
 autoMerge();renderShop();renderBench();saveGame(false);toast(`${t.name} 영입!`);
}
function reroll(){
 if(game.gold<2){toast('골드가 부족합니다.');return}
 if(game.state!=='prepare')return;
 game.gold-=2;makeShop();toast('상점을 새로고침했습니다.');
}
function buyXP(){
 if(game.gold<4){toast('골드가 부족합니다.');return}
 game.gold-=4;game.xp+=4;
 while(game.xp>=xpNeed()){game.xp-=xpNeed();game.level=Math.min(8,game.level+1);toast(`레벨 ${game.level} 달성!`)}
 updateUI();
}
function xpNeed(){return 6+(game.level-1)*4}

function autoMerge(){
 let changed=true;
 while(changed){
  changed=false;
  for(const type of typeKeys){
   for(let star=1;star<3;star++){
    const arr=game.units.filter(u=>u.type===type&&u.star===star);
    if(arr.length>=3){
      const keep=arr[0];keep.star++;const t=TYPES[type],scale=Math.pow(1.55,keep.star-1);
      keep.maxhp=t.hp*scale;keep.hp=keep.maxhp;keep.atk=t.atk*scale;
      for(let k=1;k<3;k++){const idx=game.units.indexOf(arr[k]);if(idx>=0)game.units.splice(idx,1)}
      changed=true;game.effects.push({kind:'merge',x:keep.x||W/2,y:keep.y||H/2,t:0});
      toast(`${t.name} ${keep.star}성 강화!`);
      break;
    }
   }
   if(changed)break;
  }
 }
}

function boardRect(){return {x:Math.max(175,W*.16),y:90,w:Math.min(W-355,W*.68),h:Math.min(H-260,Math.max(270,H*.52))}}
function renderBench(){
 const b=document.getElementById('bench');b.innerHTML='';
 game.units.filter(u=>u.bench).slice(0,10).forEach(u=>{
  const s=document.createElement('div');s.className='benchSlot';s.dataset.id=u.id;
  const cv=document.createElement('canvas');cv.width=cv.height=64;drawSprite(cv.getContext('2d'),u.type,u.star,32,34,.85);s.append(cv);b.append(s);
 });
}
function startBattle(){
 if(game.state==='battle')return;
 const alive=game.units.filter(u=>!u.bench&&!u.dead);
 if(!alive.length){toast('전투판에 유닛을 배치하세요!');return}
 game.state='battle';game.battleTimer=0;game.enemies=[];
 const r=boardRect();
 const n=Math.min(7,Math.max(3,3+Math.floor(game.round/2)));
 for(let i=0;i<n;i++){
  const boss=game.round%5===0&&i===n-1;
  game.enemies.push({id:i,boss,type:boss?'orc':'skeleton',name:boss?'오크 대장':'스켈레톤',hp:(boss?260:80)+game.round*14,maxhp:(boss?260:80)+game.round*14,atk:(boss?18:10)+game.round*.8,x:r.x+r.w*.65+(i%3)*45,y:r.y+60+Math.floor(i/3)*70,cd:0,dead:false});
 }
 toast('전투 시작!');
}
function finishBattle(win){
 game.state='prepare';
 if(win){game.gold+=5;game.xp+=2;game.round++;showResult(true)}
 else{game.life=Math.max(0,game.life-8);showResult(false)}
 for(const u of game.units) {u.dead=false;u.hp=u.maxhp;u.bench=true}
 game.enemies=[];makeShop();renderBench();saveGame(false);updateUI();
}
function showResult(win){
 const r=document.getElementById('result'),box=r.querySelector('.resultBox'),title=document.getElementById('resultTitle');
 r.style.display='flex';title.textContent=win?'승리!':'패배';
 title.style.color=win?'#ffd84f':'#ff4545';
 document.getElementById('resultText').textContent=win?'모든 적을 처치했습니다!':'아군이 모두 쓰러졌습니다...';
 document.getElementById('resultReward').textContent=win?'🪙 +5 골드   ✦ XP +2':'♥ 생명력 -8';
 box.style.borderColor=win?'#d5a938':'#8e2929';
}
function closeResult(){document.getElementById('result').style.display='none';if(game.life<=0){game.life=100;game.round=1;game.gold=10;game.level=1;game.xp=0;game.units=[];toast('새 게임으로 시작합니다.')}updateUI()}

function saveGame(show=true){
 const data={...game,drag:null,effects:[],particles:[]};localStorage.setItem('miniTFT_save_v2',JSON.stringify(data));
 if(show)toast('저장했습니다.');
}
function loadGame(){
 const raw=localStorage.getItem('miniTFT_save_v2');if(!raw){toast('저장된 게임이 없습니다.');return}
 try{const d=JSON.parse(raw);game={...game,...d};renderShop();renderBench();updateUI();toast('불러왔습니다.')}catch(e){toast('저장 데이터를 읽을 수 없습니다.')}
}
function toggleHelp(){const h=document.getElementById('help');h.style.display=h.style.display==='block'?'none':'block'}

function updateUI(){
 document.getElementById('gold').textContent=game.gold;document.getElementById('level').textContent=game.level;
 document.getElementById('xp').textContent=`${game.xp}/${xpNeed()}`;document.getElementById('life').textContent=game.life;
 document.getElementById('stage').textContent=`${Math.ceil(game.round/3)}-${((game.round-1)%3)+1}`;
 const counts={knight:0,mage:0,archer:0,assassin:0};game.units.forEach(u=>{if(counts[u.type]!=null)counts[u.type]++});
 document.getElementById('tKnight').textContent=`${counts.knight}/2`;document.getElementById('tMage').textContent=`${counts.mage}/2`;
 document.getElementById('tArcher').textContent=`${counts.archer}/2`;document.getElementById('tAssassin').textContent=`${counts.assassin}/2`;
}

function drawArena(){
 ctx.clearRect(0,0,W,H);
 const r=boardRect();
 // background
 const g=ctx.createLinearGradient(0,0,0,H);g.addColorStop(0,'#16271e');g.addColorStop(1,'#07100c');ctx.fillStyle=g;ctx.fillRect(0,0,W,H);
 // forest silhouettes
 ctx.fillStyle='#0d1c14';for(let x=0;x<W;x+=48){ctx.beginPath();ctx.moveTo(x,105);ctx.lineTo(x+24,55+(x%3)*12);ctx.lineTo(x+48,105);ctx.fill()}
 // board
 ctx.fillStyle='#3b4a37';ctx.fillRect(r.x,r.y,r.w,r.h);ctx.strokeStyle='#92723a';ctx.lineWidth=3;ctx.strokeRect(r.x,r.y,r.w,r.h);
 const cols=8,rows=4;for(let i=1;i<cols;i++){ctx.strokeStyle='#5d6c51';ctx.lineWidth=1;ctx.beginPath();ctx.moveTo(r.x+i*r.w/cols,r.y);ctx.lineTo(r.x+i*r.w/cols,r.y+r.h);ctx.stroke()}
 for(let j=1;j<rows;j++){ctx.beginPath();ctx.moveTo(r.x,r.y+j*r.h/rows);ctx.lineTo(r.x+r.w,r.y+j*r.h/rows);ctx.stroke()}
 // player units
 for(const u of game.units.filter(u=>!u.bench&&!u.dead)) drawUnit(u);
 // enemies
 for(const e of game.enemies.filter(e=>!e.dead)) drawEnemy(e);
 // effects
 for(const ef of game.effects) drawEffect(ef);
}
function drawUnit(u){
 drawSprite(ctx,u.type,u.star,u.x,u.y,1.05);
 ctx.fillStyle='#0b120e';ctx.fillRect(u.x-25,u.y-38,50,5);ctx.fillStyle='#4ee067';ctx.fillRect(u.x-24,u.y-37,48*Math.max(0,u.hp/u.maxhp),3);
 ctx.fillStyle='#ffd95c';ctx.font='bold 12px sans-serif';ctx.textAlign='center';ctx.fillText('★'.repeat(u.star),u.x,u.y+39);
}
function drawEnemy(e){
 drawMonster(ctx,e.type,e.x,e.y,e.boss?1.35:1);
 ctx.fillStyle='#180b0b';ctx.fillRect(e.x-27,e.y-40,54,6);ctx.fillStyle='#e54b4b';ctx.fillRect(e.x-26,e.y-39,52*Math.max(0,e.hp/e.maxhp),4);
}
function drawSprite(c,type,star,x,y,s){
 c.save();c.translate(x,y);c.scale(s,s);c.imageSmoothingEnabled=false;
 const t=TYPES[type]; // pixel-art-like block character
 const body=t.color;
 c.fillStyle='#111';c.fillRect(-14,-19,28,35);
 c.fillStyle=body;c.fillRect(-11,-12,22,25);
 c.fillStyle='#f0c49b';c.fillRect(-8,-24,16,12);
 c.fillStyle='#171717';c.fillRect(-9,-27,18,5);
 c.fillStyle='#fff';c.fillRect(-5,-19,3,3);c.fillRect(2,-19,3,3);
 if(type==='warrior'){c.fillStyle='#dce8ff';c.fillRect(12,-11,5,25);c.fillStyle='#8b6a35';c.fillRect(16,7,6,3)}
 if(type==='knight'){c.fillStyle='#c9d2d7';c.fillRect(-13,-25,26,8);c.fillStyle='#e0b34b';c.fillRect(10,-7,7,20)}
 if(type==='mage'){c.fillStyle='#5a2c9b';c.fillRect(-14,-28,28,5);c.fillStyle='#74d7ff';c.fillRect(13,-5,4,4)}
 if(type==='archer'){c.strokeStyle='#e2b56b';c.lineWidth=3;c.beginPath();c.arc(15,0,11,-1.2,1.2);c.stroke()}
 if(type==='assassin'){c.fillStyle='#20222b';c.fillRect(-12,-25,24,5);c.fillStyle='#d6d6d6';c.fillRect(12,-13,4,15)}
 if(star>=2){c.fillStyle='#ffd34f';for(let i=0;i<star;i++){c.fillRect(-9+i*8,20,5,5)}}
 c.restore();
}
function drawMonster(c,type,x,y,s){
 c.save();c.translate(x,y);c.scale(s,s);c.imageSmoothingEnabled=false;
 if(type==='orc'){c.fillStyle='#101010';c.fillRect(-17,-18,34,36);c.fillStyle='#4c9b3d';c.fillRect(-14,-13,28,27);c.fillStyle='#76b65a';c.fillRect(-10,-27,20,14);c.fillStyle='#fff';c.fillRect(-7,-22,4,4);c.fillRect(4,-22,4,4);c.fillStyle='#aaa';c.fillRect(15,-5,7,18)}
 else{c.fillStyle='#dedbd0';c.fillRect(-14,-17,28,32);c.fillStyle='#151515';c.fillRect(-10,-12,20,22);c.fillStyle='#fff';c.fillRect(-8,-20,16,5);c.fillStyle='#e44b4b';c.fillRect(-7,-8,4,4);c.fillRect(3,-8,4,4)}
 c.restore();
}
function drawEffect(e){
 if(e.kind==='merge'){const p=Math.min(1,e.t/45);ctx.save();ctx.globalAlpha=1-p;ctx.strokeStyle='#ffd34f';ctx.lineWidth=5;ctx.beginPath();ctx.arc(e.x,e.y,20+p*70,0,Math.PI*2);ctx.stroke();for(let i=0;i<10;i++){const a=i*Math.PI*.2;ctx.fillRect(e.x+Math.cos(a)*(20+p*65),e.y+Math.sin(a)*(20+p*65),5,5)}ctx.restore()}
 if(e.kind==='hit'){ctx.fillStyle='#ffda5c';ctx.fillRect(e.x-3,e.y-3,6,6)}
}

function update(dt){
 if(game.state==='battle'){
  game.battleTimer+=dt;
  const units=game.units.filter(u=>!u.bench&&!u.dead), enemies=game.enemies.filter(e=>!e.dead);
  for(const u of units){
   u.skillCD=Math.max(0,u.skillCD-dt);
   let target=enemies.reduce((best,e)=>!best||dist(u,e)<dist(u,best)?e:best,null);
   if(!target)continue;
   const d=dist(u,target);
   if(d>u.range){u.x+=(target.x-u.x)/Math.max(1,d)*dt*.055;u.y+=(target.y-u.y)/Math.max(1,d)*dt*.055}
   else if(u.skillCD<=0){let dmg=u.atk; if(u.type==='mage')dmg*=1.8;if(u.type==='assassin')dmg*=2.1;target.hp-=dmg;u.skillCD=u.type==='archer'?700:u.type==='knight'?1100:850;game.effects.push({kind:'hit',x:target.x,y:target.y,t:0})}
  }
  for(const e of enemies){
   e.cd=Math.max(0,e.cd-dt);let target=units.reduce((best,u)=>!best||dist(e,u)<dist(e,best)?u:best,null);if(!target)continue;
   const d=dist(e,target);if(d>35){e.x+=(target.x-e.x)/Math.max(1,d)*dt*.045;e.y+=(target.y-e.y)/Math.max(1,d)*dt*.045}
   else if(e.cd<=0){target.hp-=e.atk;e.cd=900}
  }
  units.forEach(u=>{if(u.hp<=0)u.dead=true});
  enemies.forEach(e=>{if(e.hp<=0)e.dead=true});
  if(enemies.length&&enemies.every(e=>e.dead))finishBattle(true);
  else if(units.length&&units.every(u=>u.dead))finishBattle(false);
 }
 game.effects.forEach(e=>e.t+=dt/16);game.effects=game.effects.filter(e=>e.t<50);
}

function dist(a,b){return Math.hypot(a.x-b.x,a.y-b.y)}

let last=performance.now();
function loop(now){const dt=Math.min(40,now-last);last=now;update(dt);drawArena();updateUI();requestAnimationFrame(loop)}
makeShop();renderBench();updateUI();requestAnimationFrame(loop);

// Drag/touch: click bench unit, then place on board.
function pointerPos(e){const r=canvas.getBoundingClientRect();return {x:e.clientX-r.left,y:e.clientY-r.top}}
canvas.addEventListener('pointerdown',e=>{
 if(game.state!=='prepare')return;
 const p=pointerPos(e),r=boardRect();
 let picked=null;
 for(const u of game.units.filter(u=>!u.bench)){if(Math.hypot(u.x-p.x,u.y-p.y)<32)picked=u}
 if(picked){game.drag=picked;canvas.setPointerCapture(e.pointerId);return}
 // click bench slots isn't on canvas, handled below
});
canvas.addEventListener('pointermove',e=>{if(game.drag){const p=pointerPos(e);game.drag.x=p.x;game.drag.y=p.y}});
canvas.addEventListener('pointerup',e=>{
 if(game.drag){const r=boardRect();const u=game.drag;u.x=Math.max(r.x+25,Math.min(r.x+r.w-25,u.x));u.y=Math.max(r.y+25,Math.min(r.y+r.h-25,u.y));u.bench=false;game.drag=null;renderBench();saveGame(false)}
});
document.getElementById('bench').addEventListener('pointerdown',e=>{
 const slot=e.target.closest('.benchSlot');if(!slot||game.state!=='prepare')return;
 const id=+slot.dataset.id,u=game.units.find(x=>x.id===id);if(!u)return;
 const r=boardRect();u.bench=false;u.x=r.x+r.w*.25+(game.units.indexOf(u)%4)*55;u.y=r.y+r.h*.55;renderBench();toast(`${TYPES[u.type].name} 배치!`);saveGame(false);
});
addEventListener('keydown',e=>{
 if(e.key.toLowerCase()==='d'){if(game.state==='prepare')reroll()}
 if(e.key.toLowerCase()==='f'){buyXP()}
 if(e.key==='Enter'&&game.state==='prepare')startBattle()
});
</script>
</body>
</html>
