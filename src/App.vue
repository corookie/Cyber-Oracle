<script setup>
import { computed, onBeforeUnmount, ref } from "vue";

const oracles = [
  { title: "地天泰", symbol: "䷊", level: "大吉", grade: "great", text: "天地相交，万物通达。小的阻碍退去，大的机会正在进来。", interpretation: "这是顺势之象。合作、推进、启动项目都比较有利，关键是保持信息流通。", advice: "可以加速推进，把握当下窗口。", keywords: ["通达", "合作", "开局"], probability: 94 },
  { title: "乾为天", symbol: "䷀", level: "大吉", grade: "great", text: "乾元亨利贞。势如天行，自强不息，正道可成。", interpretation: "这是强势上升之象。你的主动性、执行力和方向感都在增强。", advice: "大胆承担主导权，但不要脱离正道。", keywords: ["主动", "突破", "正道"], probability: 96 },
  { title: "火天大有", symbol: "䷍", level: "大吉", grade: "great", text: "火在天上，所有者大。资源汇聚，光明可照四方。", interpretation: "资源、机会和人心正在聚拢。适合定目标、扩影响、做关键推进。", advice: "抓住高点，把成果转化为长期优势。", keywords: ["丰盛", "资源", "扩张"], probability: 98 },
  { title: "泽山咸", symbol: "䷞", level: "吉", grade: "normal", text: "有感而应。先动心，后动事；真诚相交，事情自会有回声。", interpretation: "适合沟通、表白、建立连接。不要用力过猛，先确认彼此是否同频。", advice: "主动释放善意，但保留分寸。", keywords: ["感应", "互动", "同频"], probability: 72 },
  { title: "水天需", symbol: "䷄", level: "吉", grade: "normal", text: "云在天上，雨未落下。时机将至，静待其成。", interpretation: "事情不是没希望，只是还需要时间。现在适合准备材料、养精蓄锐。", advice: "耐心等待，不要用焦虑破坏节奏。", keywords: ["等待", "蓄养", "时机"], probability: 69 },
  { title: "山水蒙", symbol: "䷃", level: "平", grade: "normal", text: "雾在山下，泉水初出。看不清时，先求明白，不急判断。", interpretation: "信息还不完整，容易凭感觉误判。多问、多学、多验证会带来转机。", advice: "把问题拆小，先补足关键事实。", keywords: ["求证", "学习", "启蒙"], probability: 58 },
  { title: "风天小畜", symbol: "䷈", level: "小吉", grade: "normal", text: "云密而雨未下，已有积累，尚未成势。", interpretation: "你的准备正在增加，但还不到全面爆发的时候。继续打磨细节。", advice: "先小范围验证，再扩大行动。", keywords: ["积累", "克制", "打磨"], probability: 66 },
  { title: "天地否", symbol: "䷋", level: "凶兆", grade: "omen", text: "天地不交，气机闭塞。越是强推，越容易被现实反弹。", interpretation: "当前局面沟通不畅、资源不顺，容易做多错多。先停下来保存实力。", advice: "暂停重大决定，先查清阻塞点。", keywords: ["闭塞", "停滞", "避险"], probability: 24 },
  { title: "天水讼", symbol: "䷅", level: "凶兆", grade: "omen", text: "天上水下，彼此相违。争端已起，继续硬碰只会两伤。", interpretation: "当前容易出现误会、争执或责任不清。胜负不是重点，止损才是重点。", advice: "保留证据，停止情绪化沟通。", keywords: ["争执", "边界", "止损"], probability: 19 },
  { title: "坎为水", symbol: "䷜", level: "大凶", grade: "omen", text: "重水相叠，险中有险。若无把握，勿入深渊。", interpretation: "当前风险较重，容易陷入反复、损耗或不可控局面。先撤一步。", advice: "停止冒险，优先保全现金、精力和信用。", keywords: ["险阻", "反复", "止损"], probability: 8 }
];

const phase = ref("idle");
const question = ref("");
const selectedOracle = ref(null);
const logs = ref(["SYSTEM_READY", "AETHER_LINK_ESTABLISHED"]);
const lastTitle = ref("");
const generatedAt = ref("");
let resultTimer = null;
let logTimer = null;

