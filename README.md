<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>数学课堂助手</title>
<style>
  :root{
    --bg:#eef2f9; --card:#fff; --primary:#2563eb;
    --green:#16a34a; --amber:#f59e0b; --purple:#7c3aed;
    --text:#1e293b; --muted:#64748b; --line:#e2e8f0;
  }
  *{box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
  body{
    margin:0; padding:20px 14px 40px;
    font-family:system-ui,-apple-system,"PingFang SC","Microsoft YaHei",sans-serif;
    background:var(--bg); color:var(--text); line-height:1.6;
    min-height:100vh;
  }
  .wrap{max-width:560px;margin:0 auto;}

  h1{
    font-size:24px; margin:0 0 6px;
    text-align:center;
  }
  .sub{
    font-size:14px; color:var(--muted);
    text-align:center; margin:0 0 26px;
  }

  /* ===== 通用卡片 ===== */
  .card{
    background:var(--card); border-radius:16px;
    box-shadow:0 6px 20px rgba(30,41,59,.07);
    overflow:hidden;
    margin-bottom:12px;
  }

  /* ===== 七年级上册按钮 ===== */
  .gradeCard{
    display:flex; align-items:center; justify-content:space-between;
    padding:20px 20px;
    cursor:pointer;
    transition:.2s;
  }
  .gradeCard:hover{
    transform:translateY(-2px);
    box-shadow:0 10px 26px rgba(37,99,235,.18);
  }
  .gradeCard .left{
    display:flex; align-items:center; gap:14px;
  }
  .gradeCard .icon{
    width:44px; height:44px; border-radius:12px;
    background:linear-gradient(135deg,#3b82f6,#2563eb);
    display:flex; align-items:center; justify-content:center;
    font-size:22px; color:#fff; flex-shrink:0;
  }
  .gradeCard .title{
    font-size:17px; font-weight:800; margin-bottom:2px;
  }
  .gradeCard .desc{
    font-size:12px; color:var(--muted);
  }
  .gradeCard .arrow{
    font-size:20px; color:#94a3b8;
    transition:transform .2s;
  }
  .gradeCard.open .arrow{
    transform:rotate(90deg);
  }

  /* ===== 章节列表 ===== */
  .chapterList{
    padding:0 16px 16px;
    display:none;
  }
  .chapterList.show{display:block;}

  .chapterItem{
    border:1px solid var(--line);
    border-radius:12px;
    margin-bottom:8px;
    overflow:hidden;
    background:#fafafa;
  }
  .chapterHeader{
    padding:14px 16px;
    display:flex; align-items:center; justify-content:space-between;
    cursor:pointer;
    transition:.15s;
  }
  .chapterHeader:hover{background:#f1f5f9;}
  .chapterHeader .cname{
    font-size:15px; font-weight:700;
  }
  .chapterHeader .cdesc{
    font-size:12px; color:var(--muted);
    margin-top:2px;
  }
  .chapterHeader .carrow{
    font-size:16px; color:#94a3b8;
    transition:transform .2s;
  }
  .chapterItem.open .chapterHeader .carrow{
    transform:rotate(90deg);
  }
  .chapterItem.disabled{
    opacity:.55;
    pointer-events:none;
  }
  .chapterItem.disabled .cname::after{
    content:"（待更新）";
    font-size:11px; font-weight:400;
    color:#94a3b8; margin-left:6px;
  }

  /* ===== 功能列表 ===== */
  .funcList{
    padding:0 12px 12px;
    display:none;
  }
  .funcList.show{display:block;}

  .funcItem{
    display:flex; align-items:center; gap:12px;
    padding:12px 14px;
    border-radius:10px;
    text-decoration:none;
    color:var(--text);
    background:#fff;
    border:1px solid var(--line);
    margin-bottom:8px;
    transition:.15s;
  }
  .funcItem:last-child{margin-bottom:0;}
  .funcItem:hover{
    background:#eff6ff;
    border-color:#93c5fd;
    transform:translateX(2px);
  }
  .funcItem .ficon{
    width:36px; height:36px; border-radius:9px;
    display:flex; align-items:center; justify-content:center;
    font-size:18px; flex-shrink:0; color:#fff;
  }
  .funcItem .ftext{flex:1;min-width:0;}
  .funcItem .ftitle{
    font-size:15px; font-weight:700;
  }
  .funcItem .fdesc{
    font-size:12px; color:var(--muted);
    margin-top:1px;
  }
  .funcItem .farrow{
    font-size:16px; color:#cbd5e1;
  }

  /* 不同功能的配色 */
  .c1{background:linear-gradient(135deg,#3b82f6,#2563eb);}
  .c2{background:linear-gradient(135deg,#10b981,#059669);}
  .c3{background:linear-gradient(135deg,#f59e0b,#d97706);}
  .c4{background:linear-gradient(135deg,#8b5cf6,#7c3aed);}
  .c5{background:linear-gradient(135deg,#ec4899,#db2777);}

  /* 未开放功能 */
  .funcItem.locked{
    background:#f8fafc;
    color:#94a3b8;
    cursor:not-allowed;
    pointer-events:none;
  }
  .funcItem.locked .ftitle{color:#94a3b8;}
  .funcItem.locked .fdesc{color:#cbd5e1;}
  .funcItem.locked .farrow{color:#e2e8f0;}

  /* 小提示 */
  .tip{
    text-align:center; font-size:12px;
    color:#94a3b8; margin-top:22px;
  }

  @media (max-width:420px){
    h1{font-size:21px;}
    .gradeCard{padding:16px;}
    .gradeCard .icon{width:40px;height:40px;font-size:20px;}
    .gradeCard .title{font-size:16px;}
    .chapterHeader{padding:12px 14px;}
    .funcItem{padding:10px 12px;gap:10px;}
    .funcItem .ficon{width:32px;height:32px;font-size:16px;}
  }
</style>
</head>
<body>
<div class="wrap">

  <h1>数学课堂助手</h1>
  <p class="sub">按教材目录选择章节，点击进入对应练习</p>

  <!-- ================= 七年级上册 ================= -->
  <div class="card">
    <div class="gradeCard open" id="gradeCard" data-target="gradeContent">
      <div class="left">
        <div class="icon">📘</div>
        <div>
          <div class="title">七年级上册</div>
          <div class="desc">湘教版 · 第一至四章</div>
        </div>
      </div>
      <div class="arrow">▶</div>
    </div>

    <div class="chapterList show" id="gradeContent">

      <!-- ---------- 第一章 有理数 ---------- -->
      <div class="chapterItem open" id="chapter1">
        <div class="chapterHeader" data-target="func1">
          <div>
            <div class="cname">第一章　有理数</div>
            <div class="cdesc">认识负数 · 数轴 · 相反数 · 绝对值 · 加减法</div>
          </div>
          <div class="carrow">▶</div>
        </div>

        <div class="funcList show" id="func1">

          <a class="funcItem" href="choubei.html">
            <div class="ficon c1">📚</div>
            <div class="ftext">
              <div class="ftitle">数学知识抽背</div>
              <div class="fdesc">随机抽查概念、法则、定义</div>
            </div>
            <div class="farrow">›</div>
          </a>

          <a class="funcItem" href="kousuanceshi.html">
            <div class="ficon c2">🔢</div>
            <div class="ftext">
              <div class="ftitle">口算过关</div>
              <div class="fdesc">有理数加减法口算，逐步提升</div>
            </div>
            <div class="farrow">›</div>
          </a>

          <a class="funcItem" href="shuxueceshi.html">
            <div class="ficon c3">📝</div>
            <div class="ftext">
              <div class="ftitle">限时计算测试</div>
              <div class="fdesc">限时计算，记录正确率</div>
            </div>
            <div class="farrow">›</div>
          </a>

          <!-- 以下两个待开放 -->
          <a class="funcItem locked" href="javascript:void(0)">
            <div class="ficon c4">📋</div>
            <div class="ftext">
              <div class="ftitle">单元测试</div>
              <div class="fdesc">整章综合检测，待更新</div>
            </div>
            <div class="farrow">›</div>
          </a>

          <a class="funcItem locked" href="javascript:void(0)">
            <div class="ficon c5">🚀</div>
            <div class="ftext">
              <div class="ftitle">专题提升</div>
              <div class="fdesc">易错题、拓展题精选，待更新</div>
            </div>
            <div class="farrow">›</div>
          </a>

        </div>
      </div>

      <!-- ---------- 第二章 代数式 ---------- -->
      <div class="chapterItem disabled">
        <div class="chapterHeader">
          <div>
            <div class="cname">第二章　代数式</div>
            <div class="cdesc">用字母表示数 · 整式 · 合并同类项</div>
          </div>
          <div class="carrow">▶</div>
        </div>
      </div>

      <!-- ---------- 第三章 一次方程组 ---------- -->
      <div class="chapterItem disabled">
        <div class="chapterHeader">
          <div>
            <div class="cname">第三章　一次方程组</div>
            <div class="cdesc">一元一次方程 · 二元一次方程组</div>
          </div>
          <div class="carrow">▶</div>
        </div>
      </div>

      <!-- ---------- 第四章 图形的认识 ---------- -->
      <div class="chapterItem disabled">
        <div class="chapterHeader">
          <div>
            <div class="cname">第四章　图形的认识</div>
            <div class="cdesc">线段 · 射线 · 直线 · 角</div>
          </div>
          <div class="carrow">▶</div>
        </div>
      </div>

    </div>
  </div>

  <p class="tip">更多年级与章节正在制作中…</p>

</div>

<script>
/* ================= 展开 / 收起 ================= */
function toggleItem(cardEl, contentEl){
  if(!contentEl) return;
  const isOpen = contentEl.classList.contains('show');
  if(isOpen){
    contentEl.classList.remove('show');
    cardEl.classList.remove('open');
  } else {
    contentEl.classList.add('show');
    cardEl.classList.add('open');
  }
}

/* 年级卡片：点击展开/收起章节列表 */
const gradeCard = document.getElementById('gradeCard');
const gradeContent = document.getElementById('gradeContent');
if(gradeCard && gradeContent){
  gradeCard.addEventListener('click', () => {
    toggleItem(gradeCard, gradeContent);
  });
}

/* 章节卡片：点击展开/收起功能列表 */
document.querySelectorAll('.chapterHeader').forEach(header => {
  header.addEventListener('click', () => {
    const item = header.closest('.chapterItem');
    if(item.classList.contains('disabled')) return;
    const targetId = header.dataset.target;
    if(!targetId) return;
    const content = document.getElementById(targetId);
    toggleItem(item, content);
  });
});

/* ================= 未开放功能的小提示 ================= */
document.querySelectorAll('.funcItem.locked').forEach(el => {
  el.addEventListener('click', e => {
    e.preventDefault();
    alert('该功能正在开发中，敬请期待！');
  });
});
</script>
</body>
</html>
