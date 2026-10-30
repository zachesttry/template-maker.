<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover,maximum-scale=1,user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#0b0b0f">
<title>Template Maker</title>
<style>
:root{
  --bg:#0b0b0f;--panel:#16161d;--panel2:#1f1f29;--line:#2c2c38;--text:#f4f4f7;--mute:#9a9aab;
  --accent:#ff3b5c;--accent2:#ffb800;--ok:#34d399;--vh:100vh;
  --sat:env(safe-area-inset-top,0px);--sab:env(safe-area-inset-bottom,0px);
}
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{margin:0;height:100%;background:var(--bg);color:var(--text);font-family:-apple-system,BlinkMacSystemFont,"SF Pro Text","Segoe UI",Roboto,sans-serif;overscroll-behavior:none}
body{overflow:hidden;-webkit-user-select:none;user-select:none;touch-action:manipulation}
input,textarea,select{-webkit-user-select:text;user-select:text;font:inherit;color:inherit}
button{font:inherit;color:inherit;border:0;background:none;cursor:pointer;touch-action:manipulation}
#app{height:var(--vh);display:flex;flex-direction:column;padding-top:var(--sat);position:relative;z-index:1;background:var(--bg)}
header{flex:0 0 52px;display:flex;align-items:center;gap:8px;padding:0 10px;border-bottom:1px solid var(--line);background:var(--bg);z-index:5}
header h1{flex:1;margin:0;font-size:17px;font-weight:700;text-align:center;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
#backBtn{width:64px;height:40px;font-size:30px;line-height:1;text-align:left;padding-left:4px;visibility:hidden}
#saveBtn{width:64px;height:34px;border-radius:17px;background:var(--panel2);font-size:14px;font-weight:700;position:relative}
#saveBtn.dirty::after{content:"";position:absolute;top:4px;right:6px;width:7px;height:7px;border-radius:50%;background:var(--accent2)}
#saveBtn.done{background:var(--ok);color:#04110b}
main{flex:1;position:relative;min-height:0}
.screen{position:absolute;inset:0;display:none;flex-direction:column;overflow-y:auto;-webkit-overflow-scrolling:touch}
.screen.on{display:flex}
.pad{padding:14px 14px calc(30px + var(--sab))}
.muted{color:var(--mute)} .small{font-size:13px}
h2{font-size:15px;margin:22px 2px 10px;color:var(--mute);text-transform:uppercase;letter-spacing:.06em;font-weight:700}
.card-p{background:var(--panel);border:1px solid var(--line);border-radius:16px;padding:14px;margin-bottom:12px}

/* Home */
.hero{padding:6px 2px 12px}
.hero b{font-size:26px;display:block;letter-spacing:-.02em}
.tiles{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.tile{background:var(--panel);border:1px solid var(--line);border-radius:18px;padding:16px 14px;text-align:left;min-height:108px;display:flex;flex-direction:column;justify-content:space-between;position:relative;overflow:hidden}
.tile .ic{font-size:28px}
.tile .tt{font-weight:800;font-size:16px;margin-top:8px}
.tile .ts{font-size:12px;color:var(--mute);margin-top:2px}
.tile.big{grid-column:span 2;min-height:92px;flex-direction:row;align-items:center;gap:14px;justify-content:flex-start}
.tile.big .tt{margin:0}
.tile.g1{background:linear-gradient(135deg,#2a1020,#16161d 70%)}
.tile.g2{background:linear-gradient(135deg,#2a2208,#16161d 70%)}
.tile.g3{background:linear-gradient(135deg,#0c2420,#16161d 70%)}
.banner{background:#2a1a08;border:1px solid #5a3a10;color:#ffd38a;border-radius:12px;padding:10px 12px;font-size:13px;margin-bottom:12px}
.banner.bad{background:#2a0c12;border-color:#6a1a28;color:#ff9aaa}

/* Buttons */
.btn{height:46px;padding:0 16px;border-radius:14px;background:var(--panel2);font-weight:700;font-size:15px;display:inline-flex;align-items:center;justify-content:center;gap:6px;white-space:nowrap}
.btn.primary{background:var(--accent);color:#fff;flex:1}
.btn.ghost{background:var(--panel2)}
.btn.sq{width:46px;flex:0 0 46px;padding:0;font-size:18px}
.btn.disabled{opacity:.35;pointer-events:none}
.btn.full{width:100%}
.btn.warn{background:#3a1018;color:#ff8a9c}
.btn.okb{background:var(--ok);color:#04110b}
.row{display:flex;gap:8px;align-items:center}
.wrap{flex-wrap:wrap}

/* Game screens */
.game{overflow:hidden}
.stageWrap{flex:1;min-height:0;display:flex;align-items:center;justify-content:center;padding:8px;position:relative}
.stage{position:relative;background:#000;border-radius:14px;overflow:hidden;--sw:360px;flex:0 0 auto}
.controls{flex:0 0 auto;display:flex;gap:8px;padding:8px 10px calc(10px + var(--sab));align-items:center}
.m{position:absolute;inset:0;width:100%;height:100%;object-fit:cover;display:block;-webkit-user-drag:none}
.empty{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;padding:24px;color:var(--mute);gap:14px;font-size:15px}

/* captions */
.cap{position:absolute;left:5%;right:5%;top:4.5%;z-index:4;text-align:center;font-weight:800;line-height:1.15;font-size:calc(var(--sw) * .062);pointer-events:none;word-wrap:break-word}
.cap span{display:inline;-webkit-box-decoration-break:clone;box-decoration-break:clone;padding:.12em .38em;border-radius:.3em}
.cap.k-outline span{color:#fff;text-shadow:0 0 3px #000,0 0 3px #000,0 2px 6px rgba(0,0,0,.8)}
.cap.k-white span{background:#fff;color:#000}
.cap.k-black span{background:rgba(0,0,0,.82);color:#fff}
.cap.k-accent span{background:var(--accent);color:#fff}
.cap.k-yellow span{color:#ffe14d;text-shadow:0 0 3px #000,0 0 3px #000,0 2px 6px rgba(0,0,0,.8)}

/* WYR */
.wyr-top{position:absolute;left:0;right:0;top:0;overflow:hidden;background:#000}
.cards{position:absolute;left:0;right:0;bottom:0;display:grid;gap:4px;padding:4px;background:#000}
.cards.c2{grid-template-columns:1fr 1fr}
.cards.c3{grid-template-columns:1fr 1fr 1fr}
.cards.c4{grid-template:1fr 1fr/1fr 1fr}
.card{position:relative;overflow:hidden;border-radius:10px;background:#111;min-height:0;min-width:0;cursor:pointer;transition:transform .12s}
.card::after{content:"";position:absolute;inset:0;border:2px solid transparent;border-radius:inherit;pointer-events:none;transition:border-color .1s}
.card.sel::after{border-color:#fff;box-shadow:inset 0 0 0 1px rgba(0,0,0,.35)}
.card.sel{transform:scale(.975)}
.res2{position:absolute;inset:0;display:grid;grid-template-columns:1fr 1fr;gap:4px;padding:4px}
.res2 .cell{position:relative;overflow:hidden;border-radius:10px;background:#111}

/* WHB */
.whb-head{position:absolute;left:0;right:0;top:0;height:15%;display:flex;align-items:center;justify-content:center;padding:0 5%}
.whb-head .cap{position:static;left:auto;right:auto;top:auto}
.whb-grid{position:absolute;left:0;right:0;top:15%;bottom:0;display:grid;gap:4px;padding:4px}
.whb-grid.n1{grid-template:1fr/1fr}
.whb-grid.n2{grid-template-rows:1fr 1fr}
.whb-grid.n3{grid-template-rows:1fr 1fr 1fr}
.whb-grid.n4{grid-template:1fr 1fr/1fr 1fr}
.whb-grid.n5,.whb-grid.n6{grid-template:1fr 1fr 1fr/1fr 1fr}
.ctl-info{font-size:13px;color:var(--mute);text-align:center;min-width:64px}
.ctl-info b{color:var(--text);font-size:15px;display:block}

/* Roulette player layouts */
.lay{position:absolute;inset:0;display:grid;gap:3px;background:#000}
.lay.gw{background:#fff}
.lay-full{grid-template:1fr/1fr}
.lay-stack3{grid-template-rows:1fr 1fr 1fr}
.lay-stack2{grid-template-rows:1fr 1fr}
.lay-grid4{grid-template:1fr 1fr/1fr 1fr}
.lay-side2{grid-template-columns:1fr 1fr}
.lay-grid9{grid-template:repeat(3,1fr)/repeat(3,1fr)}
.lay-pip{display:block}
.lay-pip .s0{position:absolute;inset:0}
.lay-pip .s1{position:absolute;width:44%;height:36%;right:6%;bottom:9%;border:3px solid #fff;border-radius:12px;box-shadow:0 8px 30px rgba(0,0,0,.6)}
.slot{position:relative;overflow:hidden;min-height:0;min-width:0;background:#000}
#vidHolder{position:fixed;left:0;top:0;width:120px;z-index:0;pointer-events:none;display:flex;flex-wrap:wrap}
#vidHolder video{width:60px;height:107px;object-fit:cover}
.p-end{position:absolute;inset:0;z-index:6;background:rgba(0,0,0,.6);display:flex;flex-direction:column;align-items:center;justify-content:center;gap:12px}
.p-end .btn{width:62%}
.p-controls{flex-direction:column;align-items:stretch;gap:6px}
.p-bar{display:flex;align-items:center;gap:10px;font-size:12px;color:var(--mute);font-variant-numeric:tabular-nums}
.p-bar input{flex:1}
.p-btns{display:flex;gap:8px;justify-content:space-between}
.p-btns .btn{flex:1;padding:0}
#s-player.fs .stageWrap{position:fixed;inset:0;z-index:50;background:#000;padding:0}
#s-player.fs .stage{border-radius:0}
#s-player.fs .p-controls{position:fixed;left:0;right:0;bottom:0;z-index:51;background:linear-gradient(transparent,rgba(0,0,0,.85) 40%);padding-top:30px;transition:opacity .2s}
#s-player.fs.hidectl .p-controls{opacity:0;pointer-events:none}
body.fsmode header{display:none}
input[type=range]{-webkit-appearance:none;appearance:none;height:28px;background:transparent}
input[type=range]::-webkit-slider-runnable-track{height:4px;border-radius:2px;background:var(--line)}
input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:20px;height:20px;border-radius:50%;background:#fff;margin-top:-8px}
.chip-snd{position:absolute;z-index:7;right:10px;bottom:10px;background:rgba(0,0,0,.7);color:#fff;border-radius:20px;padding:8px 12px;font-size:13px;font-weight:700}

/* Forms */
.field{display:flex;align-items:center;justify-content:space-between;gap:12px;padding:12px 0;border-bottom:1px solid var(--line)}
.field:last-child{border-bottom:0}
.field .lb{font-size:15px}
.field .lb small{display:block;color:var(--mute);font-size:12px;margin-top:2px}
.sw{position:relative;width:50px;height:30px;flex:0 0 auto}
.sw input{opacity:0;width:0;height:0;position:absolute}
.sw i{position:absolute;inset:0;border-radius:15px;background:var(--line);transition:.15s}
.sw i::after{content:"";position:absolute;width:24px;height:24px;left:3px;top:3px;border-radius:50%;background:#fff;transition:.15s}
.sw input:checked+i{background:var(--accent)}
.sw input:checked+i::after{transform:translateX(20px)}
.sl{display:block;padding:10px 0;border-bottom:1px solid var(--line)}
.sl:last-child{border-bottom:0}
.sl .top{display:flex;justify-content:space-between;font-size:15px}
.sl .top b{color:var(--accent2);font-variant-numeric:tabular-nums}
.sl input{width:100%}
.chips{display:flex;gap:6px}
.chip{min-width:42px;height:34px;border-radius:17px;background:var(--panel2);border:1px solid var(--line);font-weight:700;padding:0 12px}
.chip.on{background:var(--accent);border-color:var(--accent);color:#fff}
.seg{display:flex;background:var(--panel2);border-radius:12px;padding:3px;gap:3px}
.seg button{flex:1;height:36px;border-radius:10px;font-weight:700;font-size:14px;color:var(--mute)}
.seg button.on{background:var(--text);color:#000}

/* Prompt editor */
.pe-add{display:flex;gap:6px;margin-bottom:10px}
.pe-add input[type=text]{flex:1;min-width:0}
.tx{height:44px;border-radius:12px;background:var(--panel2);border:1px solid var(--line);padding:0 12px;font-size:16px;outline:none}
.tx:focus{border-color:var(--accent)}
select.tx{padding:0 8px;-webkit-appearance:none;appearance:none;text-align:center;font-weight:700}
.pe-row{display:flex;gap:6px;align-items:center;margin-bottom:6px}
.pe-row .tx{height:40px;font-size:15px}
.pe-row .tx.grow{flex:1;min-width:0}
.pe-del{width:36px;height:36px;border-radius:10px;background:#2a1018;color:#ff8a9c;font-size:18px;flex:0 0 auto}
.pe-radio{width:26px;height:26px;border-radius:50%;border:2px solid var(--line);flex:0 0 auto;position:relative}
.pe-radio.on{border-color:var(--accent)}
.pe-radio.on::after{content:"";position:absolute;inset:4px;border-radius:50%;background:var(--accent)}
.pe-meta{font-size:11px;color:var(--mute);text-align:center;line-height:1.1}
details.sec{background:var(--panel);border:1px solid var(--line);border-radius:16px;margin-bottom:10px;overflow:hidden}
details.sec summary{list-style:none;padding:14px;font-weight:800;display:flex;justify-content:space-between;align-items:center}
details.sec summary::-webkit-details-marker{display:none}
details.sec summary::after{content:"›";transform:rotate(90deg);color:var(--mute);font-size:22px;transition:.15s}
details.sec[open] summary::after{transform:rotate(-90deg)}
details.sec .body{padding:0 14px 14px}

/* Media */
.tabs{display:flex;gap:6px;margin-bottom:12px}
.tabs button{flex:1;height:40px;border-radius:12px;background:var(--panel);border:1px solid var(--line);font-weight:800}
.tabs button.on{background:var(--text);color:#000}
.mgrid{display:grid;grid-template-columns:repeat(3,1fr);gap:4px}
.mitem{position:relative;aspect-ratio:3/4;border-radius:8px;overflow:hidden;background:var(--panel2)}
.mitem img,.mitem video{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
.mitem .dur{position:absolute;right:4px;bottom:4px;background:rgba(0,0,0,.7);font-size:11px;padding:2px 5px;border-radius:5px;font-weight:700}
.mitem .ck{position:absolute;top:5px;right:5px;width:22px;height:22px;border-radius:50%;border:2px solid #fff;background:rgba(0,0,0,.3);display:none}
.selecting .mitem .ck{display:block}
.mitem.sel .ck{background:var(--accent);border-color:var(--accent)}
.mitem.sel .ck::after{content:"✓";position:absolute;inset:0;font-size:13px;font-weight:900;display:flex;align-items:center;justify-content:center}
.mitem.sel{outline:2px solid var(--accent);outline-offset:-2px}
.mbar{position:sticky;bottom:0;display:flex;gap:8px;padding:10px 0 calc(6px + var(--sab));background:linear-gradient(transparent,var(--bg) 30%)}

/* Ads */
.ad-back{position:absolute;inset:0;z-index:40;background:rgba(0,0,0,.55);display:flex;align-items:center;justify-content:center;animation:fadeIn .18s}
body>.ad-back{position:fixed}
.ad-back.v-sheet{align-items:flex-end}
.ad{position:relative;width:86%;max-width:420px;background:#fff;color:#111;border-radius:18px;overflow:hidden;box-shadow:0 20px 60px rgba(0,0,0,.5);animation:pop .22s cubic-bezier(.2,1.4,.4,1)}
.v-sheet .ad{width:100%;max-width:none;border-radius:20px 20px 0 0;animation:up .25s ease-out}
.v-full .ad{width:100%;height:100%;max-width:none;border-radius:0;display:flex;flex-direction:column}
.ad-head{display:flex;align-items:center;gap:8px;padding:9px 10px;font-size:12px;color:#666;font-weight:600}
.v-full .ad-head{position:absolute;top:0;left:0;right:0;z-index:3;color:#fff;background:linear-gradient(rgba(0,0,0,.55),transparent)}
.ad-badge{background:#ffcc00;color:#000;font-weight:900;font-size:11px;padding:2px 6px;border-radius:4px}
.ad-x{margin-left:auto;width:30px;height:30px;border-radius:50%;background:rgba(0,0,0,.08);font-weight:800;font-size:14px;color:inherit}
.v-full .ad-x{background:rgba(0,0,0,.45)}
.ad-x.wait{opacity:.6;pointer-events:none}
.ad-img{position:relative;width:100%;aspect-ratio:4/5;background:#000;overflow:hidden}
.v-sheet .ad-img{aspect-ratio:16/11}
.v-full .ad-img{flex:1;aspect-ratio:auto}
.ad-txt{padding:12px 14px 4px;font-weight:800;font-size:calc(var(--sw,360px) * .056);line-height:1.2}
body>.ad-back .ad-txt{font-size:20px}
.v-full .ad-txt{position:absolute;left:0;right:0;bottom:84px;z-index:2;color:#fff;text-shadow:0 2px 10px rgba(0,0,0,.7);font-size:calc(var(--sw,360px) * .07);text-align:center;padding:0 8%}
.v-full .ad-txt.k{bottom:auto;top:50%}
.ad-cta{display:block;margin:10px 14px 14px;width:calc(100% - 28px);height:44px;border-radius:12px;background:#0a84ff;color:#fff;font-weight:800;font-size:15px}
.v-full .ad-cta{position:absolute;left:8%;right:8%;width:84%;margin:0;bottom:calc(22px + var(--sab));z-index:2}
.v-sheet .ad-cta{margin-bottom:calc(14px + var(--sab))}
.ad-cards{display:grid;gap:5px;padding:4px 10px}
.ad-cards.n2{grid-template-columns:1fr 1fr}
.ad-cards.n4,.ad-cards.n6{grid-template-columns:1fr 1fr}
.ad-cards.n3{grid-template-columns:1fr 1fr 1fr}
.ad-cards .card{aspect-ratio:3/4}
.ad-cards.n6 .card{aspect-ratio:1/1}
.v-full .ad-cards{flex:1;padding-top:8px;align-content:center}
.ad-cards .card.sel::after{border-color:#0a84ff;border-width:2px}
.ad-sub{text-align:center;font-size:12px;color:#777;padding:2px}
.v-full .ad-sub{color:#ccc}
.v-full .ad .ad-txt.mc{position:static;color:#fff;padding:58px 14px 4px;text-align:center}
.v-full .ad{background:#111;color:#fff}
@keyframes pop{from{transform:scale(.8);opacity:0}to{transform:scale(1);opacity:1}}
@keyframes up{from{transform:translateY(100%)}to{transform:none}}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}

/* Modal / toast */
.mod-back{position:fixed;inset:0;z-index:100;background:rgba(0,0,0,.6);display:flex;align-items:center;justify-content:center;padding:20px;animation:fadeIn .15s}
.mod{width:100%;max-width:400px;background:var(--panel);border:1px solid var(--line);border-radius:18px;padding:18px;max-height:85vh;overflow:auto}
.mod h3{margin:0 0 8px;font-size:18px}
.mod p{margin:0 0 14px;color:var(--mute);font-size:14px;line-height:1.4}
.mod .row{margin-top:8px}
.mod .btn{flex:1}
.preview{position:relative;width:100%;aspect-ratio:3/4;border-radius:12px;overflow:hidden;background:#000;margin-bottom:10px}
.preview .m{object-fit:contain}
#toast{position:fixed;left:50%;bottom:calc(90px + var(--sab));transform:translateX(-50%) translateY(20px);background:#fff;color:#000;padding:11px 16px;border-radius:14px;font-weight:700;font-size:14px;opacity:0;pointer-events:none;transition:.2s;z-index:200;max-width:90vw;text-align:center;box-shadow:0 10px 30px rgba(0,0,0,.4)}
#toast.on{opacity:1;transform:translateX(-50%)}
.prog{position:fixed;inset:0;z-index:150;background:rgba(0,0,0,.7);display:flex;align-items:center;justify-content:center;flex-direction:column;gap:12px;font-weight:700}
.prog .bar{width:60%;height:6px;background:var(--line);border-radius:3px;overflow:hidden}
.prog .bar i{display:block;height:100%;width:0;background:var(--accent);transition:width .15s}
/* v2 */
body.gamemode header{flex-basis:44px;border-bottom:0}
body.gamemode #title{visibility:hidden}
body.gamemode #saveBtn{height:30px;font-size:13px}
body.gamemode .stageWrap{padding:0;flex:0 0 auto;height:calc(var(--vh) - 44px - var(--sat))}
body.gamemode .screen.game{overflow-y:auto}
body.gamemode #s-player.fs .stageWrap{height:auto}
body.gamemode .stage{border-radius:0}
body.gamemode .controls{padding-top:6px}
.cap span.who.who{background:var(--accent);color:#fff;text-shadow:none}
.ad-txt .who{background:var(--accent);color:#fff;border-radius:.3em;padding:.08em .35em;-webkit-box-decoration-break:clone;box-decoration-break:clone}
#convPill{position:fixed;left:50%;transform:translateX(-50%);top:calc(60px + var(--sat));z-index:90;background:#fff;color:#000;font-weight:700;font-size:13px;padding:7px 12px;border-radius:16px;box-shadow:0 6px 20px rgba(0,0,0,.4);pointer-events:none;white-space:nowrap}
.mitem .ph{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;font-size:28px}
.v-full .mcbox .ad-cta{position:static;width:calc(100% - 28px);margin:8px 14px 14px}
.v-full .mcbox .ad-cards{min-height:0;grid-auto-rows:1fr}
.v-full .mcbox .ad-cards .card{aspect-ratio:auto}
.ad-cards.n5,.ad-cards.n6{grid-template-columns:1fr 1fr 1fr}
.ad-cards.n5 .card,.ad-cards.n6 .card{aspect-ratio:3/4}
.vc{position:absolute;inset:0;width:100%;height:100%;display:block}
.nm{position:absolute;left:0;right:0;bottom:0;z-index:2;padding:18% 6% 6%;background:linear-gradient(transparent,rgba(0,0,0,.78));color:#fff;font-weight:800;text-align:center;font-size:calc(var(--sw,360px) * .042);line-height:1.1;pointer-events:none;text-shadow:0 1px 3px rgba(0,0,0,.6);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.cards.c3 .nm,.whb-grid.n5 .nm,.whb-grid.n6 .nm,.ad-cards.n3 .nm,.ad-cards.n6 .nm{font-size:calc(var(--sw,360px) * .034)}
.adon .controls{opacity:.3;pointer-events:none}
.ad-back.v-full{background:#000;animation:none}
.v-full .ad{animation:fadeIn .15s}
.tiles.t3{grid-template-columns:repeat(3,1fr)}
.t3 .tile{min-height:92px;padding:12px 10px;border-radius:16px}
.t3 .tile .ic{font-size:24px}.t3 .tile .tt{font-size:14px;margin-top:6px}.t3 .tile .ts{font-size:11px}
.mitem .tag{position:absolute;left:4px;right:4px;bottom:4px;font-size:11px;font-weight:700;background:rgba(0,0,0,.7);padding:2px 5px;border-radius:5px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.plist{display:flex;flex-direction:column}
.pr{display:flex;align-items:center;gap:12px;padding:9px 4px;border-bottom:1px solid var(--line);text-align:left;width:100%}
.av{width:42px;height:42px;border-radius:50%;background:var(--panel2);overflow:hidden;flex:0 0 42px;display:flex;align-items:center;justify-content:center;font-weight:800;font-size:14px;color:var(--mute)}
.av img{width:100%;height:100%;object-fit:cover}
.pn{flex:1;font-weight:700;font-size:15px;min-width:0;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.pc{color:var(--mute);font-size:13px;font-variant-numeric:tabular-nums}
.btn.nofx{flex:0 0 auto}
</style>
</head>
<body>
<div id="app">
  <header>
    <button id="backBtn" aria-label="Back">‹</button>
    <h1 id="title">Template Maker</h1>
    <button id="saveBtn">Save</button>
  </header>
  <main>
    <section class="screen on" id="s-home"><div class="pad" id="homeBody"></div></section>

    <section class="screen game" id="s-wyr">
      <div class="stageWrap"><div class="stage" id="wyrStage"></div></div>
      <div class="controls" id="wyrCtl"></div>
    </section>

    <section class="screen game" id="s-whb">
      <div class="stageWrap"><div class="stage" id="whbStage"></div></div>
      <div class="controls" id="whbCtl"></div>
    </section>

    <section class="screen" id="s-roul"><div class="pad" id="roulBody"></div></section>

    <section class="screen game" id="s-player">
      <div class="stageWrap"><div class="stage" id="pStage"></div></div>
      <div class="controls p-controls">
        <div class="p-bar"><span id="pCur">0:00</span><input type="range" id="pSeek" min="0" max="30" step="0.01" value="0"><span id="pTot">0:30</span></div>
        <div class="p-btns">
          <button class="btn" id="pBack">⟲ 5</button>
          <button class="btn primary" id="pPlay">▶</button>
          <button class="btn" id="pFwd">5 ⟳</button>
          <button class="btn" id="pFs">⛶</button>
          <button class="btn" id="pNew">🎲 New</button>
        </div>
      </div>
    </section>

    <section class="screen" id="s-media"><div class="pad" id="mediaBody"></div></section>
    <section class="screen" id="s-prompts"><div class="pad" id="promptsBody"></div></section>
    <section class="screen" id="s-ads"><div class="pad" id="adsBody"></div></section>
    <section class="screen" id="s-settings"><div class="pad" id="setBody"></div></section>
    <section class="screen" id="s-people"><div class="pad" id="peopleBody"></div></section>
    <section class="screen" id="s-person"><div class="pad" id="personBody"></div></section>
  </main>
</div>
<div id="vidHolder"></div>
<div id="toast"></div>
<input type="file" id="filePhotos" accept="image/*" multiple hidden>
<input type="file" id="fileClips" accept="video/*" multiple hidden>
<input type="file" id="fileBackup" hidden>

<script>
'use strict';
/* =========================================================
   Helpers
   ========================================================= */
// ignore empty slots when building screens
['replaceChildren','append'].forEach(fn=>{const o=Element.prototype[fn];Element.prototype[fn]=function(...k){return o.apply(this,k.flat(9).filter(x=>x!=null&&x!==false))}});
const $=(s,r=document)=>r.querySelector(s), $$=(s,r=document)=>[...r.querySelectorAll(s)];
const rand=(a,b)=>a+Math.random()*(b-a), randInt=(a,b)=>Math.floor(rand(a,b+1));
const pick=a=>a[Math.floor(Math.random()*a.length)];
const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));
function shuffle(a){a=a.slice();for(let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]]}return a}
const uid=()=>Date.now().toString(36)+Math.random().toString(36).slice(2,8);
const fmtT=s=>{s=Math.max(0,s);return Math.floor(s/60)+':'+String(Math.floor(s%60)).padStart(2,'0')};
function h(tag,props,...kids){
  const e=document.createElement(tag);
  if(props)for(const k in props){const v=props[k];if(v==null||v===false)continue;
    if(k==='class')e.className=v;else if(k==='style')e.style.cssText=v;
    else if(k.startsWith('on')&&typeof v==='function')e.addEventListener(k.slice(2),v);
    else if(k==='html')e.innerHTML=v;
    else if(k in e&&typeof v!=='string')e[k]=v;else e.setAttribute(k,v===true?'':v)}
  for(const c of kids.flat(9))if(c!=null&&c!==false)e.append(c.nodeType?c:String(c));
  return e;
}
const btn=(label,fn,cls='')=>h('button',{class:'btn '+cls,onclick:fn},label);
function toast(msg,ms=2200){const t=$('#toast');t.textContent=msg;t.classList.add('on');clearTimeout(t._t);t._t=setTimeout(()=>t.classList.remove('on'),ms)}
// Custom dialogs (phone code apps often block alert/confirm)
function modal(title,text,buttons,extra){
  return new Promise(res=>{
    const back=h('div',{class:'mod-back'});
    const close=v=>{back.remove();res(v)};
    back.append(h('div',{class:'mod'},h('h3',null,title),text?h('p',null,text):null,extra||null,
      h('div',{class:'row'},buttons.map(b=>btn(b.label,()=>close(b.value),b.cls||'')))));
    back.addEventListener('click',e=>{if(e.target===back)close(null)});
    document.body.append(back);
  });
}
const confirmBox=(title,text,yes='Yes',cls='primary')=>modal(title,text,[{label:'Cancel',value:false},{label:yes,value:true,cls}]);
/* Clip playback has two modes:
   • HD video — real <video>, painted onto a canvas (used in Safari / home-screen app).
   • Frames  — for viewers like Koder that force every video fullscreen: each clip is
     turned into 24 fps frames once and flipped on a canvas; sound comes from <audio>.
   Auto mode picks HD unless the viewer is an in-app browser, and switches to Frames
   for good the moment the phone tries to take a video fullscreen. */
const FPS=24,MAXS=15,LONG=720;
const Flips=new Set();
const PLAY={real:true,
  isInApp(){const ua=navigator.userAgent,ios=/iP(hone|ad|od)/.test(ua)||(navigator.platform==='MacIntel'&&navigator.maxTouchPoints>1);return ios&&!/Safari\//.test(ua)&&!navigator.standalone},
  decide(){
    const pref=S.settings.playMode||'auto';let forced=false;try{forced=localStorage.getItem('tm_forcedfs')==='1'}catch(e){}
    return pref==='real'?true:pref==='frames'?false:!(forced||this.isInApp());
  },
  apply(real){
    if(real===this.real&&this._applied)return;this._applied=true;
    const was=[...Flips].map(f=>[f,f._mid,!f.paused]);
    Flips.forEach(f=>f._stopAll());this.real=real;
    was.forEach(([f,id,pl])=>{f._mid=null;if(id){f.setClip(id);if(pl)f.play()}});
    if(!real)Conv.add(S.media.filter(m=>m.kind==='clip').map(m=>m.id));
  },
  forced(v){
    try{v.webkitExitFullscreen&&v.webkitExitFullscreen()}catch(e){}
    if((S.settings.playMode||'auto')!=='auto')return;
    try{localStorage.setItem('tm_forcedfs','1')}catch(e){}
    this.apply(false);
    toast('This viewer forces videos fullscreen — switched to Frames mode. Open the app in Safari for HD video.',5000);
  }
};
class Flip{
  constructor(){this._mid=null;this.fr=null;this.t=0;this.paused=true;this._m=true;this._wantSound=false;this.audio=null;this._asrc=null;this.vid=null;this.imgs=new Map();this._last=null;this.lastNow=performance.now();this._prep=-1;this._pend=null;Flips.add(this)}
  _vidEl(){
    if(!this.vid){const v=document.createElement('video');
      v.setAttribute('playsinline','');v.setAttribute('webkit-playsinline','');v.playsInline=true;
      v.setAttribute('disablepictureinpicture','');v.setAttribute('x-webkit-airplay','deny');v.loop=true;v.preload='auto';v.muted=true;
      v.addEventListener('webkitbeginfullscreen',()=>PLAY.forced(v));
      v.addEventListener('loadedmetadata',()=>{if(this._pend!=null&&PLAY.real){const t=this._pend;this._pend=null;this.currentTime=t}});
      $('#vidHolder').append(v);this.vid=v}
    return this.vid;
  }
  _stopAll(){if(this.vid)this.vid.pause();if(this.audio)this.audio.pause()}
  get duration(){if(PLAY.real){const d=this.vid&&this.vid.duration;return isFinite(d)&&d>0?d:0}return this.fr?this.fr.n/this.fr.fps:0}
  get readyState(){return PLAY.real?(this.vid?this.vid.readyState:0):(this.fr?4:0)}
  get muted(){return this._m}
  set muted(v){this._m=!!v;if(PLAY.real){if(this.vid)this.vid.muted=this._m||S.settings.clipSound===false}else this._sync(false)}
  get currentTime(){return PLAY.real?(this.vid?this.vid.currentTime:0):this.t}
  set currentTime(t){
    const d=this.duration;if(!d){this._pend=t;this.t=0;return}
    this._pend=null;const w=((t%d)+d)%d;
    if(PLAY.real){try{this.vid.currentTime=w}catch(e){}return}
    this.t=w;this._prime();
    if(this.audio&&!this.audio.paused&&this._asrc===this._mid){try{this.audio.currentTime=this.t}catch(e){}}
  }
  setClip(id){
    if(this._mid===id)return;this._mid=id;this.imgs.clear();this._last=null;this.t=0;this._pend=null;this.fr=FR.map.get(id)||null;
    if(this.audio&&!this.audio.paused)this.audio.pause();
    if(PLAY.real){const v=this._vidEl();if(v._mid!==id){v._mid=id;v.src=url(id)}}
  }
  play(){
    this.paused=false;this.lastNow=performance.now();VR.start();
    if(PLAY.real){const v=this._vidEl();v.muted=this._m||S.settings.clipSound===false;const p=v.play();
      if(p)p.catch(e=>{if(e&&e.name==='NotAllowedError'&&!v.muted){v.muted=true;this._m=true;v.play().catch(()=>{});audioBlocked()}});return Promise.resolve()}
    this._sync(true);return Promise.resolve();
  }
  pause(){this.paused=true;this._stopAll()}
  unlock(){ // called inside a tap so sound is allowed later
    const first=S.media.find(m=>m.kind==='clip');if(!first)return;
    const el=PLAY.real?this._vidEl():this._audioEl();
    if(!el.getAttribute('src')){el.src=first.url;if(PLAY.real)el._mid=first.id;else this._asrc=first.id}
    el.muted=false;const p=el.play();p&&p.catch(()=>{});el.pause();if(PLAY.real)el.muted=this._m;
  }
  _audioEl(){if(!this.audio){const a=document.createElement('audio');a.preload='auto';a.loop=true;a.setAttribute('playsinline','');$('#vidHolder').append(a);this.audio=a}return this.audio}
  _sync(force){
    const want=!PLAY.real&&!this.paused&&!this._m&&!!this.fr&&S.settings.clipSound!==false;
    if(!want){if(this.audio&&!this.audio.paused)this.audio.pause();return}
    const el=this._audioEl();
    if(this._asrc!==this._mid){this._asrc=this._mid;el.src=url(this._mid);el.addEventListener('loadedmetadata',()=>{try{el.currentTime=this.t}catch(e){}},{once:true})}
    else if(force&&el.readyState>=1){try{el.currentTime=this.t}catch(e){}}
    if(el.paused){const p=el.play();p&&p.catch(e=>{if(e&&e.name==='NotAllowedError'){this._m=true;audioBlocked()}})}
  }
  tick(now){
    const dt=Math.min(.1,Math.max(0,(now-this.lastNow)/1000));this.lastNow=now;
    if(PLAY.real)return;
    if(!this.fr&&this._mid&&FR.map.has(this._mid)){this.fr=FR.map.get(this._mid);if(this._pend!=null)this.currentTime=this._pend;if(!this.paused)this._sync(true)}
    if(this.paused||!this.fr)return;
    const d=this.duration,a=this.audio;
    if(a&&!a.paused&&!this._m&&a.readyState>=2&&this._asrc===this._mid){let t=a.currentTime;if(t>=d){t%=d;try{a.currentTime=t}catch(e){}}this.t=t}
    else this.t=(this.t+dt)%d;
  }
  _load(k){if(this.imgs.has(k))return;const fr=this.fr,im=new Image(),u=URL.createObjectURL(fr.blob.slice(fr.offs[k],fr.offs[k+1],'image/jpeg'));
    im.onload=()=>{im._ok=true;URL.revokeObjectURL(u)};im.onerror=()=>URL.revokeObjectURL(u);im.src=u;this.imgs.set(k,im)}
  _idx(){return Math.min(this.fr.n-1,Math.floor(this.t*this.fr.fps))}
  _prime(){if(!this.fr)return;const i=this._idx(),n=this.fr.n;for(let k=0;k<12;k++)this._load((i+k)%n)}
  frame(){
    if(PLAY.real){const v=this.vid;return v&&v.readyState>=2&&v.videoWidth?v:null}
    if(!this.fr)return this._last;
    const i=this._idx(),n=this.fr.n;this._prime();
    const im=this.imgs.get(i);if(im&&im._ok)this._last=im;
    if(this.imgs.size>20)for(const k of [...this.imgs.keys()])if(((k-i+n)%n)>14)this.imgs.delete(k);
    return this._last;
  }
}
function audioBlocked(){
  gvMuted=true;Player.forceMute=true;
  const st=$('.screen.on .stage');
  st&&soundChip(st,()=>{gvMuted=false;Player.forceMute=false;Flips.forEach(f=>{if(f._wantSound&&!f.paused){f.muted=false;if(PLAY.real)f.play()}})});
}
const VR={list:new Set(),raf:0,
  canvas(f){const c=document.createElement('canvas');c.className='vc';c._v=f;this.list.add(c);this.start();return c},
  start(){if(this.raf)return;const fn=now=>{Flips.forEach(f=>f.tick(now));this.draw();this.raf=this.list.size?requestAnimationFrame(fn):0};this.raf=requestAnimationFrame(fn)},
  draw(){
    for(const c of this.list){
      if(!c.isConnected){this.list.delete(c);continue}
      const src=c._v.frame();if(!src)continue;
      const iw=src.videoWidth||src.naturalWidth,ih=src.videoHeight||src.naturalHeight;if(!iw||!ih)continue;
      const dpr=Math.min(2.5,window.devicePixelRatio||1),w=Math.round(c.clientWidth*dpr),hh=Math.round(c.clientHeight*dpr);
      if(!w||!hh)continue;
      const tt=src.videoWidth?src.currentTime:0;
      if(c._img===src&&c._t===tt&&c.width===w&&c.height===hh)continue;
      if(c.width!==w||c.height!==hh){c.width=w;c.height=hh}
      const vr=iw/ih,cr=w/hh;let sw,sh,sx,sy;
      if(vr>cr){sh=ih;sw=sh*cr;sx=(iw-sw)/2;sy=0}else{sw=iw;sh=sw/cr;sx=0;sy=(ih-sh)/2}
      const ctx=c.getContext('2d');ctx.imageSmoothingQuality='high';
      try{ctx.drawImage(src,sx,sy,sw,sh,0,0,w,hh);c._img=src;c._t=tt}catch(e){}
    }
  }
};
function seekTo(v,t){return new Promise(res=>{let done=false;const on=()=>fin(true);const fin=ok=>{if(done)return;done=true;v.removeEventListener('seeked',on);res(ok)};
  v.addEventListener('seeked',on);try{v.currentTime=t}catch(e){fin(false)}setTimeout(()=>fin(false),2500)})}
const FR={map:new Map(),
  async loadAll(){
    if(!DB.ok)return;
    try{for(const r of await DB.getAll('frames')){const blob=r.blob||(r.buf?new Blob([r.buf],{type:'image/jpeg'}):null);
      if(blob&&mediaMap.has(r.id))this.map.set(r.id,{fps:r.fps,n:r.n,w:r.w,h:r.h,offs:r.offs,blob})}}catch(e){console.warn(e)}
  },
  async extract(m,onProg){
    // Only ever seeks a hidden, paused video — never calls play(), so nothing can go fullscreen.
    const v=document.createElement('video');v.muted=true;v.setAttribute('muted','');v.setAttribute('playsinline','');v.setAttribute('webkit-playsinline','');v.preload='auto';
    $('#vidHolder').append(v);
    try{
      await new Promise((res,rej)=>{v.onloadedmetadata=res;v.onerror=()=>rej(new Error('This clip can’t be read'));setTimeout(()=>rej(new Error('Clip took too long to open')),20000);v.src=m.url;try{v.load()}catch(e){}});
      const dur=isFinite(v.duration)&&v.duration>0?v.duration:(m.dur||1);
      const n=Math.max(1,Math.floor(Math.min(dur,MAXS)*FPS));
      const vw=v.videoWidth||360,vh=v.videoHeight||640,sc=Math.min(1,LONG/Math.max(vw,vh));
      const w=Math.max(2,Math.round(vw*sc)),hh=Math.max(2,Math.round(vh*sc));
      const c=document.createElement('canvas');c.width=w;c.height=hh;const ctx=c.getContext('2d');
      const parts=[],offs=[0];let drawn=0;
      for(let i=0;i<n;i++){
        await seekTo(v,Math.min(Math.max(0.001,i/FPS),Math.max(0.001,dur-0.05)));
        if(v.readyState>=2){ctx.drawImage(v,0,0,w,hh);drawn++}
        if(i===5&&!drawn)throw new Error('This viewer won’t let the app read video frames');
        const b=await new Promise(r=>c.toBlob(r,'image/jpeg',.82));if(!b)throw new Error('Couldn’t save frames');
        parts.push(b);offs.push(offs[offs.length-1]+b.size);onProg&&onProg(i+1,n);
      }
      if(!drawn)throw new Error('This viewer won’t let the app read video frames');
      const rec={id:m.id,fps:FPS,n,w,h:hh,offs,blob:new Blob(parts,{type:'image/jpeg'})};
      if(DB.ok){try{await DB.put('frames',rec)}catch(e){try{await DB.put('frames',Object.assign({},rec,{blob:null,buf:await rec.blob.arrayBuffer()}))}catch(e2){}}}
      this.map.set(m.id,rec);
    }finally{v.removeAttribute('src');try{v.load()}catch(e){}v.remove()}
  }
};
const frReady=id=>{const f=FR.map.get(id);return !!f&&f.fps>=FPS};
const Conv={q:[],busy:false,done:0,total:0,err:{},
  add(ids){if(PLAY.real)return;for(const id of ids){if(frReady(id)||this.q.includes(id))continue;delete this.err[id];this.q.push(id);this.total++}this.run()},
  async run(){
    if(this.busy||!this.q.length)return;this.busy=true;
    while(this.q.length){
      const id=this.q[0],m=M(id);
      if(m&&!PLAY.real){try{await FR.extract(m,(i,n)=>this.pill(`🎞 Preparing clip ${this.done+1}/${this.total} · ${Math.round(i/n*100)}%`))}catch(e){this.err[id]=e.message||'failed';console.warn(e)}}
      this.q.shift();this.done++;
      if(cur()==='media'&&mediaTab==='clip'&&!selecting)HOOKS.media.show();
    }
    this.busy=false;this.done=0;this.total=0;this.pill('');
    const errs=Object.keys(this.err).length;if(errs)toast(`⚠ ${errs} clip${errs>1?'s':''} couldn’t be prepared — check the Media Pool`,4000);
    if(cur()==='home')HOOKS.home.show();
  },
  pill(t){let p=$('#convPill');if(!t){p&&p.remove();return}if(!p){p=h('div',{id:'convPill'});document.body.append(p)}p.textContent=t}
};
const readyClips=()=>S.media.filter(m=>m.kind==='clip'&&(PLAY.real||FR.map.has(m.id)));
function setVH(){document.documentElement.style.setProperty('--vh',window.innerHeight+'px')}
setVH();addEventListener('resize',setVH);

/* =========================================================
   Storage (IndexedDB for media + prompts, localStorage mirror)
   ========================================================= */
const DB={db:null,ok:false,
  open(){return new Promise(res=>{
    let done=false;const fin=v=>{if(!done){done=true;res(v)}};
    if(!window.indexedDB)return fin(false);
    let r;try{r=indexedDB.open('templateMakerDB',2)}catch(e){return fin(false)}
    r.onupgradeneeded=()=>{const d=r.result;
      if(!d.objectStoreNames.contains('media'))d.createObjectStore('media',{keyPath:'id'});
      if(!d.objectStoreNames.contains('kv'))d.createObjectStore('kv',{keyPath:'k'});
      if(!d.objectStoreNames.contains('frames'))d.createObjectStore('frames',{keyPath:'id'});};
    r.onsuccess=()=>{this.db=r.result;this.ok=true;fin(true)};
    r.onerror=()=>fin(false);r.onblocked=()=>fin(false);
    setTimeout(()=>fin(this.ok),5000);
  })},
  tx(store,mode,fn){return new Promise((res,rej)=>{
    let t;try{t=this.db.transaction(store,mode)}catch(e){return rej(e)}
    let out;const r=fn(t.objectStore(store));if(r)r.onsuccess=()=>{out=r.result};
    t.oncomplete=()=>res(out);t.onerror=()=>rej(t.error);t.onabort=()=>rej(t.error||new Error('abort'));
  })},
  getAll(s){return this.tx(s,'readonly',o=>o.getAll())},
  get(s,k){return this.tx(s,'readonly',o=>o.get(k))},
  put(s,v){return this.tx(s,'readwrite',o=>o.put(v))},
  del(s,k){return this.tx(s,'readwrite',o=>o.delete(k))},
  count(s){return this.tx(s,'readonly',o=>o.count())},
  clear(s){return this.tx(s,'readwrite',o=>o.clear())},
};

const DEF_PROMPTS={
  wyr:['Which fit matches this vibe?','Pick the card that fits the song','Which one would you wear here?','Choose your outfit for this moment','Which one matches the energy?'],
  whbSingle:['Who has the best fit?','Who has the cleanest shoes?','Best drip wins','Who did it better?','Which one goes the hardest?'],
  whbMulti:[['Pick your top 2 fits',2],['Choose the 3 cleanest',3],['Which 2 would you cop?',2]],
  roulette:['POV: your closet in 30 seconds','rate my rotation 1–10','fits that go hard','which one is your favorite?','the archive'],
  adPhoto:['Would you wear this? 👀','Rate this fit 1–10','Comment 🔥 if you’d cop','This or nothing.'],
  adMulti:[['Pick your favorite',1,2],['Choose your top 2',2,4],['Pick 3 you’d wear',3,6]],
};
function defaultPrompts(){
  const P={};
  for(const k of ['wyr','whbSingle','roulette','adPhoto'])P[k]=DEF_PROMPTS[k].map(t=>({id:uid(),text:t}));
  P.whbMulti=DEF_PROMPTS.whbMulti.map(([t,r])=>({id:uid(),text:t,req:r}));
  P.adMulti=DEF_PROMPTS.adMulti.map(([t,r,c])=>({id:uid(),text:t,req:r,cards:c}));
  return P;
}
const DEF_SETTINGS={
  wyrCounts:[2,3,4], whbCounts:[2,4,6], whbMultiChance:40, showNames:true, clipSound:true, cycleMs:1200,
  roulDur:30, flash:0.4, roulCap:'random', roulChosen:null,
  ads:{enabled:true,photo:true,multi:true,afterNext:true,afterNextChance:25,between:true,betweenChance:35,after:true,afterChance:70,skipDelay:3}
};
const DEF_PEOPLE=['Embreigh','Alice Rosenblum','Sydney Sweeney','Sydney Thomas','Millie Bobby Brown','Madison Beer','Ice Spice','Rubi Rose','Sadie McKenna','Kira Pregiato','Sage','Hailey','Bailey','London','Kaybre','Charli','Maci','Gwen','Aubrey','Ava','Faith','Cheyenne','Eden','Brooke','Cat Piscotta','Sommer Ray','Addison Rae','Brooke Monk','Sophie Rain','Darla','Lilly Phillips','Faith Ordway','Chloe Parker','Mia','Sky Bri','Riley Reid','Corinna Kopf','Mkayla','Demitra Kalogras','Ari Kytsya','Megan Fox','Leah Halton','Lana Rhoades','Doja Cat','Ehcico','Persephone Pink','Piper Rockelle','Zoe','Jolie','Sophie','Breckie Hill','Hannah'];
const defaultPeople=()=>DEF_PEOPLE.map(name=>({id:uid(),name}));
const S={prompts:defaultPrompts(),settings:JSON.parse(JSON.stringify(DEF_SETTINGS)),media:[],people:defaultPeople(),tags:{}};
const mediaMap=new Map();
const M=id=>mediaMap.get(id);
const url=id=>{const m=M(id);return m?m.url:''};
const count=kind=>S.media.reduce((n,m)=>n+(m.kind===kind),0);
const promptCount=()=>Object.values(S.prompts).reduce((n,a)=>n+a.length,0);

function mergeState(st){
  if(!st)return;
  if(Array.isArray(st.people))S.people=st.people;
  if(st.tags&&typeof st.tags==='object')S.tags=st.tags;
  if(st.prompts){for(const k in S.prompts)if(Array.isArray(st.prompts[k]))S.prompts[k]=st.prompts[k]}
  if(st.settings){const ads=Object.assign({},DEF_SETTINGS.ads,st.settings.ads||{});
    S.settings=Object.assign(JSON.parse(JSON.stringify(DEF_SETTINGS)),st.settings,{ads})}
}
function addMediaToMemory(rec,blob){
  const m={id:rec.id,kind:rec.kind,name:rec.name,type:rec.type,dur:rec.dur||0,thumb:rec.thumb||null,added:rec.added||Date.now(),blob,url:URL.createObjectURL(blob)};
  S.media.push(m);mediaMap.set(m.id,m);return m;
}
async function loadAll(){
  let st=null,ls=null;
  if(DB.ok){try{const r=await DB.get('kv','state');if(r)st=r.v}catch(e){}}
  try{ls=JSON.parse(localStorage.getItem('tm_state')||'null')}catch(e){}
  if(ls&&(!st||(ls.savedAt||0)>(st.savedAt||0)))st=ls;
  mergeState(st);
  if(DB.ok){
    try{const recs=await DB.getAll('media');recs.sort((a,b)=>(a.added||0)-(b.added||0));
      for(const r of recs){const blob=r.blob||(r.buf?new Blob([r.buf],{type:r.type||''}):null);if(blob)addMediaToMemory(r,blob)}
    }catch(e){console.warn(e)}
  }
}
async function storeMedia(rec,blob){
  if(!DB.ok)return false;
  try{await DB.put('media',Object.assign({},rec,{blob}));return true}
  catch(e){try{const buf=await blob.arrayBuffer();await DB.put('media',Object.assign({},rec,{buf}));return true}catch(e2){console.warn(e2);return false}}
}
let saveTimer=null;
function stateObj(){return {prompts:S.prompts,settings:S.settings,people:S.people,tags:S.tags,savedAt:Date.now()}}
function markDirty(){$('#saveBtn').classList.add('dirty');clearTimeout(saveTimer);saveTimer=setTimeout(()=>saveNow(false),250)}
async function saveNow(manual){
  clearTimeout(saveTimer);
  const st=stateObj();let ok=false,lsOk=false;
  try{localStorage.setItem('tm_state',JSON.stringify(st));lsOk=true}catch(e){}
  if(DB.ok){try{await DB.put('kv',{k:'state',v:st});ok=true}catch(e){}}
  $('#saveBtn').classList.remove('dirty');
  if(!manual)return ok||lsOk;
  try{navigator.storage&&navigator.storage.persist&&navigator.storage.persist()}catch(e){}
  const b=$('#saveBtn');
  if(ok){
    let n=S.media.length;try{n=await DB.count('media')}catch(e){}
    b.classList.add('done');b.textContent='✓';setTimeout(()=>{b.classList.remove('done');b.textContent='Save'},1600);
    toast(`✓ Saved — ${count('photo')} photos · ${count('clip')} clips · ${promptCount()} prompts (${n} media files on device)`,3200);
  }else if(lsOk){
    toast('⚠ Prompts saved, but this viewer blocks media storage. Make a backup in Settings.',4200);
  }else toast('⚠ This viewer is blocking storage. Use Settings → Backup.',4200);
  return ok;
}
document.addEventListener('visibilitychange',()=>{if(document.hidden)saveNow(false)});
addEventListener('pagehide',()=>saveNow(false));

/* =========================================================
   Shuffle bags — every item is used before any repeats
   ========================================================= */
class Bag{
  constructor(getIds){this.getIds=getIds;this.q=[];this.last=null}
  next(avoid=[]){
    const ids=this.getIds();if(!ids.length)return null;
    const set=new Set(ids);this.q=this.q.filter(i=>set.has(i));
    for(let t=0;t<3;t++){
      const ix=this.q.findIndex(i=>!avoid.includes(i)&&(i!==this.last||ids.length===1));
      if(ix>=0){const id=this.q.splice(ix,1)[0];this.last=id;return id}
      this.q.push(...shuffle(ids));
    }
    return pick(ids);
  }
}
const bags={
  photo:new Bag(()=>S.media.filter(m=>m.kind==='photo').map(m=>m.id)),
  clip:new Bag(()=>readyClips().map(m=>m.id)),
};
const pBags={};
function promptFrom(cat,filter){
  const list=S.prompts[cat].filter(p=>p.text&&p.text.trim()&&(!filter||filter(p)));
  if(!list.length)return null;
  const key=cat+(filter?'*':'');
  if(!pBags[key])pBags[key]=new Bag(()=>[]);
  pBags[key].getIds=()=>S.prompts[cat].filter(p=>p.text&&p.text.trim()&&(!filter||filter(p))).map(p=>p.id);
  const id=pBags[key].next();return S.prompts[cat].find(p=>p.id===id)||pick(list);
}
function personOf(id){const pid=S.tags[id];return pid?S.people.find(p=>p.id===pid)||null:null}
// Each person counts once no matter how many photos they have; untagged photos count on their own.
function entities(){const out=[],seen=new Set();for(const m of S.media){if(m.kind!=='photo')continue;const p=personOf(m.id);if(p){if(!seen.has(p.id)){seen.add(p.id);out.push('p:'+p.id)}}else out.push('m:'+m.id)}return out}
const entBag=new Bag(entities),personBags={};
const namedBag=new Bag(()=>S.people.filter(p=>photosOf(p.id).length).map(p=>p.id));
function photoFromEnt(e,avoid){if(e.startsWith('m:'))return e.slice(2);const pid=e.slice(2);
  if(!personBags[pid])personBags[pid]=new Bag(()=>S.media.filter(m=>m.kind==='photo'&&S.tags[m.id]===pid).map(m=>m.id));
  return personBags[pid].next(avoid)}
function takePhotos(n){
  const ents=[],out=[];
  for(let i=0;i<n;i++){const e=entBag.next(ents);if(!e||ents.includes(e))break;ents.push(e);const id=photoFromEnt(e,out);if(id&&!out.includes(id))out.push(id)}
  while(out.length<n){const id=bags.photo.next(out);if(!id||out.includes(id))break;out.push(id)}
  return shuffle(out);
}
const takePhoto=()=>takePhotos(1)[0]||null;
function nameTag(id){const p=S.settings.showNames?personOf(id):null;return p?h('div',{class:'nm'},p.name):null}
const pickText=t=>(t||'').replace(/\{name\}/gi,'___');
function withNames(prompt,ids){
  const names=[...new Set(ids.map(personOf).filter(Boolean).map(p=>p.name))];
  prompt=prompt||'';if(!names.length)return pickText(prompt);
  const n=names.length>1?names.slice(0,-1).join(', ')+' & '+names[names.length-1]:names[0];
  if(/\{name\}/i.test(prompt))return prompt.replace(/\{name\}/gi,n);
  return prompt?prompt+' → '+n:n;
}
const CAPSTYLES=['outline','white','black','outline','yellow','accent'];
function joinNames(a){return a.length>1?a.slice(0,-1).join(', ')+' & '+a[a.length-1]:(a[0]||'')}
// Picked person's name goes in front of the prompt (or wherever {name} is).
function whoParts(prompt,ids){
  const n=joinNames([...new Set(ids.map(personOf).filter(Boolean).map(p=>p.name))]);prompt=prompt||'';
  if(/\{name\}/i.test(prompt))return [h('span',null,prompt.replace(/\{name\}/gi,n||'___'))];
  const out=[];if(n)out.push(h('span',{class:'who'},n),' ');if(prompt)out.push(h('span',null,prompt));return out;
}
function capWho(prompt,ids,style){return h('div',{class:'cap k-'+(style||'outline')},whoParts(prompt,ids))}
// On the result, show a different (random) photo of the picked person — one photo, it stays.
function personCycler(id){
  const p=personOf(id);let show=id;
  if(p&&photosOf(p.id).length>1){show=photoFromEnt('p:'+p.id,[id])||id;if(show===id){const o=photosOf(p.id).map(m=>m.id).filter(x=>x!==id);if(o.length)show=pick(o)}}
  return h('img',{class:'m',src:url(show),alt:''});
}
function capEl(text,style,extraCls=''){return h('div',{class:'cap k-'+(style||'outline')+' '+extraCls},h('span',null,text||''))}

/* =========================================================
   Stage sizing (always 9:16)
   ========================================================= */
function fitStage(wrap){
  const st=wrap.querySelector('.stage');if(!st)return;
  const cs=getComputedStyle(wrap);
  const W=wrap.clientWidth-parseFloat(cs.paddingLeft)-parseFloat(cs.paddingRight);
  const H=wrap.clientHeight-parseFloat(cs.paddingTop)-parseFloat(cs.paddingBottom);
  if(W<=0||H<=0)return;
  const w=W,hh=H; // games fill all available space
  st.style.width=Math.floor(w)+'px';st.style.height=Math.floor(hh)+'px';st.style.setProperty('--sw',Math.floor(w)+'px');
}
const ro=window.ResizeObserver?new ResizeObserver(es=>es.forEach(e=>fitStage(e.target))):null;
$$('.stageWrap').forEach(w=>{ro&&ro.observe(w)});
addEventListener('resize',()=>$$('.stageWrap').forEach(fitStage));
function emptyMsg(text,showBtn=true){return h('div',{class:'empty'},h('div',{style:'font-size:40px'},'🖼'),h('div',null,text),showBtn?btn('Open Media Pool',()=>go('media'),'primary'):null)}

/* =========================================================
   Navigation
   ========================================================= */
const TITLES={people:'People',person:'Person',home:'Template Maker',wyr:'Would You Rather',whb:'Who Has The Best',roul:'Roulette',player:'Roulette',media:'Media Pool',prompts:'Prompts',ads:'Ad Options',settings:'Settings & Backup'};
const HOOKS={};
const navStack=['home'];
function cur(){return navStack[navStack.length-1]}
function showScreen(name){
  document.body.classList.toggle('gamemode',['wyr','whb','player'].includes(name));
  $$('.screen').forEach(s=>s.classList.toggle('on',s.id==='s-'+name));
  $('#title').textContent=TITLES[name]||'';
  $('#backBtn').style.visibility=name==='home'?'hidden':'visible';
  const w=$('#s-'+name+' .stageWrap');if(w)requestAnimationFrame(()=>fitStage(w));
  HOOKS[name]&&HOOKS[name].show&&HOOKS[name].show();
}
function go(name){
  const c=cur();if(c===name){HOOKS[name]&&HOOKS[name].show&&HOOKS[name].show();return}
  HOOKS[c]&&HOOKS[c].hide&&HOOKS[c].hide();
  const ix=navStack.indexOf(name);if(ix>=0)navStack.length=ix+1;else navStack.push(name);
  showScreen(name);
}
function back(){if(navStack.length<2)return;const c=navStack.pop();HOOKS[c]&&HOOKS[c].hide&&HOOKS[c].hide();showScreen(cur())}
$('#backBtn').onclick=back;
$('#saveBtn').onclick=()=>saveNow(true);

/* =========================================================
   Shared game video (one element, so sound stays unlocked)
   ========================================================= */
const GV=new Flip();
let gvMuted=false;
function setGV(id){if(GV._mid!==id){GV.setClip(id);GV.currentTime=0}}
function soundChip(container,onTap){
  if(container.querySelector('.chip-snd'))return;
  const c=h('button',{class:'chip-snd',onclick:e=>{e.stopPropagation();c.remove();onTap()}},'🔇 Tap for sound');
  container.append(c);
}
function playGV(){GV._wantSound=true;GV.muted=gvMuted;GV.play()}

/* =========================================================
   Game 1 — Would You Rather
   ========================================================= */
const WYR={st:null,
  newRound(){
    const stage=$('#wyrStage');const nP=count('photo'),nC=readyClips().length;
    if(!nC||nP<2){this.st=null;GV.pause();stage.replaceChildren(emptyMsg(count('clip')&&!nC?'Your clips are still being prepared — give it a moment, then tap 🔀.':'Add at least 1 clip and 2 photos to play Would You Rather.'));this.ctl();return}
    let counts=S.settings.wyrCounts.filter(c=>c<=nP);if(!counts.length)counts=[Math.min(nP,2)];
    const n=pick(counts);const pr=promptFrom('wyr');
    this.st={clip:bags.clip.next(),cards:takePhotos(n),prompt:pr?pr.text:'',sel:-1,phase:'pick',cap:pick(CAPSTYLES)};
    this.render();this.ctl();
  },
  render(){
    const s=this.st,stage=$('#wyrStage');if(!s)return;setGV(s.clip);$('#s-wyr').scrollTop=0;
    if(s.phase==='pick'){
      const topH={2:55,3:52,4:46}[s.cards.length]||52;
      this.capEl=capWho(s.prompt,s.sel>=0?[s.cards[s.sel]]:[],s.cap);
      const top=h('div',{class:'wyr-top',style:`height:${topH}%`},VR.canvas(GV),this.capEl);
      const cards=h('div',{class:'cards c'+s.cards.length,style:`top:${topH}%`});
      s.cards.forEach((id,i)=>cards.append(h('div',{class:'card'+(i===s.sel?' sel':''),onclick:()=>{
        s.sel=i;$$('.card',cards).forEach((c,j)=>c.classList.toggle('sel',j===i));
        const nc=capWho(s.prompt,[id],s.cap);this.capEl.replaceWith(nc);this.capEl=nc;this.ctl()}},
        h('img',{src:url(id),class:'m',draggable:'false',alt:''}),nameTag(id))));
      stage.replaceChildren(top,cards);
    }else{
      stage.replaceChildren(h('div',{class:'res2'},
        h('div',{class:'cell'},VR.canvas(GV)),h('div',{class:'cell'},personCycler(s.cards[s.sel]))),
        capWho(s.prompt,[s.cards[s.sel]],s.cap));
    }
    playGV();
  },
  ctl(){
    const c=$('#wyrCtl'),s=this.st;
    if(!s){c.replaceChildren(btn('🖼 Media Pool',()=>go('media'),'primary'));return}
    if(s.phase==='pick')c.replaceChildren(btn('✎',()=>openPrompts('wyr'),'sq'),
      h('div',{class:'ctl-info'},h('b',null,s.cards.length+' cards'),s.sel<0?'pick one':'selected'),
      btn('Next ▶',()=>this.confirm(),'primary'+(s.sel<0?' disabled':'')),btn('🔀',()=>this.newRound(),'sq'));
    else c.replaceChildren(btn('✎',()=>openPrompts('wyr'),'sq'),btn('↺',()=>{s.phase='pick';s.sel=-1;this.render();this.ctl()},'sq'),
      btn('Next round ▶',()=>this.next(),'primary'));
  },
  confirm(){if(!this.st||this.st.sel<0||Ads.open)return;Ads.afterNext().then(()=>{this.st.phase='result';this.render();this.ctl()})},
  next(){if(Ads.open)return;GV.pause();Ads.between().then(()=>this.newRound())},
};
HOOKS.wyr={show(){if(!WYR.st)WYR.newRound();else{WYR.render();WYR.ctl()}Ads.start()},hide(){GV.pause();Ads.stop()}};

/* =========================================================
   Game 3 — Who Has The Best
   ========================================================= */
const WHB={st:null,
  newRound(){
    const stage=$('#whbStage');const nP=count('photo');
    if(nP<2){this.st=null;stage.replaceChildren(emptyMsg('Add at least 2 photos to play Who Has The Best.'));this.ctl();return}
    const enabled=(S.settings.whbCounts.length?S.settings.whbCounts:[2,4,6]).slice().sort((a,b)=>a-b);
    let multi=null;
    if(Math.random()*100<S.settings.whbMultiChance){
      multi=promptFrom('whbMulti',p=>{const r=p.req||2;return [2,4,6].some(c=>c>r&&c<=nP)});
    }
    let n,req=1,prompt;
    if(multi){
      req=clamp(multi.req||2,2,5);
      let opts=enabled.filter(c=>c>req&&c<=nP);if(!opts.length)opts=[2,4,6].filter(c=>c>req&&c<=nP);
      n=pick(opts);prompt=multi.text;
    }else{
      let opts=enabled.filter(c=>c<=nP);if(!opts.length)opts=[Math.min(nP,2)];
      n=pick(opts);const p=promptFrom('whbSingle');prompt=p?p.text:'';
    }
    this.st={cards:takePhotos(n),req,prompt,sel:[],phase:'pick',cap:pick(CAPSTYLES)};
    this.render();this.ctl();
  },
  render(){
    const s=this.st,stage=$('#whbStage');if(!s)return;$('#s-whb').scrollTop=0;
    this.capEl=capWho(s.prompt,s.sel.map(i=>s.cards[i]),s.cap);
    const head=h('div',{class:'whb-head'},this.capEl);
    if(s.phase==='pick'){
      const g=h('div',{class:'whb-grid n'+s.cards.length});
      s.cards.forEach((id,i)=>g.append(h('div',{class:'card'+(s.sel.includes(i)?' sel':''),onclick:e=>this.tap(i,e.currentTarget)},
        h('img',{src:url(id),class:'m',draggable:'false',alt:''}),nameTag(id))));
      stage.replaceChildren(head,g);
    }else{
      const chosen=s.sel.slice().sort((a,b)=>a-b);
      const g=h('div',{class:'whb-grid n'+chosen.length});
      chosen.forEach(i=>g.append(h('div',{class:'card'},personCycler(s.cards[i]))));
      stage.replaceChildren(head,g);
    }
  },
  tap(i,elc){
    const s=this.st;const k=s.sel.indexOf(i);
    if(k>=0)s.sel.splice(k,1);
    else if(s.req===1){s.sel=[i]}
    else if(s.sel.length<s.req)s.sel.push(i);
    else{elc.animate&&elc.animate([{transform:'translateX(-4px)'},{transform:'translateX(4px)'},{transform:'none'}],{duration:180});toast(`Only ${s.req} — tap one to unselect`,1200);return}
    $$('#whbStage .card').forEach((c,j)=>c.classList.toggle('sel',s.sel.includes(j)));
    const nc=capWho(s.prompt,s.sel.map(j=>s.cards[j]),s.cap);this.capEl.replaceWith(nc);this.capEl=nc;
    this.ctl();
  },
  ctl(){
    const c=$('#whbCtl'),s=this.st;
    if(!s){c.replaceChildren(btn('🖼 Media Pool',()=>go('media'),'primary'));return}
    if(s.phase==='pick'){
      const ready=s.sel.length===s.req;
      c.replaceChildren(btn('✎',()=>openPrompts('whb'),'sq'),
        h('div',{class:'ctl-info'},h('b',null,s.req>1?`Pick ${s.req}`:'Pick 1'),`${s.sel.length}/${s.req} selected`),
        btn('Next ▶',()=>this.confirm(),'primary'+(ready?'':' disabled')),btn('🔀',()=>this.newRound(),'sq'));
    }else c.replaceChildren(btn('✎',()=>openPrompts('whb'),'sq'),btn('↺',()=>{s.phase='pick';s.sel=[];this.render();this.ctl()},'sq'),btn('Next round ▶',()=>{if(!Ads.open)Ads.between().then(()=>this.newRound())},'primary'));
  },
  confirm(){const s=this.st;if(!s||s.sel.length!==s.req||Ads.open)return;Ads.afterNext().then(()=>{s.phase='result';this.render();this.ctl()})},
};
HOOKS.whb={show(){if(!WHB.st)WHB.newRound();else{WHB.render();WHB.ctl()}Ads.start()},hide(){Ads.stop()}};

/* =========================================================
   Ads
   ========================================================= */
const CTAS=['Learn more','Shop now','Tap to see','View','Get it','See more'];
const Ads={open:false,
  start(){},stop(){},
  types(){
    const a=S.settings.ads,nP=count('photo'),t=[];
    if(a.photo&&nP>=1&&S.prompts.adPhoto.some(p=>p.text.trim()))t.push('photo');
    if(a.multi&&nP>=2&&S.prompts.adMulti.some(p=>p.text.trim()))t.push('multi');
    return t;
  },
  roll(on,chance){const a=S.settings.ads;if(a.enabled&&on&&Math.random()*100<chance)return this.show();return Promise.resolve()},
  afterNext(){const a=S.settings.ads;return this.roll(a.afterNext,a.afterNextChance)},
  between(){const a=S.settings.ads;return this.roll(a.between,a.betweenChance)},
  after(){const a=S.settings.ads;return this.roll(a.after,a.afterChance)},
  show(force){
    if(this.open)return Promise.resolve();
    if(force==='photo'&&!(count('photo')&&S.prompts.adPhoto.some(p=>p.text.trim()))){toast('Need 1+ photo and a photo-ad prompt');return Promise.resolve()}
    if(force==='multi'&&!(count('photo')>=2&&S.prompts.adMulti.some(p=>p.text.trim()))){toast('Need 2+ photos and a card-ad prompt');return Promise.resolve()}
    const types=force?[force]:this.types();
    if(!types.length)return Promise.resolve();
    this.open=true;
    const scr=$('.screen.on');scr&&scr.classList.add('adon');
    const paused=[...Flips].filter(f=>!f.paused);paused.forEach(f=>f.pause());
    const mount=$('.screen.on .stage')||document.body;
    return new Promise(res=>{
      const type=pick(types);
      const back=h('div',{class:'ad-back v-full'});
      const box=h('div',{class:'ad'});
      let closed=false;
      const done=()=>{if(closed)return;closed=true;back.remove();this.open=false;scr&&scr.classList.remove('adon');
        paused.forEach(f=>f.play());res()};
      const x=h('button',{class:'ad-x wait',onclick:e=>{e.stopPropagation();done()}});
      let left=Math.round(S.settings.ads.skipDelay);
      const tickX=()=>{if(left<=0){x.textContent='✕';x.classList.remove('wait')}else{x.textContent=left;left--;setTimeout(tickX,1000)}};
      tickX();
      const head=h('div',{class:'ad-head'},h('span',{class:'ad-badge'},'Ad'),h('span',null,'Sponsored'),x);
      if(type==='photo'){
        const p=promptFrom('adPhoto');const id=takePhoto();
        const imgBox=h('div',{class:'ad-img'},h('img',{src:url(id),class:'m',alt:''}));
        box.append(head,imgBox,h('div',{class:'ad-txt'},whoParts(p?p.text:'',[id])),h('button',{class:'ad-cta',onclick:done},pick(CTAS)+' ›'));
      }else{
        const p=promptFrom('adMulti');const nP=count('photo');
        let n=[2,3,4,6].includes(p.cards)?p.cards:4;n=Math.min(n,nP);if(n<2)n=2;
        const req=clamp(p.req||1,1,n-1);
        const ids=takePhotos(n);const sel=[];
        const grid=h('div',{class:'ad-cards n'+ids.length});
        const sub=h('div',{class:'ad-sub'});
        const go2=h('button',{class:'ad-cta'});
        const upd=()=>{sub.textContent=req>1?`Select ${req} · ${sel.length}/${req}`:'Tap one';go2.textContent=sel.length===req?'Submit ›':(req>1?`Pick ${req-sel.length} more`:'Pick one');go2.style.opacity=sel.length===req?1:.45;$$('.card',grid).forEach((c,j)=>c.classList.toggle('sel',sel.includes(j)));txt.replaceChildren(...whoParts(p.text,sel.map(i=>ids[i])))};
        ids.forEach((id,i)=>grid.append(h('div',{class:'card',onclick:()=>{const k=sel.indexOf(i);if(k>=0)sel.splice(k,1);else if(req===1){sel.length=0;sel.push(i)}else if(sel.length<req)sel.push(i);upd()}},h('img',{src:url(id),class:'m',alt:''}),nameTag(id))));
        const txt=h('div',{class:'ad-txt mc'},whoParts(p.text,[]));
        go2.onclick=()=>{
          if(sel.length!==req)return;
          const chosen=sel.slice().sort((a,b)=>a-b).map(i=>ids[i]);
          const res2=h('div',{class:'ad-cards n'+Math.min(chosen.length,6)});
          chosen.forEach(id=>res2.append(h('div',{class:'card sel'},personCycler(id))));
          grid.replaceWith(res2);txt.replaceChildren(...whoParts(p.text,chosen));sub.textContent='✓ Your pick';go2.textContent='Continue ›';go2.style.opacity=1;go2.onclick=done;
        };
        box.classList.add('mcbox');box.append(head,txt,grid,sub,go2);upd();
      }
      back.append(box);mount.append(back);
    });
  },
};

/* =========================================================
   Game 2 — Roulette montage generator
   ========================================================= */
function genMontage(){
  const D=S.settings.roulDur,F=S.settings.flash;
  const nP=count('photo'),nC=readyClips().length;
  if(!nP&&!nC)return null;
  const P=()=>takePhoto(),C=()=>bags.clip.next();
  const startFor=(id,L)=>{const d=(M(id)&&M(id).dur)||0;return d>L+0.3?rand(0,d-L-0.1):0};
  const img=(id,x)=>Object.assign({t:'img',id},x||{});
  const vid=(id,start,audio)=>({t:'vid',id,start,audio:!!audio});
  const zoom=()=>({s:rand(1.8,3.2),x:rand(10,90),y:rand(10,90)});
  const gw=()=>Math.random()<.3;
  const named=S.people.filter(p=>photosOf(p.id).length);
  const mPrompt=()=>{const cats=['roulette','whbSingle','wyr'].filter(c=>S.prompts[c].some(p=>p.text&&p.text.trim()));const p=cats.length?promptFrom(pick(cats)):null;return p?p.text:''};
  const nameCap=id=>({ids:[id],text:mPrompt(),style:pick(CAPSTYLES),key:uid()});
  const styles=[
    {n:'spotlight',w:named.length?3:0,c:0,p:1,make(){const pid=namedBag.next();const ph=shuffle(photosOf(pid).map(m=>m.id));if(!ph.length)return[{dur:F,layout:'full',items:[img(P())]}];
      const k=clamp(ph.length,2,4),cap=nameCap(ph[0]),out=[];for(let i=0;i<k;i++)out.push({dur:rand(.55,.9),layout:'full',items:[img(ph[i%ph.length],Math.random()<.3?{zoom:{s:rand(1.3,1.8),x:rand(30,70),y:rand(25,60)}}:null)],cap});return out}},
    {n:'clipHit',w:2.2,c:1,p:0,make(){const id=C(),L=rand(1.2,2.6);return[{dur:L,layout:'full',items:[vid(id,startFor(id,L),1)]}]}},
    {n:'rapidClips',w:2,c:1,p:0,make(){const n=randInt(3,6),out=[];for(let i=0;i<n;i++){const id=C(),L=rand(.45,.9);out.push({dur:L,layout:'full',items:[vid(id,startFor(id,L),1)]})}return out}},
    {n:'flashBurst',w:2.2,c:0,p:1,make(){const n=randInt(5,10),out=[];for(let i=0;i<n;i++)out.push({dur:F,layout:'full',items:[img(P())]});return out}},
    {n:'closeUps',w:1.6,c:0,p:1,make(){const id=P(),out=[{dur:F,layout:'full',items:[img(id)]}];const k=randInt(3,5);for(let i=0;i<k;i++)out.push({dur:F,layout:'full',items:[img(id,{zoom:zoom()})]});if(Math.random()<.5)out.push({dur:F*1.5,layout:'full',items:[img(id)]});return out}},
    {n:'kenBurns',w:1,c:0,p:1,make(){const id=P();return[{dur:rand(1.4,2.4),layout:'full',items:[img(id,{kb:{a:1,b:rand(1.25,1.55),x:rand(20,80),y:rand(20,80)}})]}]}},
    {n:'grid3PVP',w:2.2,c:1,p:1,make(){const c=C(),L=rand(2.2,3.6),m=pick([1,2,3,4]),s0=startFor(c,L),g=gw(),out=[];
      for(let k=0;k<m;k++)out.push({dur:L/m,layout:'stack3',gw:g,items:[img(P()),vid(c,s0+k*L/m,1),img(P())]});return out}},
    {n:'grid3VPV',w:1.6,c:1,p:1,make(){const c1=C(),c2=nC>1?bags.clip.next([c1]):c1,L=rand(2,3.2),m=randInt(1,4),s1=startFor(c1,L),s2=startFor(c2,L)+(c1===c2?1:0),g=gw(),out=[];
      for(let k=0;k<m;k++){const o=k*L/m;out.push({dur:L/m,layout:'stack3',gw:g,items:[vid(c1,s1+o,1),img(P()),vid(c2,s2+o,0)]})}return out}},
    {n:'split2',w:1.5,c:1,p:0,make(){const c=C(),L=rand(1.8,3),g=gw(),out=[];
      if(nP&&Math.random()<.55){const m=randInt(2,5),s0=startFor(c,L),top=Math.random()<.5;
        for(let k=0;k<m;k++){const v=vid(c,s0+k*L/m,1),p=img(P());out.push({dur:L/m,layout:'stack2',gw:g,items:top?[v,p]:[p,v]})}}
      else{const c2=nC>1?bags.clip.next([c]):c;out.push({dur:L,layout:'stack2',gw:g,items:[vid(c,startFor(c,L),1),vid(c2,startFor(c2,L)+(c===c2?1.2:0),0)]})}
      return out}},
    {n:'grid4',w:1.2,c:0,p:1,make(){const m=randInt(2,4),g=gw(),withV=nC&&Math.random()<.4,cell=randInt(0,3),out=[];let c,s0,L=0;
      const durs=[];for(let k=0;k<m;k++)durs.push(rand(F,F*2.5));L=durs.reduce((a,b)=>a+b,0);if(withV){c=C();s0=startFor(c,L)}
      let o=0;for(let k=0;k<m;k++){const items=[img(P()),img(P()),img(P()),img(P())];if(withV)items[cell]=vid(c,s0+o,1);out.push({dur:durs[k],layout:'grid4',gw:g,items});o+=durs[k]}return out}},
    {n:'side2',w:1,c:0,p:1,make(){const m=randInt(2,4),g=gw(),out=[];for(let k=0;k<m;k++)out.push({dur:rand(F,F*2),layout:'side2',gw:g,items:[img(P()),img(P())]});return out}},
    {n:'grid9',w:.8,c:0,p:1,make(){const items=[];for(let i=0;i<9;i++)items.push(img(P()));const out=[{dur:rand(.7,1.2),layout:'grid9',gw:gw(),items}];
      if(Math.random()<.6)out.push({dur:F*2,layout:'full',items:[img(pick(items).id,{zoom:Math.random()<.5?zoom():null})]});return out}},
    {n:'combo',w:1.6,c:1,p:1,make(){const pairs=randInt(2,4),out=[];for(let i=0;i<pairs;i++){const id=C(),L=rand(.5,.85);out.push({dur:L,layout:'full',items:[vid(id,startFor(id,L),1)]});
      const k=randInt(2,3);for(let j=0;j<k;j++)out.push({dur:F,layout:'full',items:[img(P())]})}return out}},
    {n:'pip',w:1,c:1,p:1,make(){const c=C(),L=rand(2,3.2),m=randInt(1,3),s0=startFor(c,L),out=[];for(let k=0;k<m;k++)out.push({dur:L/m,layout:'pip',items:[vid(c,s0+k*L/m,1),img(P())]});return out}},
  ];
  const avail=styles.filter(s=>s.w>0&&(!s.c||nC>=s.c)&&(!s.p||nP>=s.p));
  if(!avail.length)return null;
  const shots=[];let total=0;const recent=[];
  let guard=0;
  while(total<D&&guard++<400){
    let pool=avail.filter(s=>!recent.includes(s.n));if(!pool.length)pool=avail;
    const sum=pool.reduce((a,s)=>a+s.w,0);let r=Math.random()*sum,st=pool[0];
    for(const s of pool){r-=s.w;if(r<=0){st=s;break}}
    recent.push(st.n);if(recent.length>Math.min(2,avail.length-1))recent.shift();
    const grp=st.make();
    // sometimes put the person's name + one of your prompts on close-ups / slow zooms
    if((st.n==='closeUps'||st.n==='kenBurns')&&Math.random()<.6){const id=grp[0].items[0].id;if(personOf(id)){const cap=nameCap(id);grp.forEach(g=>g.cap=cap)}}
    for(const sh of grp){sh.style=st.n;shots.push(sh);total+=sh.dur;if(total>=D)break}
  }
  // trim to exact length
  let at=0;for(const s of shots){s.at=at;at+=s.dur}
  while(shots.length>1&&shots[shots.length-1].at>=D-0.15)shots.pop();
  const last=shots[shots.length-1];last.dur=D-last.at;
  // caption
  let caption='';const set=S.settings;
  if(set.roulCap==='random'){const p=promptFrom('roulette');caption=p?p.text:''}
  else if(set.roulCap==='chosen'){const p=S.prompts.roulette.find(p=>p.id===set.roulChosen);caption=p?p.text:''}
  return {shots,total:D,caption,cap:pick(CAPSTYLES)};
}

const Player={shots:[],total:30,t:0,playing:false,cur:-1,raf:0,last:0,pool:[],inUse:new Set(),forceMute:false,layEl:null,kb:null,imgCache:[],capEl:null,endEl:null,ui:{},
  init(){
    for(let i=0;i<6;i++)this.pool.push(new Flip());
    this.ui={cur:$('#pCur'),tot:$('#pTot'),seek:$('#pSeek'),play:$('#pPlay')};
    $('#pPlay').onclick=()=>this.playing?this.pause():this.play();
    $('#pBack').onclick=()=>this.seek(this.t-5);
    $('#pFwd').onclick=()=>this.seek(this.t+5);
    $('#pFs').onclick=()=>this.toggleFs();
    $('#pNew').onclick=()=>this.newMontage();
    const sk=this.ui.seek;let wasPlaying=false;
    sk.addEventListener('pointerdown',()=>{wasPlaying=this.playing;if(wasPlaying)this.pause()});
    sk.addEventListener('input',()=>this.seek(parseFloat(sk.value),true));
    sk.addEventListener('change',()=>{if(wasPlaying){wasPlaying=false;this.play()}});
    $('#pStage').addEventListener('click',e=>{if(e.target.closest('button,.ad-back'))return;
      const s=$('#s-player');if(s.classList.contains('fs'))s.classList.toggle('hidectl');else this.playing?this.pause():this.play()});
  },
  newMontage(){
    const m=genMontage();
    if(!m){toast('Add photos or clips to the Media Pool first');return false}
    this.load(m);this.unlock();this.play();return true;
  },
  load(m){
    this.pause();this.shots=m.shots;this.total=m.total;this.t=0;this.cur=-1;this.kb=null;
    this.pool.forEach(v=>{v._prep=-1});
    const ids=[...new Set(m.shots.flatMap(s=>s.items.filter(i=>i.t==='img').map(i=>i.id)))];
    this.imgCache=ids.map(id=>{const im=new Image();im.src=url(id);im.decode&&im.decode().catch(()=>{});return im});
    const st=$('#pStage');
    this.layEl=h('div',{style:'position:absolute;inset:0'});
    this.capEl=m.caption?capEl(m.caption,m.cap):null;this.capKey=null;this.shotCap=null;
    this.endEl=null;
    st.replaceChildren(this.layEl,...(this.capEl?[this.capEl]:[]));
    this.ui.seek.max=this.total;this.ui.tot.textContent=fmtT(this.total);this.updUI();
  },
  unlock(){this.pool.forEach(v=>v.unlock());GV.unlock()}, // runs inside a tap so sound is allowed
  wrapT(v,t){const d=v.duration;return isFinite(d)&&d>0?t%d:t},
  loadV(v,id,want){
    v._want=want;
    if(v._mid!==id){v.setClip(id);v.currentTime=want}
    else{const w=this.wrapT(v,want);if(Math.abs(v.currentTime-w)>0.3)v.currentTime=w}
  },
  playV(v){v.play()},
  getVid(i,id,want,taken){
    const free=this.pool.filter(v=>!taken.has(v));
    let v=free.find(v=>this.inUse.has(v)&&v._mid===id&&v.readyState>=1&&Math.abs(v.currentTime-this.wrapT(v,want))<0.4);
    if(!v)v=free.find(v=>v._prep===i&&v._mid===id);
    if(!v)v=free.find(v=>!this.inUse.has(v)&&v._prep!==i);
    if(!v)v=free[0];
    return v;
  },
  applyImg(im,it){
    im.style.transform='';im.style.transformOrigin='';
    if(it.zoom){im.style.transformOrigin=it.zoom.x+'% '+it.zoom.y+'%';im.style.transform='scale('+it.zoom.s+')'}
    if(it.kb){im.style.transformOrigin=it.kb.x+'% '+it.kb.y+'%';this.kb={el:im,kb:it.kb}}
  },
  samePatch(a,b){
    if(!a||!b||a.layout!==b.layout||a.items.length!==b.items.length)return false;
    for(let k=0;k<a.items.length;k++){const x=a.items[k],y=b.items[k];if(x.t!==y.t)return false;
      if(x.t==='vid'&&(x.id!==y.id||Math.abs((x.start+a.dur)-y.start)>0.05))return false}
    return true;
  },
  enter(i,off){
    {const sc=this.shots[i].cap,ck=sc?sc.key:'';
      if(ck!==this.capKey){this.capKey=ck;if(this.shotCap){this.shotCap.remove();this.shotCap=null}
        if(sc){this.shotCap=capWho(sc.text,sc.ids,sc.style);$('#pStage').append(this.shotCap)}
        if(this.capEl)this.capEl.style.visibility=sc?'hidden':''}}
    const shot=this.shots[i],prev=this.shots[this.cur];
    this.kb=null;
    if(this.cur===i-1&&this.samePatch(prev,shot)&&this.layEl.firstChild){
      const slots=this.layEl.firstChild.children;
      shot.items.forEach((it,k)=>{if(it.t==='img'){const im=slots[k].firstChild;im.src=url(it.id);this.applyImg(im,it)}});
      this.cur=i;this.prep(i+1);return;
    }
    const lay=h('div',{class:'lay lay-'+shot.layout+(shot.gw?' gw':'')});
    const taken=new Set();
    shot.items.forEach((it,k)=>{
      const slot=h('div',{class:'slot s'+k});
      if(it.t==='img'){const im=h('img',{class:'m',src:url(it.id),alt:''});this.applyImg(im,it);slot.append(im)}
      else{const v=this.getVid(i,it.id,it.start+off,taken);taken.add(v);v._prep=-1;v._audio=it.audio;v._wantSound=it.audio;
        v.muted=!it.audio||this.forceMute;this.loadV(v,it.id,it.start+off);slot.append(VR.canvas(v))}
      lay.append(slot);
    });
    this.layEl.replaceChildren(lay);
    this.pool.forEach(v=>{if(!taken.has(v))v.pause()});
    this.inUse=taken;this.cur=i;
    if(this.playing)taken.forEach(v=>this.playV(v));
    this.prep(i+1);
  },
  prep(j){
    const shot=this.shots[j];if(!shot)return;
    const curShot=this.shots[this.cur];
    if(this.samePatch(curShot,shot))return;
    const reserved=new Set(this.inUse);
    shot.items.forEach(it=>{
      if(it.t!=='vid')return;
      // continuing clip will be reused
      if(curShot&&curShot.items.some(x=>x.t==='vid'&&x.id===it.id&&Math.abs(x.start+curShot.dur-it.start)<0.05))return;
      const v=this.pool.find(v=>!reserved.has(v)&&v._prep!==j);
      if(!v)return;reserved.add(v);v._prep=j;v.muted=true;this.loadV(v,it.id,it.start);
    });
  },
  indexAt(t){for(let i=this.shots.length-1;i>=0;i--)if(t>=this.shots[i].at)return i;return 0},
  frame(now){
    const dt=Math.min(0.1,(now-this.last)/1000);this.last=now;
    if(this.playing){this.t+=dt;if(this.t>=this.total){this.t=this.total;this.end()}}
    if(this.shots.length){
      const i=this.indexAt(Math.min(this.t,this.total-0.001));
      if(i!==this.cur)this.enter(i,Math.max(0,this.t-this.shots[i].at));
      if(this.kb){const s=this.shots[this.cur],p=clamp((this.t-s.at)/s.dur,0,1);this.kb.el.style.transform='scale('+(this.kb.kb.a+(this.kb.kb.b-this.kb.kb.a)*p)+')'}
    }
    this.updUI();
  },
  loop(){cancelAnimationFrame(this.raf);this.last=performance.now();const f=n=>{this.frame(n);this.raf=requestAnimationFrame(f)};this.raf=requestAnimationFrame(f)},
  updUI(){this.ui.cur.textContent=fmtT(this.t);if(document.activeElement!==this.ui.seek)this.ui.seek.value=this.t;this.ui.play.textContent=this.playing?'❚❚':'▶'},
  play(){
    if(!this.shots.length)return;
    if(this.t>=this.total-0.05){this.t=0;this.cur=-1}
    if(this.endEl){this.endEl.remove();this.endEl=null}
    this.playing=true;
    if(this.cur>=0){const s=this.shots[this.cur];this.inUse.forEach(v=>{const it=s.items.find(x=>x.t==='vid'&&x.id===v._mid);if(it)this.loadV(v,v._mid,it.start+(this.t-s.at));this.playV(v)})}
    this.loop();this.updUI();
  },
  pause(){this.playing=false;this.pool.forEach(v=>v.pause());this.updUI()},
  seek(t,keep){
    this.t=clamp(t,0,Math.max(0,this.total-0.05));this.cur=-1;this.pool.forEach(v=>{v._prep=-1});
    if(this.endEl){this.endEl.remove();this.endEl=null}
    if(!this.playing)this.frame(performance.now());
  },
  end(){
    this.playing=false;this.pool.forEach(v=>v.pause());
    this.endEl=h('div',{class:'p-end'},
      btn('↺ Replay',e=>{e.stopPropagation();this.seek(0);this.play()},'okb'),
      btn('🎲 New montage',e=>{e.stopPropagation();this.newMontage()},'primary'));
    $('#pStage').append(this.endEl);
    setTimeout(()=>{if(cur()==='player'&&!this.playing)Ads.after()},700);
  },
  toggleFs(force){
    const s=$('#s-player');const on=force!=null?force:!s.classList.contains('fs');
    s.classList.toggle('fs',on);s.classList.remove('hidectl');document.body.classList.toggle('fsmode',on);
    try{if(on&&document.documentElement.requestFullscreen)document.documentElement.requestFullscreen().catch(()=>{});
      else if(!on&&document.fullscreenElement)document.exitFullscreen().catch(()=>{})}catch(e){}
    requestAnimationFrame(()=>fitStage($('#s-player .stageWrap')));
  },
  stop(){cancelAnimationFrame(this.raf);this.pause();this.toggleFs(false)},
};
HOOKS.player={show(){if(this._fromGen){this._fromGen=false}else if(Player.shots.length){Player.updUI()}},hide(){Player.stop()}};

/* =========================================================
   Prompt editor (shared)
   ========================================================= */
function promptEditor(cat,opts={}){
  const wrap=h('div',{class:'pe'});
  const multi=cat==='whbMulti',adm=cat==='adMulti',radio=cat==='roulette';
  const mkReq=(v,min,max)=>{const s=h('select',{class:'tx',style:'width:58px'});for(let i=min;i<=max;i++)s.append(h('option',{value:i},String(i)));s.value=String(v);return s};
  const mkCards=v=>{const s=h('select',{class:'tx',style:'width:58px'});[2,3,4,6].forEach(i=>s.append(h('option',{value:i},String(i))));s.value=String(v);return s};
  const inp=h('input',{type:'text',class:'tx',placeholder:opts.placeholder||'Type a new prompt…',enterkeyhint:'done'});
  const req=multi?mkReq(2,2,5):adm?mkReq(1,1,5):null,cards=adm?mkCards(4):null;
  const list=h('div');
  const add=()=>{const t=inp.value.trim();if(!t){inp.focus();return}
    const p={id:uid(),text:t};if(multi)p.req=+req.value;if(adm){p.cards=+cards.value;p.req=Math.min(+req.value,p.cards-1)}
    S.prompts[cat].unshift(p);inp.value='';markDirty();render();toast('Prompt added');opts.onChange&&opts.onChange()};
  inp.addEventListener('keydown',e=>{if(e.key==='Enter'){e.preventDefault();add()}});
  const addRow=h('div',{class:'pe-add'},inp,
    multi?h('div',null,h('div',{class:'pe-meta'},'pick'),req):null,
    adm?h('div',null,h('div',{class:'pe-meta'},'pick'),req):null,
    adm?h('div',null,h('div',{class:'pe-meta'},'cards'),cards):null,
    btn('+',add,'primary sq'));
  function render(){
    list.replaceChildren();
    const arr=S.prompts[cat];
    if(!arr.length)list.append(h('div',{class:'muted small',style:'padding:6px 2px'},'No prompts yet — add one above.'));
    arr.forEach(p=>{
      const t=h('input',{type:'text',class:'tx grow',value:p.text});
      t.addEventListener('input',()=>{p.text=t.value;markDirty()});
      const row=h('div',{class:'pe-row'});
      if(radio){const on=S.settings.roulCap==='chosen'&&S.settings.roulChosen===p.id;
        row.append(h('button',{class:'pe-radio'+(on?' on':''),onclick:()=>{S.settings.roulCap='chosen';S.settings.roulChosen=p.id;markDirty();opts.onChange&&opts.onChange();render()}}))}
      row.append(t);
      if(multi){const r=mkReq(p.req||2,2,5);r.onchange=()=>{p.req=+r.value;markDirty()};row.append(r)}
      if(adm){const r=mkReq(p.req||1,1,5),c=mkCards(p.cards||4);
        r.onchange=()=>{p.req=+r.value;if(p.req>=p.cards){p.cards=[2,3,4,6].find(x=>x>p.req)||6;c.value=String(p.cards)}markDirty()};
        c.onchange=()=>{p.cards=+c.value;if(p.req>=p.cards){p.req=p.cards-1;r.value=String(p.req)}markDirty()};row.append(r,c)}
      row.append(h('button',{class:'pe-del',onclick:()=>{const i=arr.indexOf(p);arr.splice(i,1);markDirty();render();opts.onChange&&opts.onChange()}},'×'));
      list.append(row);
    });
  }
  render();
  wrap.append(addRow,list);
  return wrap;
}
const CAT_INFO={
  wyr:['Would You Rather prompts','Shown on top of the video. The picked person’s name appears in front of the prompt — or put {name} where you want it.'],
  whbSingle:['Who Has The Best — single pick','Player selects 1 card. The name appears in front of the prompt ({name} works too).'],
  whbMulti:['Who Has The Best — multiple choice','“pick” = how many cards must be selected.'],
  roulette:['Roulette captions','Tap the circle to always use one, or use Random.'],
  adPhoto:['Photo ad prompts','Text shown on photo pop-up ads.'],
  adMulti:['Card ad prompts (multiple choice)','“pick” = cards to select, “cards” = cards shown.'],
};
function section(cat,open){
  return h('details',{class:'sec',open},h('summary',null,h('span',null,CAT_INFO[cat][0],' ',h('span',{class:'muted small'},'('+S.prompts[cat].length+')'))),
    h('div',{class:'body'},h('div',{class:'muted small',style:'margin-bottom:10px'},CAT_INFO[cat][1]),promptEditor(cat)));
}
let promptsFocus=null;
function openPrompts(which){promptsFocus=which;go('prompts')}
HOOKS.prompts={show(){
  const b=$('#promptsBody');const f=promptsFocus;
  const order={wyr:['wyr'],whb:['whbSingle','whbMulti'],roulette:['roulette'],ads:['adPhoto','adMulti']};
  const first=f?order[f]:[];
  const all=['wyr','whbSingle','whbMulti','roulette','adPhoto','adMulti'];
  const rest=all.filter(c=>!first.includes(c));
  b.replaceChildren(h('div',{class:'muted small',style:'margin:0 2px 12px'},'Everything saves automatically. Edit any prompt by tapping its text.'),
    ...first.map(c=>section(c,true)),...rest.map(c=>section(c,!f)));
}};

/* =========================================================
   Form helpers
   ========================================================= */
function playModeRow(){
  const seg=h('div',{class:'seg',style:'margin-top:8px'}),note=h('div',{class:'muted small',style:'margin-top:6px'});
  const draw=()=>{const cur=S.settings.playMode||'auto';
    seg.replaceChildren(...[['auto','Auto'],['real','HD video'],['frames','Frames']].map(([k,l])=>h('button',{class:cur===k?'on':'',onclick:()=>{
      S.settings.playMode=k;if(k!=='auto'){try{localStorage.removeItem('tm_forcedfs')}catch(e){}}markDirty();PLAY.apply(PLAY.decide());draw()}},l)));
    note.textContent='Now playing clips as: '+(PLAY.real?'HD video':'Frames (24 fps)')+'. Koder’s preview forces videos fullscreen, so it needs Frames — open the app in Safari for HD.'};
  draw();
  return h('div',{class:'sl'},h('div',{class:'top'},h('span',null,'Clip playback')),seg,note);
}
function toggleRow(label,sub,get,set){
  const i=h('input',{type:'checkbox',checked:!!get()});i.onchange=()=>{set(i.checked);markDirty()};
  return h('label',{class:'field'},h('div',{class:'lb'},label,sub?h('small',null,sub):null),h('span',{class:'sw'},i,h('i')));
}
function sliderRow(label,min,max,step,get,set,fmt=v=>v){
  const v=h('b',null,fmt(get()));const r=h('input',{type:'range',min,max,step,value:String(get())});
  r.oninput=()=>{set(parseFloat(r.value));v.textContent=fmt(parseFloat(r.value));markDirty()};
  return h('div',{class:'sl'},h('div',{class:'top'},h('span',null,label),v),r);
}
function chipRow(label,opts,arr){
  const c=h('div',{class:'chips'});
  const draw=()=>{c.replaceChildren(...opts.map(o=>h('button',{class:'chip'+(arr.includes(o)?' on':''),onclick:()=>{const i=arr.indexOf(o);if(i>=0){if(arr.length>1)arr.splice(i,1);else{toast('Keep at least one');return}}else arr.push(o);arr.sort((a,b)=>a-b);markDirty();draw()}},String(o))))};
  draw();return h('div',{class:'field'},h('div',{class:'lb'},label),c);
}

/* =========================================================
   Home
   ========================================================= */
let storageBanner='';
HOOKS.home={show(){
  const nP=count('photo'),nC=count('clip');
  const tile=(ic,tt,ts,fn,cls='')=>h('button',{class:'tile '+cls,onclick:fn},h('div',{class:'ic'},ic),h('div',null,h('div',{class:'tt'},tt),h('div',{class:'ts'},ts)));
  $('#homeBody').replaceChildren(
    storageBanner?h('div',{class:'banner bad'},storageBanner):null,
    (!nP||!nC)?h('div',{class:'banner'},'Start here → open the Media Pool and add your photos (cards) and video clips.'):null,
    h('div',{class:'hero'},h('b',null,'Template Maker'),h('span',{class:'muted small'},`${nP} photos · ${nC} clips · ${promptCount()} prompts`)),
    h('div',{class:'tiles'},
      tile('🤔','Would You Rather','Video + 2–4 cards',()=>{WYR.st=null;go('wyr')},'big g1'),
      tile('🎰','Roulette','30s random montage',()=>go('roul'),'big g2'),
      tile('🏆','Who Has The Best','2 / 4 / 6 photos',()=>{WHB.st=null;go('whb')},'big g3'),
    ),
    h('div',{class:'tiles t3',style:'margin-top:10px'},
      tile('🖼','Media Pool',`${nP} photos · ${nC} clips`,()=>go('media')),
      tile('👤','People',`${S.people.length} people`,()=>go('people')),
      tile('📣','Ad Options',S.settings.ads.enabled?'Ads on':'Ads off',()=>go('ads'))),
    h('div',{class:'tiles',style:'margin-top:10px'},
      tile('✎','Prompts',`${promptCount()} saved`,()=>openPrompts(null)),
      tile('⚙','Settings & Backup','Save · export · restore',()=>go('settings'))
    ));
}};

/* =========================================================
   Roulette setup screen (with prompt maker)
   ========================================================= */
HOOKS.roul={show(){
  const b=$('#roulBody');const set=S.settings;
  const nP=count('photo'),nC=count('clip');
  const seg=h('div',{class:'seg'});
  const drawSeg=()=>{seg.replaceChildren(...[['random','🎲 Random'],['chosen','📌 Chosen'],['none','No caption']].map(([k,l])=>h('button',{class:set.roulCap===k?'on':'',onclick:()=>{
    if(k==='chosen'&&!S.prompts.roulette.find(p=>p.id===set.roulChosen)){toast('Tap the circle next to a caption to choose it');return}
    set.roulCap=k;markDirty();drawSeg();ed.replaceWith(ed=promptEditor('roulette',{placeholder:'Write a caption for the montage…',onChange:drawSeg}))}},l)))};
  let ed=promptEditor('roulette',{placeholder:'Write a caption for the montage…',onChange:()=>{drawSeg()}});
  drawSeg();
  const genBtn=btn('🎲 Generate montage',()=>{
    if(!nP&&!nC){toast('Add photos or clips first');go('media');return}
    HOOKS.player._fromGen=true;go('player');Player.newMontage();
  },'primary full');genBtn.style.height='58px';genBtn.style.fontSize='18px';
  b.replaceChildren(
    (!nP&&!nC)?h('div',{class:'banner'},'Your media pool is empty — add photos and clips first.'):null,
    genBtn,
    h('div',{class:'muted small',style:'text-align:center;margin:8px 0 4px'},`Shuffles ${nP} photos and ${nC} clips into a new edit every time.`),
    previousBtn(),
    h('h2',null,'Montage'),
    h('div',{class:'card-p'},
      sliderRow('Length',10,60,1,()=>set.roulDur,v=>set.roulDur=v,v=>v+'s'),
      sliderRow('Photo flash speed',0.2,1,0.05,()=>set.flash,v=>set.flash=v,v=>(+v).toFixed(2)+'s')),
    h('h2',null,'Prompt maker — captions'),
    h('div',{class:'card-p'},seg,h('div',{style:'height:12px'}),ed));
}};
function previousBtn(){return Player.shots.length?btn('▶ Watch last montage again',()=>{HOOKS.player._fromGen=true;go('player');Player.seek(0);Player.unlock();Player.play()},'full'):null}

/* =========================================================
   Media Pool
   ========================================================= */
let mediaTab='photo',selecting=false;const selected=new Set();
function photoThumb(u){return new Promise(res=>{const im=new Image();im.onload=()=>{try{const s=Math.min(1,360/Math.max(im.width,im.height));const c=document.createElement('canvas');c.width=Math.max(1,Math.round(im.width*s));c.height=Math.max(1,Math.round(im.height*s));c.getContext('2d').drawImage(im,0,0,c.width,c.height);res(c.toDataURL('image/jpeg',.7))}catch(e){res(null)}};im.onerror=()=>res(null);im.src=u;setTimeout(()=>res(null),10000)})}
function videoThumb(u){return new Promise(res=>{
  const v=document.createElement('video');v.muted=true;v.playsInline=true;v.setAttribute('playsinline','');v.preload='auto';
  let dur=0,done=false;const fin=t=>{if(done)return;done=true;v.removeAttribute('src');v.load();res({thumb:t,dur})};
  v.onloadedmetadata=()=>{dur=isFinite(v.duration)?v.duration:0;try{v.currentTime=Math.min(0.6,dur/3||0)}catch(e){}};
  v.onseeked=()=>{try{const s=Math.min(1,360/Math.max(v.videoWidth,v.videoHeight));const c=document.createElement('canvas');c.width=Math.max(1,Math.round(v.videoWidth*s));c.height=Math.max(1,Math.round(v.videoHeight*s));c.getContext('2d').drawImage(v,0,0,c.width,c.height);fin(c.toDataURL('image/jpeg',.7))}catch(e){fin(null)}};
  v.onerror=()=>fin(null);v.src=u;setTimeout(()=>fin(null),9000);
})}
function progress(text){const bar=h('i');const t=h('div',null,text);const p=h('div',{class:'prog'},t,h('div',{class:'bar'},bar));document.body.append(p);return{set(n,tot,txt){bar.style.width=(n/tot*100)+'%';if(txt)t.textContent=txt},close(){p.remove()}}}
async function importFiles(files,kind){
  files=[...files];if(!files.length)return;
  const pr=progress(`Adding ${files.length} ${kind==='photo'?'photos':'clips'}…`);
  let ok=0,stored=0;const newIds=[];
  for(let i=0;i<files.length;i++){
    const f=files[i];pr.set(i,files.length,`Adding ${i+1} / ${files.length}…`);
    const id=uid();const u=URL.createObjectURL(f);let thumb=null,dur=0;
    try{if(kind==='photo')thumb=await photoThumb(u);else{const r=await videoThumb(u);thumb=r.thumb;dur=r.dur}}catch(e){}
    URL.revokeObjectURL(u);
    const rec={id,kind,name:f.name,type:f.type||'',dur,thumb,added:Date.now()+i};
    if(await storeMedia(rec,f))stored++;
    addMediaToMemory(rec,f);ok++;newIds.push(id);
  }
  if(kind==='clip')Conv.add(newIds);
  pr.close();
  if(stored===ok)toast(`✓ Added & saved ${ok} ${kind==='photo'?'photos':'clips'}`);
  else toast(`Added ${ok} — but only ${stored} could be saved to this device. Make a backup!`,4500);
  HOOKS.media.show();
}
$('#filePhotos').onchange=e=>{importFiles(e.target.files,'photo');e.target.value=''};
$('#fileClips').onchange=e=>{importFiles(e.target.files,'clip');e.target.value=''};
async function deleteMedia(ids){
  for(const id of ids){const m=M(id);if(!m)continue;try{DB.ok&&await DB.del('media',id)}catch(e){}try{DB.ok&&await DB.del('frames',id)}catch(e){}FR.map.delete(id);
    URL.revokeObjectURL(m.url);mediaMap.delete(id);delete S.tags[id];S.media.splice(S.media.indexOf(m),1)}
  markDirty();
}
function thumbOf(m){return m.thumb?h('img',{src:m.thumb,loading:'lazy',alt:''}):(m.kind==='photo'?h('img',{src:m.url,loading:'lazy',alt:''}):h('div',{class:'ph'},'🎬'))}
function photosOf(pid){return S.media.filter(m=>m.kind==='photo'&&S.tags[m.id]===pid)}
function assignPhotos(ids,pid){ids.forEach(id=>{if(pid)S.tags[id]=pid;else delete S.tags[id]});markDirty()}
function addPerson(name){name=name.trim().replace(/\s+/g,' ');const ex=S.people.find(p=>p.name.toLowerCase()===name.toLowerCase());if(ex)return ex;const p={id:uid(),name};S.people.push(p);markDirty();return p}
function avatar(p){const ph=photosOf(p.id);const m=ph[ph.length-1];return h('span',{class:'av'},m?h('img',{src:m.thumb||m.url,alt:''}):p.name.split(' ').map(w=>w[0]||'').join('').slice(0,2).toUpperCase())}
const byName=(a,b)=>a.name.localeCompare(b.name);
// Choose who a photo belongs to. Resolves to a person id, '' (remove name) or null (cancel).
function personPicker(title){
  return new Promise(res=>{
    const back=h('div',{class:'mod-back'});const close=v=>{back.remove();res(v)};
    const q=h('input',{type:'text',class:'tx',placeholder:'Search or type a new name…',style:'width:100%;margin-bottom:8px'});
    const list=h('div',{class:'plist',style:'max-height:48vh;overflow:auto'});
    const draw=()=>{
      const raw=q.value.trim(),t=raw.toLowerCase();list.replaceChildren();
      if(raw&&!S.people.some(p=>p.name.toLowerCase()===t))list.append(h('button',{class:'pr',onclick:()=>close(addPerson(raw).id)},h('span',{class:'av'},'+'),h('span',{class:'pn'},'Add “'+raw+'”')));
      S.people.filter(p=>!t||p.name.toLowerCase().includes(t)).sort(byName).forEach(p=>list.append(
        h('button',{class:'pr',onclick:()=>close(p.id)},avatar(p),h('span',{class:'pn'},p.name),h('span',{class:'pc'},String(photosOf(p.id).length)))));
    };
    q.addEventListener('input',draw);draw();
    back.append(h('div',{class:'mod'},h('h3',null,title),q,list,h('div',{class:'row'},btn('Cancel',()=>close(null)),btn('Remove name',()=>close(''),'warn'))));
    back.addEventListener('click',e=>{if(e.target===back)close(null)});
    document.body.append(back);
  });
}
// Choose photos from the media pool to give to one person.
function photoPicker(p){
  return new Promise(res=>{
    const back=h('div',{class:'mod-back'});const close=v=>{back.remove();res(v)};
    const sel=new Set();let filt=S.media.some(m=>m.kind==='photo'&&!personOf(m.id))?'untagged':'all';
    const grid=h('div',{class:'mgrid selecting'}),seg=h('div',{class:'seg',style:'margin-bottom:8px'});
    const ok=btn('',()=>{if(sel.size)close([...sel])},'primary');
    const upd=()=>{ok.textContent=sel.size?`Add ${sel.size}`:'Select photos';ok.classList.toggle('disabled',!sel.size)};
    const draw=()=>{
      seg.replaceChildren(...[['untagged','No name'],['all','All photos']].map(([k,l])=>h('button',{class:filt===k?'on':'',onclick:()=>{filt=k;draw()}},l)));
      const list=S.media.filter(m=>m.kind==='photo'&&S.tags[m.id]!==p.id&&(filt==='all'||!personOf(m.id))).slice().reverse();
      if(!list.length)grid.replaceChildren(h('div',{class:'muted small',style:'grid-column:1/-1;padding:24px;text-align:center'},filt==='untagged'?'Every photo already has a name.':'No photos yet.'));
      else grid.replaceChildren(...list.map(m=>{const o=personOf(m.id);const it=h('div',{class:'mitem'+(sel.has(m.id)?' sel':''),onclick:()=>{sel.has(m.id)?sel.delete(m.id):sel.add(m.id);it.classList.toggle('sel');upd()}},thumbOf(m),o?h('span',{class:'tag'},o.name):null,h('span',{class:'ck'}));return it}));
      upd();
    };
    back.append(h('div',{class:'mod'},h('h3',null,'Add photos → '+p.name),seg,h('div',{style:'max-height:52vh;overflow:auto;margin-bottom:6px'},grid),h('div',{class:'row'},btn('Cancel',()=>close(null)),ok)));
    back.addEventListener('click',e=>{if(e.target===back)close(null)});
    document.body.append(back);draw();
  });
}
function previewMedia(m){
  let f=null,box;
  if(m.kind==='photo')box=h('div',{class:'preview'},h('img',{class:'m',src:m.url}));
  else if(PLAY.real||FR.map.has(m.id)){f=new Flip();f.setClip(m.id);f.muted=true;f.play();box=h('div',{class:'preview'},VR.canvas(f))}
  else box=h('div',{class:'preview'},h('div',{class:'empty'},Conv.err[m.id]?'⚠ '+Conv.err[m.id]:'⏳ Still preparing this clip…'));
  const who=m.kind==='photo'?personOf(m.id):null;
  const btns=[{label:'Close',value:0}];if(m.kind==='photo')btns.push({label:'👤 Name',value:2});if(m.kind==='clip'&&Conv.err[m.id])btns.push({label:'Retry',value:3});btns.push({label:'Delete',value:1,cls:'warn'});
  modal(m.kind==='photo'?(who?who.name:'Photo · no name'):'Clip'+(m.dur?' · '+fmtT(m.dur):''),null,btns,box).then(async v=>{
    if(f){f.pause();Flips.delete(f)}
    if(v===1){await deleteMedia([m.id]);toast('Deleted');HOOKS.media.show()}
    if(v===3){Conv.add([m.id]);HOOKS.media.show()}
    if(v===2){const pid=await personPicker('Who is this?');if(pid!==null){assignPhotos([m.id],pid);toast(pid?'✓ '+S.people.find(p=>p.id===pid).name:'Name removed');HOOKS.media.show()}}
  });
}
let mediaFilter='all';
HOOKS.media={show(){
  const b=$('#mediaBody');const nP=count('photo'),nC=count('clip');
  const untagged=S.media.filter(m=>m.kind==='photo'&&!personOf(m.id)).length;
  let list=S.media.filter(m=>m.kind===mediaTab);
  if(mediaTab==='photo'&&mediaFilter==='untagged')list=list.filter(m=>!personOf(m.id));
  list=list.slice().reverse();
  const tabs=h('div',{class:'tabs'},
    h('button',{class:mediaTab==='photo'?'on':'',onclick:()=>{mediaTab='photo';selecting=false;selected.clear();HOOKS.media.show()}},`🖼 Photos / Cards (${nP})`),
    h('button',{class:mediaTab==='clip'?'on':'',onclick:()=>{mediaTab='clip';selecting=false;selected.clear();HOOKS.media.show()}},`🎬 Clips (${nC})`));
  const addB=btn(mediaTab==='photo'?'+ Add photos':'+ Add clips',()=>$(mediaTab==='photo'?'#filePhotos':'#fileClips').click(),'primary');
  const selB=list.length||selecting?btn(selecting?'Done':'Select',()=>{selecting=!selecting;selected.clear();HOOKS.media.show()}):null;
  const filt=mediaTab==='photo'&&nP?h('div',{class:'chips',style:'margin-bottom:10px'},
    h('button',{class:'chip'+(mediaFilter==='all'?' on':''),onclick:()=>{mediaFilter='all';selected.clear();HOOKS.media.show()}},`All ${nP}`),
    h('button',{class:'chip'+(mediaFilter==='untagged'?' on':''),onclick:()=>{mediaFilter='untagged';selected.clear();HOOKS.media.show()}},`No name ${untagged}`)):null;
  const grid=h('div',{class:'mgrid'+(selecting?' selecting':'')});
  list.forEach(m=>{
    const who=m.kind==='photo'?personOf(m.id):null;
    const it=h('div',{class:'mitem'+(selected.has(m.id)?' sel':''),onclick:()=>{
      if(selecting){selected.has(m.id)?selected.delete(m.id):selected.add(m.id);it.classList.toggle('sel');updBar()}else previewMedia(m)}},
      thumbOf(m),m.kind==='clip'?h('span',{class:'dur'},(PLAY.real||FR.map.has(m.id))?(m.dur?fmtT(m.dur):'▶'):(Conv.err[m.id]?'⚠ failed':'⏳ preparing')):null,who?h('span',{class:'tag'},who.name):null,h('span',{class:'ck'}));
    grid.append(it);
  });
  const bar=h('div',{class:'mbar'});
  const updBar=()=>{bar.replaceChildren();if(!selecting)return;
    const none=selected.size?'':' disabled';
    bar.append(btn(selected.size===list.length&&list.length?'None':'All',()=>{if(selected.size===list.length)selected.clear();else list.forEach(m=>selected.add(m.id));HOOKS.media.show()}),
      mediaTab==='photo'?btn(`👤 Name ${selected.size}`,async()=>{if(!selected.size)return;const pid=await personPicker(`Who is in ${selected.size>1?'these '+selected.size+' photos':'this photo'}?`);
        if(pid===null)return;const n=selected.size;assignPhotos([...selected],pid);selected.clear();toast(pid?`✓ ${n} → ${S.people.find(p=>p.id===pid).name}`:'Names removed');HOOKS.media.show()},'primary'+none):null,
      btn(`🗑 ${selected.size}`,async()=>{if(!selected.size)return;if(await confirmBox('Delete '+selected.size+' item(s)?','This removes them from the media pool on this device.','Delete','warn')){await deleteMedia([...selected]);selected.clear();selecting=false;toast('Deleted');HOOKS.media.show()}},'warn'+none))};
  updBar();
  const emptyTxt=mediaTab==='photo'&&mediaFilter==='untagged'&&nP?'✓ Every photo has a name.':'Nothing here yet. Tap “Add” to pick from your camera roll — you can select many at once.';
  b.replaceChildren(tabs,h('div',{class:'row',style:'margin-bottom:12px'},addB,selB),filt,
    h('div',{class:'muted small',style:'margin:0 2px 10px'},mediaTab==='photo'?(selecting?'Tap photos, then “Name” to give them to a person.':'Tap a photo to name it, or “Select” to name many at once.'):(PLAY.real?'Clips play in HD in Would You Rather and the Roulette montage.':'Frames mode (Koder): new clips take a few seconds to prepare, and the first 15s of each clip is used. Open the app in Safari for HD video.')),
    list.length?grid:h('div',{class:'empty',style:'position:static;padding:40px 20px'},h('div',{style:'font-size:40px'},mediaTab==='photo'?'🖼':'🎬'),emptyTxt),
    bar);
}};

/* =========================================================
   People
   ========================================================= */
let curPerson=null;
function openPerson(pid){curPerson=pid;go('person')}
HOOKS.people={show(){
  const b=$('#peopleBody');
  const q=h('input',{type:'text',class:'tx',placeholder:'Search or add a name…',enterkeyhint:'done',style:'flex:1;min-width:0'});
  const list=h('div',{class:'plist'});
  const untagged=S.media.filter(m=>m.kind==='photo'&&!personOf(m.id)).length;
  const tagged=count('photo')-untagged;
  const draw=()=>{const t=q.value.trim().toLowerCase();list.replaceChildren();
    const ppl=S.people.filter(p=>!t||p.name.toLowerCase().includes(t)).sort(byName);
    if(!ppl.length)list.append(h('div',{class:'muted small',style:'padding:10px 4px'},t?'No match — tap “+ Add” to add this person.':'No people yet.'));
    ppl.forEach(p=>{const n=photosOf(p.id).length;list.append(h('button',{class:'pr',onclick:()=>openPerson(p.id)},avatar(p),h('span',{class:'pn'},p.name),h('span',{class:'pc'},n+(n===1?' photo':' photos')),h('span',{class:'muted',style:'font-size:20px'},'›')))})};
  const add=()=>{const v=q.value.trim();if(!v){q.focus();return}const before=S.people.length;const p=addPerson(v);q.value='';draw();toast(S.people.length>before?'✓ Added '+p.name:p.name+' is already on the list')};
  q.addEventListener('input',draw);q.addEventListener('keydown',e=>{if(e.key==='Enter'){e.preventDefault();add()}});
  b.replaceChildren(h('div',{class:'pe-add'},q,btn('+ Add',add,'primary nofx')),
    h('div',{class:'muted small',style:'margin:2px 2px 10px'},`${S.people.length} people · ${tagged} photos named · ${untagged} without a name`),
    untagged?btn(`🏷 Name the ${untagged} photo${untagged>1?'s':''} without a name`,()=>{mediaTab='photo';mediaFilter='untagged';selecting=true;selected.clear();go('media')},'full'):null,
    h('div',{style:'height:6px'}),list);
  draw();
}};
HOOKS.person={show(){
  const p=S.people.find(x=>x.id===curPerson);if(!p){back();return}
  $('#title').textContent=p.name;
  const b=$('#personBody');
  const nameIn=h('input',{type:'text',class:'tx',value:p.name,style:'width:100%'});
  nameIn.addEventListener('input',()=>{const v=nameIn.value.trim();if(v){p.name=v;$('#title').textContent=v;markDirty()}});
  const ph=photosOf(p.id).slice().reverse();
  const grid=h('div',{class:'mgrid'});
  ph.forEach(m=>grid.append(h('div',{class:'mitem',onclick:()=>{
    const box=h('div',{class:'preview'},h('img',{class:'m',src:m.url}));
    modal(p.name,null,[{label:'Close',value:0},{label:'Remove name',value:1,cls:'warn'}],box).then(v=>{if(v===1){assignPhotos([m.id],'');HOOKS.person.show()}})}},thumbOf(m))));
  b.replaceChildren(
    h('div',{class:'card-p'},h('div',{class:'muted small',style:'margin-bottom:6px'},'Name'),nameIn),
    btn('+ Add photos from Media Pool',async()=>{const ids=await photoPicker(p);if(ids&&ids.length){assignPhotos(ids,p.id);toast(`✓ ${ids.length} photo${ids.length>1?'s':''} → ${p.name}`);HOOKS.person.show()}},'primary full'),
    h('h2',null,`Photos (${ph.length})`),
    ph.length?grid:h('div',{class:'muted small',style:'padding:6px 2px'},'No photos yet — tap “Add photos” above.'),
    h('div',{style:'height:28px'}),
    btn('Delete person',async()=>{if(await confirmBox('Delete '+p.name+'?','Their photos stay in the Media Pool, just without a name.','Delete','warn')){Object.keys(S.tags).forEach(k=>{if(S.tags[k]===p.id)delete S.tags[k]});S.people.splice(S.people.indexOf(p),1);markDirty();back()}},'warn full'));
}};

/* =========================================================
   Ad options screen
   ========================================================= */
HOOKS.ads={show(){
  const a=S.settings.ads;const b=$('#adsBody');
  b.replaceChildren(
    h('div',{class:'row',style:'margin-bottom:12px'},btn('Preview photo ad',()=>Ads.show('photo')),btn('Preview card ad',()=>Ads.show('multi'))),
    h('div',{class:'card-p'},
      toggleRow('Ads on','Master switch for all pop-ups',()=>a.enabled,v=>a.enabled=v),
      toggleRow('Photo ads','Photo + your prompt',()=>a.photo,v=>a.photo=v),
      toggleRow('Card ads','Multiple-choice cards',()=>a.multi,v=>a.multi=v)),
    h('h2',null,'When ads show'),
    h('div',{class:'muted small',style:'margin:-4px 2px 10px'},'Ads never pop up while you play — they only show as the next screen after you press a button.'),
    h('div',{class:'card-p'},
      toggleRow('After picking','When you press Next',()=>a.afterNext,v=>a.afterNext=v),
      sliderRow('After-pick chance',0,100,5,()=>a.afterNextChance,v=>a.afterNextChance=v,v=>v+'%'),
      toggleRow('After a round','When you press Next round',()=>a.between,v=>a.between=v),
      sliderRow('After-round chance',0,100,5,()=>a.betweenChance,v=>a.betweenChance=v,v=>v+'%'),
      toggleRow('After montages','When a Roulette video ends',()=>a.after,v=>a.after=v),
      sliderRow('After-montage chance',0,100,5,()=>a.afterChance,v=>a.afterChance=v,v=>v+'%'),
      sliderRow('Skip button wait',0,10,1,()=>a.skipDelay,v=>a.skipDelay=v,v=>v+'s')),
    h('h2',null,'Ad prompts'),
    section('adPhoto',true),section('adMulti',true));
}};

/* =========================================================
   Settings & Backup
   ========================================================= */
function bytes(n){if(n>1e9)return (n/1e9).toFixed(2)+' GB';if(n>1e6)return (n/1e6).toFixed(1)+' MB';return Math.round(n/1e3)+' KB'}
function deliverFile(blob,name){
  let f;try{f=new File([blob],name,{type:'application/octet-stream'})}catch(e){f=null}
  if(f&&navigator.canShare&&navigator.canShare({files:[f]})){
    navigator.share({files:[f],title:name}).then(()=>toast('✓ Backup shared')).catch(e=>{if(e&&e.name!=='AbortError')download(blob,name)});return;
  }
  download(blob,name);
}
function download(blob,name){const a=h('a',{href:URL.createObjectURL(blob),download:name});document.body.append(a);a.click();setTimeout(()=>{URL.revokeObjectURL(a.href);a.remove()},4000);toast('Backup file created — check your downloads',3000)}
function exportBackup(withMedia){
  const header={app:'templateMaker',v:2,date:new Date().toISOString(),prompts:S.prompts,settings:S.settings,people:S.people,tags:S.tags,media:[]};
  const parts=[];
  if(withMedia)for(const m of S.media){header.media.push({id:m.id,kind:m.kind,name:m.name,type:m.type||m.blob.type||'',dur:m.dur,thumb:m.thumb,added:m.added,size:m.blob.size});parts.push(m.blob)}
  const hj=new TextEncoder().encode(JSON.stringify(header));
  const blob=new Blob(['TMBK1\n',String(hj.length).padStart(12,'0'),hj,...parts],{type:'application/octet-stream'});
  const d=new Date().toISOString().slice(0,10);
  deliverFile(blob,`template-maker-${withMedia?'full':'prompts'}-${d}.tmbk`);
}
$('#fileBackup').onchange=async e=>{
  const f=e.target.files[0];e.target.value='';if(!f)return;
  try{
    const head=await f.slice(0,18).text();
    if(!head.startsWith('TMBK1'))throw new Error('Not a Template Maker backup file');
    const len=parseInt(head.slice(6,18),10);
    const hd=JSON.parse(await f.slice(18,18+len).text());
    const mode=await modal('Restore backup?',`${hd.media.length} media files · ${Object.values(hd.prompts||{}).reduce((n,a)=>n+a.length,0)} prompts (from ${(hd.date||'').slice(0,10)}).`,
      [{label:'Cancel',value:null},{label:'Merge',value:'merge'},{label:'Replace all',value:'replace',cls:'warn'}]);
    if(!mode)return;
    if(mode==='replace'){await deleteMedia(S.media.map(m=>m.id))}
    mergeState({prompts:hd.prompts,settings:hd.settings});
    const idMap={};
    if(mode==='replace')S.tags={};
    if(Array.isArray(hd.people)){if(mode==='replace')S.people=[];
      for(const p of hd.people){const ex=S.people.find(x=>x.id===p.id)||S.people.find(x=>x.name.toLowerCase()===p.name.toLowerCase());
        if(ex)idMap[p.id]=ex.id;else{S.people.push(p);idMap[p.id]=p.id}}}
    let off=18+len;const pr=progress('Restoring…');let n=0;
    for(const m of hd.media){
      const part=f.slice(off,off+m.size,m.type);off+=m.size;n++;pr.set(n,hd.media.length,`Restoring ${n} / ${hd.media.length}…`);
      if(M(m.id))continue;
      const blob=new Blob([await part.arrayBuffer()],{type:m.type});
      await storeMedia(m,blob);addMediaToMemory(m,blob);
    }
    for(const [mid,pid] of Object.entries(hd.tags||{}))if(M(mid)&&idMap[pid])S.tags[mid]=idMap[pid];
    pr.close();await saveNow(false);Conv.add(S.media.filter(m=>m.kind==='clip').map(m=>m.id));toast('✓ Backup restored');HOOKS.settings.show();
  }catch(err){toast('⚠ '+err.message,4000)}
};
HOOKS.settings={async show(){
  const b=$('#setBody');const set=S.settings;
  const st=h('div',{class:'muted small'},'Checking storage…');
  b.replaceChildren(
    h('div',{class:'card-p'},
      h('div',{style:'font-weight:800;margin-bottom:6px'},DB.ok?'✓ Device storage is working':'⚠ Device storage is blocked here'),st,
      h('div',{style:'height:12px'}),
      btn('💾 Save everything now',()=>saveNow(true),'primary full')),
    h('h2',null,'Backup (extra safety)'),
    h('div',{class:'card-p'},
      h('div',{class:'muted small',style:'margin-bottom:10px'},'Everything saves automatically on this device. A backup file protects you if the app ever clears its data. Save it to Files, then restore it here any time.'),
      h('div',{style:'display:grid;gap:8px'},
        btn('⬇ Full backup (media + prompts)',()=>exportBackup(true),'full'),
        btn('⬇ Prompts & settings only (small)',()=>exportBackup(false),'full'),
        btn('⬆ Restore from a backup file',()=>$('#fileBackup').click(),'full'))),
    h('h2',null,'Game options'),
    h('div',{class:'card-p'},
      toggleRow('Show names on cards','While picking',()=>set.showNames,v=>set.showNames=v),
      toggleRow('Clip sound','Play each clip’s own audio',()=>set.clipSound!==false,v=>{set.clipSound=v;Flips.forEach(f=>{f.muted=f._m})}),
      playModeRow(),
      chipRow('Would You Rather cards',[2,3,4],set.wyrCounts),
      chipRow('Who Has The Best photos',[2,4,6],set.whbCounts),
      sliderRow('Multiple-choice rounds (Who Has The Best)',0,100,5,()=>set.whbMultiChance,v=>set.whbMultiChance=v,v=>v+'%')),
    h('h2',null,'Danger zone'),
    h('div',{class:'card-p',style:'display:grid;gap:8px'},
      btn('Reset prompts to examples',async()=>{if(await confirmBox('Reset prompts?','Replaces ALL your prompts with the example ones.','Reset','warn')){S.prompts=defaultPrompts();await saveNow(false);toast('Prompts reset')}},'warn full'),
      btn('Delete all media',async()=>{if(await confirmBox('Delete all media?','Removes every photo and clip from this device.','Delete all','warn')){await deleteMedia(S.media.map(m=>m.id));toast('All media deleted');HOOKS.settings.show()}},'warn full')));
  let txt=`${count('photo')} photos · ${count('clip')} clips · ${promptCount()} prompts.`;
  try{if(navigator.storage&&navigator.storage.estimate){const e=await navigator.storage.estimate();txt+=` Using ${bytes(e.usage||0)}${e.quota?' of '+bytes(e.quota):''}.`}
    if(navigator.storage&&navigator.storage.persisted){const p=await navigator.storage.persisted();txt+=p?' Protected from auto-clearing ✓':''}}catch(e){}
  if(!DB.ok)txt+=' This viewer won’t keep photos/videos after closing — use Full backup, or open the file in Safari instead.';
  st.textContent=txt;
}};

/* =========================================================
   Boot
   ========================================================= */
(async()=>{
  Player.init();
  await DB.open();
  if(DB.ok){try{await DB.put('kv',{k:'probe',v:Date.now()})}catch(e){DB.ok=false}}
  if(!DB.ok)storageBanner='⚠ This viewer is blocking device storage. Prompts are kept, but photos/videos may not survive closing. Use Settings → Full backup.';
  await loadAll();
  await FR.loadAll();
  PLAY.real=PLAY.decide();PLAY._applied=true;
  Conv.add(S.media.filter(m=>m.kind==='clip').map(m=>m.id));
  try{navigator.storage&&navigator.storage.persist&&navigator.storage.persist()}catch(e){}
  showScreen('home');
})();
</script>
</body>
</html>