const kind = computed(() => selectedOracle.value?.grade || "normal");
const status = computed(() => phase.value === "casting" ? "RITUAL_CASTING_ACTIVE" : phase.value === "result" && kind.value === "great" ? "DIVINE_SYNC_FAVORABLE" : phase.value === "result" && kind.value === "omen" ? "SYSTEM_ANOMALY_CAUTION" : "SEQUENCE_ALPHA_READY");
const headline = computed(() => kind.value === "great" ? "DIVINE SYNC: FAVORABLE" : kind.value === "omen" ? "SYSTEM ANOMALY: CAUTION" : "ORACLE DECRYPTED");

function pickOracle() {
  const pool = oracles.length > 1 ? oracles.filter((item) => item.title !== lastTitle.value) : oracles;
  const item = pool[Math.floor(Math.random() * pool.length)];
  lastTitle.value = item.title;
  return item;
}

function startLogs() {
  const ritualLogs = ["CASTING_COPPER_LINES", "DECODING_YI_JING_SIGNAL", "ALIGNING_UPPER_TRIGRAM", "ALIGNING_LOWER_TRIGRAM", "MEASURING_OMEN_LEVEL", "LOCKING_ORACLE_RESPONSE"];
  let index = 0;
  logs.value = ["INIT_SCAN_REQUEST", "SIX_LINE_MATRIX_OPEN"];
  logTimer = window.setInterval(() => {
    logs.value = [...logs.value.slice(-5), ritualLogs[index % ritualLogs.length]];
    index += 1;
  }, 360);
}

function stopLogs() {
  if (logTimer) window.clearInterval(logTimer);
  logTimer = null;
}

function initiateDivination() {
  stopLogs();
  if (resultTimer) window.clearTimeout(resultTimer);
  phase.value = "casting";
  selectedOracle.value = null;
  startLogs();
  resultTimer = window.setTimeout(() => {
    stopLogs();
    selectedOracle.value = pickOracle();
    generatedAt.value = new Intl.DateTimeFormat("zh-CN", { year: "numeric", month: "2-digit", day: "2-digit", hour: "2-digit", minute: "2-digit", second: "2-digit" }).format(new Date());
    logs.value = [...logs.value.slice(-4), "HEXAGRAM_LOCKED", "OMEN_LEVEL_RENDERED"];
    phase.value = "result";
  }, 3600);
}

function resetReading() {
  stopLogs();
  if (resultTimer) window.clearTimeout(resultTimer);
  phase.value = "idle";
  selectedOracle.value = null;
  logs.value = ["SYSTEM_READY", "AETHER_LINK_ESTABLISHED"];
}

onBeforeUnmount(() => {
  stopLogs();
  if (resultTimer) window.clearTimeout(resultTimer);
});
</script>

<template>
  <div class="app-shell" :class="[`phase-${phase}`, `result-${kind}`]">
    <div class="scanline-overlay"></div>
    <header class="topbar">
      <h1>NEO_ORACLE_V1.0</h1><span>STABLE_CONNECTION</span>
      <nav><b :class="{active: phase !== 'result'}">DIVINE</b><b :class="{active: phase === 'result'}">ARCHIVE</b><b>SYNC</b></nav>
    </header>
    <aside class="sidebar">
      <div class="operator"><i>{{ kind === 'omen' ? '!' : '◎' }}</i><div><strong>OPERATOR_01</strong><small>{{ kind === 'omen' ? 'SOUL_SYNC_ERR_404' : 'SOUL_SYNC_ACTIVE' }}</small></div></div>
      <button @click="initiateDivination">INITIATE_SCAN</button>
      <a class="selected">▦ TERMINAL</a><a>✦ HEXAGRAMS</a><a>◉ RITUALS</a><a>⌁ CORE_SYNC</a>
    </aside>
    <main>
      <template v-if="phase !== 'result'">
        <section class="wheel" :class="{spinning: phase === 'casting'}">
          <div class="ring r1"></div><div class="ring r2"></div><div class="ring r3"></div>
          <div class="ritual-lines" :class="{casting: phase === 'casting'}"><span v-for="line in 6" :key="line"></span></div>
          <div class="trigram"><span></span><span></span><span></span></div><div class="core">○</div><div v-if="phase === 'casting'" class="scan-bar"></div>
        </section>
        <section class="terminal">
          <div class="kicker">[ AETHER_LINK_ESTABLISHED ]</div>
          <p class="copy">{{ phase === 'casting' ? 'CASTING SIX LINES... ALIGNING TRIGRAMS... MEASURING OMEN LEVEL.' : 'SYNCING SOUL WITH THE AETHER... DECIPHERING DATA FRAGMENTS FROM THE VOID.' }}</p>
          <p class="sub">{{ phase === 'casting' ? '（六爻生成中。。。卦象正在归位。）' : '（灵魂同步中。。。破译虚空数据碎片。）' }}</p>
          <label><span>QUERY_INPUT</span><textarea v-model="question" :disabled="phase === 'casting'" maxlength="120" placeholder="输入你想问的问题，也可以留空直接启动卦卜"></textarea></label>
          <button class="primary" :disabled="phase === 'casting'" @click="initiateDivination">{{ phase === 'casting' ? 'CASTING_HEXAGRAM' : 'INITIATE DIVINATION（启动卦卜）' }}</button>
        </section>
      </template>
      <section v-else class="result-screen">
        <div class="result-top"><span>{{ kind === 'great' ? '[ HARMONIOUS ]' : kind === 'omen' ? '[ STATUS: FRAGMENTED ]' : '[ ORACLE_DECRYPTED ]' }}</span><span>{{ generatedAt }}</span></div>
        <h2>{{ headline }}</h2><p class="sync">OMEN_LEVEL: {{ selectedOracle.level }}</p>
        <div class="result-layout">
          <aside class="hex"><div class="hex-lines"><span v-for="line in 6" :key="line"></span></div><em>{{ selectedOracle.symbol }}</em><strong>{{ selectedOracle.title }}</strong><small>{{ selectedOracle.level }}</small></aside>
          <article class="reading"><div class="meta"><span>[ {{ selectedOracle.grade.toUpperCase() }}_SIGNAL ]</span><span>{{ selectedOracle.probability }}%</span></div><p v-if="question.trim()" class="memory">QUERY: {{ question.trim() }}</p><p class="oracle-text">{{ selectedOracle.text }}</p><h3>SYSTEM_INTERPRETATION</h3><p>{{ selectedOracle.interpretation }}</p><h3>OPERATIONAL_ADVICE</h3><p>{{ selectedOracle.advice }}</p><div class="tags"><span v-for="tag in selectedOracle.keywords" :key="tag">[ {{ tag }} ]</span></div><button class="primary compact" @click="resetReading">{{ kind === 'omen' ? 'INIT_REBOOT（重新起卦）' : 'RECALIBRATE（再算一次）' }}</button></article>
        </div>
      </section>
      <section class="logs"><span v-for="log in logs" :key="log">{{ log }}</span></section>
    </main>
    <footer><span>© 2124 AETHER_NET.</span><span>{{ status }}</span><span>VOID_REF: 0x8FA2</span></footer>
  </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700;800&family=Space+Grotesk:wght@700&family=Space+Mono:wght@700&display=swap");
*{box-sizing:border-box}html,body,#app{min-height:100%;margin:0}body{min-width:320px;background:#131313;color:#e5e2e1;font-family:"JetBrains Mono",monospace;overflow-x:hidden}.app-shell{--primary:#ebb2ff;--secondary:#00eefa;--panel:rgba(208,252,255,.72);min-height:100vh;display:grid;grid-template-columns:320px 1fr;grid-template-rows:80px 1fr 56px;background:linear-gradient(90deg,rgba(0,238,250,.04) 1px,transparent 1px),linear-gradient(rgba(235,178,255,.04) 1px,transparent 1px),radial-gradient(circle at 64% 20%,rgba(0,238,250,.12),transparent 28rem),#131313;background-size:80px 80px,80px 80px,auto}.result-omen{--primary:#ff3b42;--secondary:#ff9f1a;--panel:rgba(255,59,66,.8)}.result-great{--primary:#ebb2ff;--secondary:#d0fcff}.scanline-overlay{position:fixed;inset:0;pointer-events:none;z-index:10;background:repeating-linear-gradient(to bottom,rgba(0,238,250,.1) 0,rgba(0,238,250,.1) 1px,transparent 1px,transparent 4px);mix-blend-mode:screen;opacity:.32}.topbar{grid-column:1/-1;display:flex;align-items:center;gap:28px;padding:0 64px;border-bottom:1px solid rgba(235,178,255,.28);background:rgba(8,8,10,.86);box-shadow:0 0 30px rgba(188,19,254,.12);position:relative;z-index:1}.topbar h1{margin:0;font-family:"Space Grotesk";color:var(--primary);font-size:2rem;text-shadow:2px 0 var(--secondary),-2px 0 #ffade6}.topbar>span,.kicker,.meta,.result-top,footer{font-family:"Space Mono";font-size:.72rem;font-weight:700;letter-spacing:.08em}.topbar>span{border:1px solid var(--panel);padding:6px 12px;color:#d0fcff}.topbar nav{margin-left:auto;display:flex;gap:48px}.topbar b{letter-spacing:.28em}.topbar .active{color:var(--primary);border-bottom:3px solid var(--primary);padding-bottom:20px}.sidebar{grid-row:2/3;border-right:1px solid rgba(0,238,250,.32);background:linear-gradient(180deg,rgba(0,238,250,.06),rgba(0,0,0,.1));padding:30px;display:flex;flex-direction:column;gap:28px;position:relative;z-index:1}.operator{display:flex;gap:16px;align-items:center}.operator i{display:grid;place-items:center;width:50px;height:50px;border:1px solid var(--panel);color:#d0fcff;font-style:normal;font-size:1.5rem}.operator small{display:block;color:#9aa7a8}.sidebar button,.primary{border:1px solid var(--panel);background:rgba(0,238,250,.06);color:#fff;font-weight:800;min-height:46px;box-shadow:inset 0 0 12px rgba(0,238,250,.16);cursor:pointer}.sidebar a{color:#d4c0d7;font-weight:800;letter-spacing:.05em;text-decoration:none;padding:20px 10px}.sidebar .selected{background:linear-gradient(90deg,rgba(0,238,250,.32),transparent);border-left:5px solid #d0fcff}main{grid-row:2/3;padding:22px 64px;display:grid;justify-items:center;align-content:start;position:relative;z-index:1}.wheel{position:relative;width:min(24vw,280px);aspect-ratio:1;display:grid;place-items:center;color:var(--secondary)}.ring{position:absolute;border:1px solid rgba(0,238,250,.25);border-radius:50%}.r1{inset:2%}.r2{inset:16%}.r3{inset:30%;background:radial-gradient(circle,rgba(0,238,250,.12),transparent 65%)}.spinning .r1{animation:spin 7s linear infinite}.spinning .r2{animation:spin 4s linear infinite reverse}.trigram{display:grid;gap:10px;transform:rotate(-48deg);filter:drop-shadow(0 0 12px var(--secondary))}.trigram span{width:86px;height:5px;background:var(--secondary)}.core{position:absolute;width:54px;height:54px;border:2px solid var(--primary);border-radius:50%;display:grid;place-items:center;transform:translate(32px,22px);box-shadow:0 0 24px rgba(235,178,255,.26)}.ritual-lines{position:absolute;display:grid;gap:13px;opacity:0}.ritual-lines span{width:138px;height:8px;background:linear-gradient(90deg,transparent 0 12%,var(--secondary) 12% 88%,transparent 88%);box-shadow:0 0 18px var(--secondary);transform:scaleX(.15)}.ritual-lines span:nth-child(2),.ritual-lines span:nth-child(5){background:linear-gradient(90deg,transparent 0 12%,var(--secondary) 12% 43%,transparent 43% 57%,var(--secondary) 57% 88%,transparent 88%)}.ritual-lines.casting{opacity:1}.ritual-lines.casting span{animation:castLine 2.8s steps(1,end) infinite}.ritual-lines.casting span:nth-child(2){animation-delay:.2s}.ritual-lines.casting span:nth-child(3){animation-delay:.4s}.ritual-lines.casting span:nth-child(4){animation-delay:.6s}.ritual-lines.casting span:nth-child(5){animation-delay:.8s}.ritual-lines.casting span:nth-child(6){animation-delay:1s}.scan-bar{position:absolute;left:20%;right:20%;height:2px;background:var(--secondary);box-shadow:0 0 20px var(--secondary);animation:scan 1.2s infinite}.terminal{width:min(690px,100%);text-align:center}.kicker{display:inline-flex;border:1px solid var(--panel);padding:8px 20px;color:#d0fcff;margin:12px 0}.copy{color:#d0fcff;font-size:1.08rem;letter-spacing:.08em;line-height:1.5}.sub{color:rgba(229,226,225,.62)}label{display:block;text-align:left;margin:0 0 14px}label span{display:block;color:var(--primary);font-family:"Space Mono";font-size:.72rem;font-weight:700;margin-bottom:8px}textarea{width:100%;min-height:58px;resize:vertical;border:0;border-bottom:1px solid var(--panel);background:rgba(0,238,250,.05);color:#fff;padding:14px;outline:none}.primary{width:min(575px,100%);min-height:62px;font-size:1.2rem;letter-spacing:.06em;box-shadow:inset 0 0 18px rgba(0,238,250,.18),5px 5px 0 rgba(235,178,255,.85)}.primary:disabled{cursor:wait;opacity:.66}.result-screen{width:min(1180px,100%);display:grid;gap:20px}.result-top{display:flex;justify-content:space-between;color:var(--secondary);border-bottom:1px solid rgba(208,252,255,.22);padding-bottom:12px}.result-screen h2{margin:0;font-family:"Space Grotesk";font-size:clamp(2.6rem,6vw,5.4rem);line-height:.95;color:#fff;text-shadow:4px 0 var(--secondary),-4px 0 var(--primary)}.result-omen .result-screen h2{color:#ff3b42;text-shadow:2px 0 #ff9f1a}.sync{margin:0;color:#9aa7a8}.result-layout{display:grid;grid-template-columns:minmax(280px,420px) 1fr;gap:30px}.hex,.reading{min-height:430px;border:1px solid var(--panel);background:rgba(5,5,5,.68);padding:34px;box-shadow:inset 0 0 28px rgba(235,178,255,.12)}.hex{display:grid;place-items:center;align-content:center}.result-omen .hex,.result-omen .reading{background:radial-gradient(circle,rgba(255,59,66,.16),rgba(5,5,5,.72) 66%);box-shadow:inset 4px 0 0 #ff3b42}.hex-lines{display:grid;gap:18px;margin-bottom:40px}.hex-lines span{width:240px;height:16px;background:var(--secondary);box-shadow:0 0 20px var(--secondary)}.hex-lines span:nth-child(2),.hex-lines span:nth-child(5){background:linear-gradient(90deg,var(--secondary) 0 43%,transparent 43% 57%,var(--secondary) 57% 100%)}.hex em{font-size:3rem;color:var(--primary);font-style:normal}.hex strong{font-size:1.4rem}.hex small{color:#9aa7a8;letter-spacing:.16em}.meta{display:flex;justify-content:space-between;color:var(--secondary)}.memory{color:#9aa7a8;overflow-wrap:anywhere}.oracle-text{font-size:1.45rem;color:#fff;line-height:1.7}.reading h3{color:var(--primary);font-family:"Space Mono";font-size:.92rem;margin:24px 0 8px;border-top:1px solid rgba(208,252,255,.18);padding-top:18px}.reading p{line-height:1.8;color:#d4c0d7}.tags{display:flex;flex-wrap:wrap;gap:10px;margin:20px 0}.tags span{border:1px solid var(--secondary);color:#d0fcff;padding:6px 10px}.compact{width:100%;font-size:1rem}.logs{align-self:end;display:flex;flex-wrap:wrap;justify-content:center;gap:8px 16px;max-width:760px;margin-top:24px;color:#d4c0d7;font-size:.72rem}.logs span{border-top:1px solid rgba(208,252,255,.35);padding-top:6px}footer{grid-column:1/-1;display:flex;align-items:center;justify-content:space-between;padding:0 80px;border-top:1px solid rgba(0,238,250,.25);background:rgba(8,8,10,.82);position:relative;z-index:1;color:#d4c0d7}footer span:first-child,footer span:last-child{color:var(--primary)}@keyframes spin{to{transform:rotate(360deg)}}@keyframes scan{0%{top:16%;opacity:0}20%,80%{opacity:1}100%{top:84%;opacity:0}}@keyframes castLine{0%{transform:scaleX(.15);opacity:.25}20%,100%{transform:scaleX(1);opacity:1}}@media(max-width:900px){.app-shell{grid-template-columns:1fr;grid-template-rows:auto auto 1fr auto}.topbar{padding:18px 16px;flex-wrap:wrap}.topbar nav{order:3;width:100%;justify-content:space-between;margin:0}.sidebar{grid-row:2/3;border-right:0;border-bottom:1px solid rgba(0,238,250,.32);padding:14px 16px;display:grid;gap:12px}.sidebar a{font-size:0;text-align:center;padding:12px}.sidebar a::first-letter{font-size:1.3rem}main{grid-row:3/4;padding:28px 16px}.wheel{width:min(86vw,390px)}.result-layout{grid-template-columns:1fr}.hex,.reading{min-height:auto}.result-top{flex-direction:column}footer{grid-row:4/5;padding:16px;flex-direction:column;align-items:flex-start}}@media(max-width:520px){.topbar h1{font-size:1.15rem}.topbar>span{font-size:.62rem}.result-screen h2{font-size:2.4rem}.hex-lines span{width:190px;height:12px}}
</style>
