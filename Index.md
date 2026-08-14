<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ระบบนัดหมายและบันทึกการรักษา – นักศึกษาฝึกประสบการณ์นวดไทย 800 ชม.</title>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@2.44.0/tabler-icons.min.css">
<style>
:root{
  --teal-deep:#065040;
  --teal-mid:#0E7B60;
  --teal-bright:#1BB087;
  --teal-light:#E0F5EE;
  --teal-pale:#F0FAF6;
  --gold:#C8922A;
  --gold-light:#FDF3E0;
  --red-soft:#E05252;
  --red-light:#FEF0F0;
  --navy:#1A2B4A;
  --text:#1C1C1A;
  --text-sub:#5A6672;
  --border:#D4E8DF;
  --bg:#F2F7F4;
  --white:#FFFFFF;
  --radius:14px;
  --shadow:0 2px 12px rgba(6,80,64,.08);
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:'Kanit',sans-serif;background:var(--bg);color:var(--text);min-height:100vh}

/* ─── TOPBAR ─────────────────────────────────── */
.topbar{background:linear-gradient(135deg,var(--teal-deep) 0%,var(--teal-mid) 60%,var(--teal-bright) 100%);padding:0;box-shadow:0 3px 16px rgba(6,80,64,.25)}
.tb-inner{padding:16px 16px 0}
.tb-brand{display:flex;align-items:center;gap:11px;margin-bottom:12px}
.tb-ico{width:46px;height:46px;background:rgba(255,255,255,.16);border-radius:13px;display:flex;align-items:center;justify-content:center;border:1.5px solid rgba(255,255,255,.25);flex-shrink:0}
.tb-title{font-size:15px;font-weight:700;color:#fff;line-height:1.3}
.tb-sub{font-size:10px;font-weight:300;color:rgba(255,255,255,.78);margin-top:2px}
.tb-chips{display:flex;gap:7px;flex-wrap:wrap;margin-bottom:12px}
.chip{display:inline-flex;align-items:center;gap:5px;background:rgba(255,255,255,.13);border:1px solid rgba(255,255,255,.2);border-radius:16px;padding:4px 11px;font-size:10px;color:rgba(255,255,255,.9);white-space:nowrap}
.chip.gold{background:rgba(200,146,42,.28);border-color:rgba(200,146,42,.5);color:#FFD98A}
/* Stats row */
.stats{display:grid;grid-template-columns:repeat(4,1fr);gap:6px;margin-bottom:10px}
.stat{background:rgba(255,255,255,.12);border-radius:11px;padding:8px 4px;text-align:center;border:1px solid rgba(255,255,255,.16)}
.stat.hi{background:rgba(255,255,255,.22)}
.stat-n{font-size:20px;font-weight:700;color:#fff}
.stat-l{font-size:9px;font-weight:300;color:rgba(255,255,255,.75);margin-top:1px}
/* Progress */
.prog-wrap{background:rgba(255,255,255,.15);border-radius:6px;height:7px;overflow:hidden;margin-bottom:4px}
.prog-fill{height:7px;background:#fff;border-radius:6px;transition:width .6s}
.prog-row{display:flex;justify-content:space-between;margin-bottom:10px}
.prog-txt{font-size:10px;color:rgba(255,255,255,.72)}
/* Admin bar */
.adm-bar{display:flex;align-items:center;justify-content:space-between;background:rgba(0,0,0,.15);padding:8px 16px;border-top:1px solid rgba(255,255,255,.1)}
.adm-info{display:flex;align-items:center;gap:6px;font-size:11px;color:rgba(255,255,255,.85)}
.adm-dot{width:7px;height:7px;border-radius:50%;background:rgba(255,255,255,.3);display:inline-block;transition:background .3s}
.adm-dot.on{background:#5DCAA5}
.adm-btns{display:flex;gap:6px}
.adm-btn{font-size:10px;font-weight:600;padding:4px 11px;border-radius:14px;border:1.5px solid rgba(255,255,255,.35);background:rgba(255,255,255,.15);color:#fff;cursor:pointer;font-family:'Kanit',sans-serif;transition:background .15s}
.adm-btn:hover{background:rgba(255,255,255,.28)}
.adm-badge{font-size:9px;background:var(--gold);color:#fff;padding:2px 6px;border-radius:8px;font-weight:700;margin-left:4px}

/* ─── SYNC BAR ───────────────────────────────── */
.sync-bar{display:flex;align-items:center;justify-content:space-between;background:#fff;padding:7px 14px;border-bottom:1px solid var(--border);font-size:11px;color:var(--text-sub)}
.sync-dot{width:7px;height:7px;border-radius:50%;background:#C8C6BC;display:inline-block;margin-right:5px;transition:background .3s}
.sync-dot.ok{background:var(--teal-bright)}
.sync-dot.spin{background:var(--gold);animation:pulse .8s infinite}
.sync-dot.err{background:var(--red-soft)}
.sync-btn-sm{font-size:10px;font-weight:500;padding:3px 10px;border-radius:10px;border:1.5px solid var(--border);background:var(--teal-pale);color:var(--teal-deep);cursor:pointer;font-family:'Kanit',sans-serif}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.35}}

/* ─── SEARCH ─────────────────────────────────── */
.search-wrap{position:relative;margin:12px 14px 0}
.search-ico{position:absolute;left:13px;top:50%;transform:translateY(-50%);color:var(--teal-mid);font-size:15px;pointer-events:none}
.search-input{width:100%;padding:10px 36px;border:2px solid var(--border);border-radius:22px;font-size:13px;font-family:'Kanit',sans-serif;color:var(--text);background:#fff;box-shadow:var(--shadow)}
.search-input::placeholder{color:#B8B6AE}
.search-input:focus{outline:none;border-color:var(--teal-bright);box-shadow:0 0 0 3px rgba(27,176,135,.12)}
.search-clr{position:absolute;right:13px;top:50%;transform:translateY(-50%);color:#999;cursor:pointer;display:none;font-size:13px}

/* ─── NAV TABS ───────────────────────────────── */
.main-wrap{padding:12px 14px 0}
.section-nav{display:flex;gap:0;background:#fff;border-radius:12px;border:1px solid var(--border);overflow:hidden;margin-bottom:12px;box-shadow:var(--shadow)}
.snav-btn{flex:1;padding:9px 4px;font-size:11px;font-weight:500;font-family:'Kanit',sans-serif;border:none;background:transparent;color:var(--text-sub);cursor:pointer;display:flex;align-items:center;justify-content:center;gap:4px;transition:all .15s;border-right:1px solid var(--border)}
.snav-btn:last-child{border-right:none}
.snav-btn.act{background:var(--teal-deep);color:#fff;font-weight:700}
.snav-btn i{font-size:14px}

/* ─── LEGEND ─────────────────────────────────── */
.legend{display:flex;gap:10px;flex-wrap:wrap;background:#fff;border-radius:12px;padding:8px 12px;margin-bottom:10px;border:1px solid var(--border)}
.leg-item{display:flex;align-items:center;gap:5px;font-size:10px;color:var(--text-sub)}
.leg-dot{width:9px;height:9px;border-radius:50%;flex-shrink:0}

/* ─── DAY NAV ────────────────────────────────── */
.week-label{font-size:10px;font-weight:600;color:var(--teal-deep);background:var(--teal-light);border-radius:7px;padding:3px 9px;margin-bottom:7px;display:inline-block}
.day-nav{display:flex;gap:6px;overflow-x:auto;padding-bottom:5px;margin-bottom:10px;scrollbar-width:none}
.day-nav::-webkit-scrollbar{display:none}
.day-btn{flex-shrink:0;padding:6px 13px;border-radius:18px;font-size:11px;font-weight:500;cursor:pointer;font-family:'Kanit',sans-serif;border:1.5px solid var(--border);background:#fff;color:var(--teal-deep);transition:all .18s;white-space:nowrap}
.day-btn.act{background:linear-gradient(135deg,#0BBFA8,#1AE0BA);border-color:#0BBFA8;color:#033028;font-weight:700;box-shadow:0 3px 10px rgba(11,191,168,.28)}
.day-btn.has{border-color:var(--teal-mid);color:var(--teal-mid)}

/* ─── DAY CARD ───────────────────────────────── */
.day-card{background:#fff;border-radius:var(--radius);border:1px solid var(--border);padding:13px 12px;box-shadow:var(--shadow)}
.day-hdr{font-size:14px;font-weight:600;color:var(--teal-deep);margin-bottom:12px;display:flex;align-items:center;gap:7px}
.day-hdr-line{flex:1;height:1px;background:var(--teal-light)}
.day-hdr-count{font-size:10px;font-weight:500;color:var(--teal-mid);background:var(--teal-light);padding:2px 8px;border-radius:9px}

/* ─── TIME GROUP ─────────────────────────────── */
.tg{margin-bottom:14px}
.tg:last-child{margin-bottom:0}
.tg-head{display:flex;align-items:center;gap:7px;margin-bottom:8px}
.tg-label{font-size:11px;font-weight:600;padding:3px 11px;border-radius:10px;font-family:'Kanit',sans-serif;white-space:nowrap}
.tc0{background:#E1F5EE;color:var(--teal-deep);border:1.5px solid #9FE1CB}
.tc1{background:#EAF3DE;color:#27500A;border:1.5px solid #B8D98C}
.tc2{background:#FEF0DC;color:#6B3A04;border:1.5px solid #F5C47A}
.tc3{background:#FDE8EF;color:#7A1E3E;border:1.5px solid #F0A8C0}
.tg-line{flex:1;height:1px;background:#EEE}
.tg-count{font-size:9px;font-weight:500;color:var(--teal-mid);background:var(--teal-light);padding:2px 7px;border-radius:8px}

/* ─── SLOT GRID ──────────────────────────────── */
.slots{display:grid;grid-template-columns:1fr;gap:8px}
.slot{border-radius:13px;padding:11px 14px 10px;cursor:pointer;min-height:72px;display:flex;flex-direction:column;position:relative;transition:all .15s;border:1.5px solid transparent}
.slot.emp{background:#FAFAF8;border-color:#C8C6BC;border-style:dashed}
.slot.emp:hover{background:#F0F5EC;border-color:#97C459}
.slot.bk-a{background:linear-gradient(150deg,var(--teal-deep),var(--teal-bright));border-color:#5DCAA5;box-shadow:0 3px 11px rgba(6,80,64,.22)}
.slot.bk-b{background:linear-gradient(150deg,#1A4D08,#3B8A14);border-color:#97C459;box-shadow:0 3px 11px rgba(26,77,8,.2)}
.slot.bk-c{background:linear-gradient(150deg,#0A7862,#22B68C);border-color:#5DCAA5;box-shadow:0 4px 13px rgba(10,120,98,.28)}
.slot.rated{outline:2px solid var(--gold);outline-offset:2px}
.slot-dot{position:absolute;top:7px;right:7px;width:6px;height:6px;border-radius:50%;background:rgba(255,255,255,.7)}
.slot-ap{width:20px;height:20px;border-radius:6px;display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:700;flex-shrink:0}
.sa{background:var(--teal-light);color:var(--teal-deep)}
.sb{background:#EAF3DE;color:#27500A}
.sc{background:#FEF0DC;color:#6B3A04}
.slot.bk-a .slot-ap,.slot.bk-b .slot-ap,.slot.bk-c .slot-ap{background:rgba(255,255,255,.22);color:#fff}
.slot-ar{display:flex;align-items:center;gap:5px;margin-bottom:4px}
.slot-name{font-size:11px;font-weight:600;line-height:1.35;flex:1;color:var(--text)}
.slot-name.empty{font-size:10px;font-weight:300;color:#B8B6AE;font-style:italic}
.slot.bk-a .slot-name,.slot.bk-b .slot-name,.slot.bk-c .slot-name{color:#fff}
.slot-ph{font-size:9px;font-weight:300;color:rgba(255,255,255,.55);margin-top:1px;font-style:italic}
.slot-cc{font-size:9px;color:rgba(255,255,255,.7);margin-top:2px;line-height:1.4}
.slot-foot{font-size:8px;font-weight:300;color:#CCC;margin-top:auto;padding-top:3px}
.slot.bk-a .slot-foot,.slot.bk-b .slot-foot,.slot.bk-c .slot-foot{color:rgba(255,255,255,.38)}
.slot-stars{display:flex;gap:2px;margin-top:3px}
.star-tag{font-size:8px;padding:1px 5px;border-radius:5px;background:rgba(255,255,255,.2);color:#fff;font-weight:500}

/* ─── SEARCH RESULTS ─────────────────────────── */
.srw{padding:10px 14px 0}
.src{background:#fff;border-radius:13px;border:1px solid var(--border);padding:12px}
.srl{font-size:12px;color:var(--text-sub);margin-bottom:8px}
.scard{background:var(--teal-pale);border:1.5px solid var(--border);border-radius:10px;padding:11px 12px;margin-bottom:7px;cursor:pointer;transition:background .12s}
.scard:hover{background:var(--teal-light)}
.scn{font-size:13px;font-weight:600;color:var(--teal-deep)}
.scm{font-size:10px;color:var(--teal-mid);margin-top:2px}
.sce{font-size:12px;color:#AAA;padding:18px 0;text-align:center}
.sc-cc{font-size:10px;color:var(--gold);margin-top:2px}
.sc-ph{font-size:10px;color:#AAA;margin-top:2px;font-style:italic}

/* ─── FOOTER INFO ────────────────────────────── */
.footer-info{margin:14px 14px 0;background:#fff;border-radius:var(--radius);border:1px solid var(--border);padding:15px;box-shadow:var(--shadow)}
.fi-head{font-size:13px;font-weight:600;color:var(--teal-deep);margin-bottom:10px;display:flex;align-items:center;gap:6px}
.fi-list{list-style:none;display:flex;flex-direction:column;gap:7px}
.fi-list li{font-size:11px;color:#333;display:flex;align-items:flex-start;gap:7px;line-height:1.6}
.fi-list i{font-size:13px;color:var(--teal-bright);flex-shrink:0;margin-top:2px}
.fi-contact{margin-top:12px;background:linear-gradient(135deg,var(--teal-deep),#0F6E56);border-radius:12px;padding:13px;display:flex;align-items:center;gap:11px}
.fi-ci{width:38px;height:38px;background:rgba(255,255,255,.15);border-radius:10px;display:flex;align-items:center;justify-content:center;flex-shrink:0;border:1px solid rgba(255,255,255,.2)}
.fi-cn{font-size:12px;font-weight:600;color:#fff}
.fi-cr{font-size:10px;font-weight:300;color:rgba(255,255,255,.78);line-height:1.5;margin-top:2px}
.fi-cp{font-size:14px;font-weight:700;color:#5DCAA5;margin-top:4px}

/* ─── MODAL BACKDROP ─────────────────────────── */
.modal-bg{display:none;position:fixed;inset:0;z-index:200;align-items:flex-end;justify-content:center}
.modal-bg.open{display:flex}
.modal-ov{position:absolute;inset:0;background:rgba(0,0,0,.45)}
.modal-box{background:#fff;border-radius:22px 22px 0 0;width:100%;padding:0;position:relative;z-index:1;max-height:96vh;overflow-y:auto;box-shadow:0 -8px 32px rgba(0,0,0,.15)}
/* modal header */
.modal-hdr{background:linear-gradient(135deg,var(--teal-deep),var(--teal-bright));padding:16px 16px 14px;border-radius:22px 22px 0 0;position:sticky;top:0;z-index:10}
.modal-htitle{font-size:15px;font-weight:700;color:#fff;margin-bottom:4px}
.modal-hinfo{font-size:11px;color:rgba(255,255,255,.82);line-height:1.6}
.modal-close{position:absolute;top:14px;right:14px;width:28px;height:28px;background:rgba(255,255,255,.18);border:1.5px solid rgba(255,255,255,.28);border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer;color:#fff;font-size:14px}
/* modal body */
.modal-body{padding:14px 16px 30px}
/* sections */
.msec{margin-bottom:18px}
.msec-title{font-size:12px;font-weight:700;color:var(--teal-deep);margin-bottom:10px;display:flex;align-items:center;gap:6px;padding-bottom:6px;border-bottom:1.5px solid var(--teal-light)}
.msec-title i{font-size:14px;color:var(--teal-bright)}
/* form row */
.frow{margin-bottom:10px}
.frow label{display:block;font-size:11px;font-weight:500;color:var(--text-sub);margin-bottom:4px}
.frow label span.req{color:var(--red-soft)}
.finput{width:100%;padding:10px 12px;border:2px solid var(--border);border-radius:10px;font-size:13px;font-family:'Kanit',sans-serif;color:var(--text);background:#fff;transition:border .15s}
.finput:focus{outline:none;border-color:var(--teal-bright);box-shadow:0 0 0 3px rgba(27,176,135,.1)}
.finput::placeholder{color:#C0BDB4;font-size:12px}
textarea.finput{resize:vertical;min-height:68px}
.fgrid2{display:grid;grid-template-columns:1fr 1fr;gap:9px}
/* radio pills */
.radio-pills{display:flex;gap:6px;flex-wrap:wrap}
.rpill input{display:none}
.rpill label{padding:5px 13px;border-radius:14px;font-size:11px;font-weight:500;font-family:'Kanit',sans-serif;border:1.5px solid var(--border);background:#fff;color:var(--text-sub);cursor:pointer;transition:all .15s;display:inline-block}
.rpill input:checked+label{background:var(--teal-deep);color:#fff;border-color:var(--teal-deep)}
/* alert tag */
.alert-tag{font-size:10px;background:var(--red-light);color:var(--red-soft);border:1px solid #F5C1C1;border-radius:7px;padding:3px 9px;display:inline-block;margin-top:4px}
/* slider */
.slider-wrap{margin-top:2px}
.slider-row{display:flex;justify-content:space-between;align-items:center;margin-bottom:5px}
.slider-label{font-size:11px;color:var(--teal-mid)}
.slider-val{font-size:17px;font-weight:700;color:var(--teal-deep)}
.fslider{width:100%;height:7px;border-radius:4px;background:#C0DD97;outline:none;-webkit-appearance:none;cursor:pointer}
.fslider::-webkit-slider-thumb{-webkit-appearance:none;width:22px;height:22px;border-radius:50%;background:linear-gradient(135deg,var(--teal-deep),var(--teal-bright));border:3px solid #fff;box-shadow:0 2px 6px rgba(6,80,64,.25);cursor:pointer}
.fslider::-moz-range-thumb{width:22px;height:22px;border-radius:50%;background:linear-gradient(135deg,var(--teal-deep),var(--teal-bright));border:3px solid #fff;cursor:pointer}
.slider-hints{display:flex;justify-content:space-between;font-size:9px;color:#999;margin-top:3px}
/* action buttons */
.modal-actions{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:14px}
.btn-cancel{padding:12px;border-radius:11px;border:2px solid var(--border);background:#fff;font-size:13px;font-weight:500;cursor:pointer;color:#444;font-family:'Kanit',sans-serif}
.btn-confirm{padding:12px;border-radius:11px;border:none;background:linear-gradient(135deg,var(--teal-deep),var(--teal-bright));font-size:13px;font-weight:700;cursor:pointer;color:#fff;font-family:'Kanit',sans-serif;box-shadow:0 3px 10px rgba(6,80,64,.28)}
.btn-delete{width:100%;margin-top:8px;padding:11px;border-radius:11px;border:1.5px solid #F5C1C1;background:#FEF0F0;font-size:12px;font-weight:500;cursor:pointer;color:#A32D2D;font-family:'Kanit',sans-serif}
.btn-full{width:100%;padding:12px;border-radius:11px;border:none;background:linear-gradient(135deg,var(--teal-deep),var(--teal-bright));font-size:13px;font-weight:700;cursor:pointer;color:#fff;font-family:'Kanit',sans-serif;margin-top:4px}
/* info card in modal */
.info-card{background:var(--teal-pale);border:1px solid var(--teal-light);border-radius:10px;padding:11px 13px;margin-bottom:14px;font-size:11px;color:#27500A;line-height:1.8}
.info-card b{color:var(--teal-deep)}
/* record display */
.rec-block{background:var(--teal-pale);border:1px solid var(--teal-light);border-radius:10px;padding:11px 13px;margin-bottom:8px}
.rec-block-title{font-size:10px;font-weight:700;color:var(--teal-deep);margin-bottom:7px;display:flex;align-items:center;gap:5px}
.rec-row{display:flex;margin-bottom:4px}
.rec-lbl{font-size:10px;color:var(--text-sub);min-width:90px;flex-shrink:0}
.rec-val{font-size:11px;font-weight:500;color:var(--text)}
.rec-val.alert{color:var(--red-soft)}
.rec-hidden{font-size:10px;color:#AAA;font-style:italic}
/* admin login modal */
.admlog-bg{display:none;position:fixed;inset:0;z-index:400;align-items:center;justify-content:center;background:rgba(0,0,0,.55);backdrop-filter:blur(5px)}
.admlog-bg.open{display:flex}
.admlog-box{background:#fff;border-radius:18px;width:88%;max-width:310px;padding:22px 18px;box-shadow:0 14px 44px rgba(0,0,0,.22);position:relative;z-index:1}
.admlog-ico{width:46px;height:46px;background:linear-gradient(135deg,var(--teal-deep),var(--teal-bright));border-radius:13px;display:flex;align-items:center;justify-content:center;margin:0 auto 12px}
.admlog-h{font-size:15px;font-weight:700;color:var(--teal-deep);text-align:center;margin-bottom:3px}
.admlog-s{font-size:11px;color:var(--text-sub);text-align:center;margin-bottom:16px}
.admlog-err{font-size:11px;color:var(--red-soft);margin-bottom:8px;display:none;text-align:center}
/* toast */
.toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%) translateY(18px);background:var(--teal-deep);color:#fff;padding:11px 22px;border-radius:28px;font-size:12px;font-weight:500;opacity:0;transition:all .25s;pointer-events:none;white-space:nowrap;z-index:500;font-family:'Kanit',sans-serif;box-shadow:0 4px 16px rgba(6,80,64,.38)}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0)}
/* highlight */
.slot.hl{outline:3px solid var(--gold);outline-offset:2px}
/* phone hidden badge */
.ph-hidden{font-size:9px;color:rgba(255,255,255,.45);margin-top:1px;font-style:italic}

/* ─── CALENDAR VIEW ──────────────────────────── */
.cal-month-nav{display:flex;align-items:center;justify-content:space-between;margin-bottom:10px}
.cal-month-title{font-size:15px;font-weight:700;color:var(--teal-deep)}
.cal-nav-btn{width:32px;height:32px;border-radius:50%;border:1.5px solid var(--border);background:#fff;color:var(--teal-deep);cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:14px}
.cal-nav-btn:hover{background:var(--teal-light)}
.cal-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:4px}
.cal-dow{text-align:center;font-size:10px;font-weight:700;color:var(--text-sub);padding:4px 0}
.cal-dow.wknd{color:#C0B0A0}
.cal-cell{border-radius:10px;min-height:64px;padding:5px 4px 4px;cursor:pointer;border:1.5px solid transparent;transition:all .18s;display:flex;flex-direction:column;position:relative;box-sizing:border-box}
.cal-cell.empty{background:transparent;border:none;cursor:default;min-height:64px}
.cal-cell.off-day{background:#F6F4F1;border-color:#EAE8E4;cursor:default}
.cal-cell.work{background:#fff;border-color:var(--border)}
.cal-cell.work:hover{border-color:var(--teal-mid);background:var(--teal-pale);transform:translateY(-1px);box-shadow:0 3px 10px rgba(6,80,64,.1)}
.cal-cell.full{background:linear-gradient(135deg,var(--teal-deep),var(--teal-bright));border-color:var(--teal-mid)}
.cal-cell.full:hover{box-shadow:0 4px 14px rgba(6,80,64,.3);transform:translateY(-1px)}
.cal-cell.partial{background:#fff;border-color:var(--teal-bright)}
.cal-date{font-size:13px;font-weight:700;color:var(--text);margin-bottom:3px;line-height:1}
.cal-cell.full .cal-date{color:#fff}
.cal-cell.off-day .cal-date{color:#C0B8B0}
.cal-status{font-size:9px;font-weight:600;border-radius:6px;padding:2px 5px;text-align:center;margin-top:auto;line-height:1.3}
.status-full{background:rgba(255,255,255,.22);color:#fff}
.status-partial{background:var(--teal-light);color:var(--teal-deep)}
.status-free{background:#EFF8E8;color:#27500A}
.status-off{font-size:9px;color:#C8C0B8;text-align:center;margin-top:auto}
.cal-pip{display:flex;gap:2px;flex-wrap:wrap;margin-top:2px}
.cal-pip-dot{width:5px;height:5px;border-radius:50%;background:var(--teal-bright)}
.cal-cell.full .cal-pip-dot{background:rgba(255,255,255,.7)}
.cal-legend{display:flex;gap:8px;flex-wrap:wrap;background:#fff;border-radius:10px;padding:8px 11px;margin-bottom:10px;border:1px solid var(--border)}
.cal-leg-item{display:flex;align-items:center;gap:5px;font-size:10px;color:var(--text-sub)}
.cal-leg-box{width:12px;height:12px;border-radius:4px;flex-shrink:0}
/* Day detail modal */
.daydet-bg{display:none;position:fixed;inset:0;z-index:250;align-items:flex-end;justify-content:center}
.daydet-bg.open{display:flex}
.daydet-ov{position:absolute;inset:0;background:rgba(0,0,0,.42)}
.daydet-box{background:#fff;border-radius:22px 22px 0 0;width:100%;max-height:90vh;overflow-y:auto;position:relative;z-index:1;box-shadow:0 -8px 32px rgba(0,0,0,.15)}
.daydet-hdr{background:linear-gradient(135deg,var(--teal-deep),var(--teal-bright));padding:15px 16px 13px;border-radius:22px 22px 0 0;position:sticky;top:0;z-index:10}
.daydet-title{font-size:15px;font-weight:700;color:#fff}
.daydet-sub{font-size:11px;color:rgba(255,255,255,.8);margin-top:3px}
.daydet-close{position:absolute;top:13px;right:14px;width:28px;height:28px;background:rgba(255,255,255,.18);border:1.5px solid rgba(255,255,255,.28);border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer;color:#fff;font-size:14px}
.daydet-body{padding:14px 16px 28px}
.time-row{display:flex;align-items:center;gap:10px;padding:10px 12px;border-radius:11px;margin-bottom:7px;cursor:pointer;transition:all .15s;border:1.5px solid var(--border);background:#fff}
.time-row:hover{background:var(--teal-pale);border-color:var(--teal-mid)}
.time-row.booked{background:linear-gradient(135deg,var(--teal-deep),var(--teal-bright));border-color:var(--teal-mid)}
.time-row.booked:hover{opacity:.88}
.tr-time{font-size:11px;font-weight:700;color:var(--teal-deep);min-width:88px;flex-shrink:0;line-height:1.3}
.time-row.booked .tr-time{color:#fff}
.tr-info{flex:1;min-width:0}
.tr-name{font-size:12px;font-weight:500;color:var(--text)}
.time-row.booked .tr-name{color:#fff}
.tr-cc{font-size:10px;color:var(--teal-mid);margin-top:1px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.time-row.booked .tr-cc{color:rgba(255,255,255,.75)}
.tr-badge-book{font-size:9px;padding:2px 7px;border-radius:7px;background:rgba(255,255,255,.2);color:#fff;font-weight:600;flex-shrink:0;white-space:nowrap}
.tr-badge-free{font-size:9px;padding:2px 7px;border-radius:7px;background:#EFF8E8;color:#1E6B14;font-weight:600;flex-shrink:0;white-space:nowrap;border:1px solid #B8DDB0}
.tr-arrow{color:#CCC;flex-shrink:0;font-size:13px}
.time-row.booked .tr-arrow{color:rgba(255,255,255,.55)}
</style>
</head>
<body>

<!-- ═══ TOP BAR ═══════════════════════════════════════════ -->
<div class="topbar">
  <div class="tb-inner">
    <div class="tb-brand">
      <div class="tb-ico"><i class="ti ti-stethoscope" style="font-size:22px;color:#fff"></i></div>
      <div>
        <div class="tb-title">ระบบนัดหมาย & บันทึกการรักษา</div>
        <div class="tb-sub">นักศึกษาฝึกประสบการณ์ หลักสูตรนวดไทย 800 ชั่วโมง · สสจ.สมุทรปราการ</div>
      </div>
    </div>
    <div class="tb-chips">
      <div class="chip"><i class="ti ti-calendar" style="font-size:11px"></i> 1–30 ก.ย. 2569</div>
      <div class="chip"><i class="ti ti-clock" style="font-size:11px"></i> วันทำการ จ–ศ</div>
      <div class="chip gold"><i class="ti ti-certificate" style="font-size:11px"></i> ภายใต้การดูแลแพทย์แผนไทย</div>
    </div>
    <div class="stats">
      <div class="stat hi"><div class="stat-n" id="stB">0</div><div class="stat-l">นัดหมาย</div></div>
      <div class="stat"><div class="stat-n" id="stF">0</div><div class="stat-l">ว่างอยู่</div></div>
      <div class="stat"><div class="stat-n" id="stP">0%</div><div class="stat-l">อัตราการใช้</div></div>
      <div class="stat"><div class="stat-n" id="stA">–</div><div class="stat-l">avg ฝีมือ</div></div>
    </div>
    <div class="prog-wrap"><div class="prog-fill" id="progFill" style="width:0%"></div></div>
    <div class="prog-row">
      <span class="prog-txt" id="progTxt">กำลังโหลด...</span>
      <span class="prog-txt">เขียว = มีการนัดหมาย</span>
    </div>
  </div>
  <div class="adm-bar">
    <div class="adm-info">
      <span class="adm-dot" id="admDot"></span>
      <span id="admTxt">ผู้ใช้ทั่วไป</span>
    </div>
    <div class="adm-btns">
      <button class="adm-btn" id="admLoginBtn" onclick="openAdminLogin()">🔐 Admin</button>
    </div>
  </div>
</div>

<!-- ═══ SYNC BAR ══════════════════════════════════════════ -->
<div class="sync-bar">
  <div><span class="sync-dot" id="syncDot"></span><span id="syncTxt">ยังไม่ได้โหลดข้อมูล</span></div>
  <button class="sync-btn-sm" onclick="loadSheets()">🔄 โหลดใหม่</button>
</div>

<!-- ═══ SEARCH ════════════════════════════════════════════ -->
<div class="search-wrap">
  <i class="ti ti-search search-ico"></i>
  <input class="search-input" id="searchInput" type="text" placeholder="ค้นหาชื่อผู้รับบริการ, อาการ, โรคประจำตัว...">
  <span class="search-clr" id="searchClr"><i class="ti ti-x"></i></span>
</div>
<div id="searchResults" style="display:none" class="srw"></div>

<!-- ═══ MAIN ══════════════════════════════════════════════ -->
<div class="main-wrap" id="mainWrap">

  <!-- Section Nav -->
  <div class="section-nav" style="margin-top:12px">
    <button class="snav-btn act" id="navSchedule" onclick="switchView('schedule')">
      <i class="ti ti-calendar-event"></i> ตาราง
    </button>
    <button class="snav-btn" id="navCalendar" onclick="switchView('calendar')">
      <i class="ti ti-calendar-month"></i> ปฏิทิน
    </button>
    <button class="snav-btn" id="navRecords" onclick="switchView('records')">
      <i class="ti ti-clipboard-list"></i> บันทึก
    </button>
    <button class="snav-btn" id="navStats" onclick="switchView('stats')">
      <i class="ti ti-chart-bar"></i> สถิติ
    </button>
  </div>

  <!-- ── VIEW: SCHEDULE ── -->
  <div id="viewSchedule">
    <div class="legend">
      <div class="leg-item"><div class="leg-dot" style="background:linear-gradient(135deg,var(--teal-deep),var(--teal-bright))"></div>มีการนัดหมาย</div>
      <div class="leg-item"><div class="leg-dot" style="background:#FAFAF8;border:1.5px dashed #C8C6BC"></div>ว่าง</div>
    </div>
    <div class="day-nav" id="dayNav"></div>
    <div id="dayCard"></div>
  </div>

  <!-- ── VIEW: RECORDS ── -->
  <div id="viewRecords" style="display:none">
    <div id="recordsList"></div>
  </div>

  <!-- ── VIEW: STATS ── -->
  <div id="viewStats" style="display:none">
    <div id="statsPanel"></div>
  </div>

  <!-- ── VIEW: CALENDAR ── -->
  <div id="viewCalendar" style="display:none">
    <div class="cal-legend">
      <div class="cal-leg-item"><div class="cal-leg-box" style="background:linear-gradient(135deg,var(--teal-deep),var(--teal-bright))"></div>เต็มทุกช่วง</div>
      <div class="cal-leg-item"><div class="cal-leg-box" style="background:#fff;border:2px solid var(--teal-bright)"></div>มีว่างบางช่วง</div>
      <div class="cal-leg-item"><div class="cal-leg-box" style="background:#fff;border:1.5px solid var(--border)"></div>ว่างทุกช่วง</div>
      <div class="cal-leg-item"><div class="cal-leg-box" style="background:#F6F4F1;border:1px solid #EAE8E4"></div>ไม่มีบริการ</div>
    </div>
    <div id="calGrid"></div>
  </div>

</div>

<!-- ═══ FOOTER INFO ═══════════════════════════════════════ -->
<div class="footer-info">
  <div class="fi-head"><i class="ti ti-info-circle" style="font-size:16px;color:var(--teal-bright)"></i>ข้อควรทราบก่อนเข้ารับบริการ</div>
  <ul class="fi-list">
    <li><i class="ti ti-shirt"></i><span>แต่งกายสุภาพ สวมเสื้อผ้าสะดวก เหมาะแก่การรับบริการนวด</span></li>
    <li><i class="ti ti-clock"></i><span>กรุณามาก่อนเวลานัด 15 นาที เพื่อลงทะเบียนและซักประวัติ</span></li>
    <li><i class="ti ti-salad"></i><span>หลีกเลี่ยงรับประทานอาหารหนักก่อนรับบริการ 1 ชั่วโมง</span></li>
    <li><i class="ti ti-heart-rate-monitor"></i><span>แจ้งข้อมูลสุขภาพ โรคประจำตัว และอาการเจ็บป่วยทุกครั้ง</span></li>
    <li><i class="ti ti-stethoscope"></i><span>การรักษาอยู่ภายใต้การควบคุมของแพทย์แผนไทยที่มีใบอนุญาต</span></li>
    <li><i class="ti ti-alert-triangle" style="color:var(--gold)"></i><span>นักศึกษาฝึกประสบการณ์ปีการศึกษา 2569 ชั่วโมงปฏิบัติ 800 ชม.</span></li>
    <li><i class="ti ti-map-pin"></i><span>คลินิกการแพทย์แผนไทยฯ สสจ.สมุทรปราการ</span></li>
  </ul>
  <div class="fi-contact">
    <div class="fi-ci"><i class="ti ti-phone" style="font-size:18px;color:#9FE1CB"></i></div>
    <div>
      <div class="fi-cn">คุณมิ้น — ผู้ประสานงาน</div>
      <div class="fi-cr">กลุ่มงานการแพทย์แผนไทยและการแพทย์ทางเลือก<br>สำนักงานสาธารณสุขจังหวัดสมุทรปราการ</div>
      <div class="fi-cp">📞 02-389-5980 ต่อ 402</div>
    </div>
  </div>
</div>

<!-- ═══ BOOKING / EDIT MODAL ══════════════════════════════ -->
<div class="modal-bg" id="bookingModal">
  <div class="modal-ov" id="bookingOv"></div>
  <div class="modal-box">
    <div class="modal-hdr">
      <div class="modal-htitle" id="modalTitle">จองนัดหมาย</div>
      <div class="modal-hinfo" id="modalInfo"></div>
      <div class="modal-close" onclick="closeModal()"><i class="ti ti-x"></i></div>
    </div>
    <div class="modal-body">
      <div class="info-card" id="modalInfoCard"></div>

      <!-- ─── BOOKING FORM ─── -->
      <div id="formBook">
        <!-- ส่วนที่ 1: ข้อมูลผู้รับบริการ -->
        <div class="msec">
          <div class="msec-title"><i class="ti ti-user"></i>ข้อมูลผู้รับบริการ</div>
          <div class="frow">
            <label>ชื่อ-นามสกุล <span class="req">*</span></label>
            <input class="finput" id="bName" type="text" placeholder="กรอกชื่อ-นามสกุล">
          </div>
          <div class="fgrid2">
            <div class="frow">
              <label>อายุ</label>
              <input class="finput" id="bAge" type="number" placeholder="ปี">
            </div>
            <div class="frow">
              <label>เพศ</label>
              <div class="radio-pills" style="margin-top:4px">
                <div class="rpill"><input type="radio" name="bGender" id="bGM" value="ชาย"><label for="bGM">ชาย</label></div>
                <div class="rpill"><input type="radio" name="bGender" id="bGF" value="หญิง"><label for="bGF">หญิง</label></div>
                <div class="rpill"><input type="radio" name="bGender" id="bGO" value="อื่นๆ"><label for="bGO">อื่นๆ</label></div>
              </div>
            </div>
          </div>
          <div class="frow">
            <label>เบอร์โทรศัพท์</label>
            <input class="finput" id="bPhone" type="tel" placeholder="0XX-XXX-XXXX">
          </div>
        </div>

        <!-- ส่วนที่ 2: อาการและประวัติ -->
        <div class="msec">
          <div class="msec-title"><i class="ti ti-heartbeat"></i>อาการและประวัติการเจ็บป่วย</div>
          <div class="frow">
            <label>อาการที่มาพบ / อาการสำคัญ <span class="req">*</span></label>
            <textarea class="finput" id="bChiefCC" placeholder="เช่น ปวดหลังเรื้อรัง, ปวดคอ บ่า ไหล่, ชาปลายมือ..."></textarea>
          </div>
          <div class="frow">
            <label>ระยะเวลาที่มีอาการ</label>
            <input class="finput" id="bDuration" type="text" placeholder="เช่น 2 สัปดาห์, 3 เดือน, 1 ปี">
          </div>
          <div class="frow">
            <label>โรคประจำตัว</label>
            <textarea class="finput" id="bChronic" placeholder="เช่น เบาหวาน, ความดันโลหิตสูง, ไทรอยด์ (ถ้าไม่มี ระบุ '-')"></textarea>
          </div>
          <div class="frow">
            <label>ประวัติการผ่าตัด</label>
            <textarea class="finput" id="bSurgery" placeholder="เช่น ผ่าตัดข้อเข่าซ้าย ปี 2565 (ถ้าไม่มี ระบุ '-')"></textarea>
          </div>
          <div class="frow">
            <label>ประวัติการแพ้ยา</label>
            <input class="finput" id="bDrugAllergy" type="text" placeholder="ระบุชื่อยาที่แพ้ หรือ 'ไม่มี'">
          </div>
          <div class="frow">
            <label>ประวัติการแพ้อาหาร</label>
            <input class="finput" id="bFoodAllergy" type="text" placeholder="ระบุอาหารที่แพ้ หรือ 'ไม่มี'">
          </div>
        </div>

        <!-- ส่วนที่ 3: ให้คะแนน (ไม่บังคับ) -->
        <div class="msec">
          <div class="msec-title"><i class="ti ti-star"></i>ให้คะแนนหลังรับบริการ <span style="font-weight:300;font-size:10px">(กรอกทีหลังได้)</span></div>
          <div class="slider-wrap">
            <div class="slider-row"><span class="slider-label">✋ ฝีมือการนวด</span><span class="slider-val" id="bSkV">5</span></div>
            <input type="range" min="1" max="10" value="5" class="fslider" id="bSkS">
            <div class="slider-hints"><span>1 ปรับปรุง</span><span>5 ปานกลาง</span><span>10 ยอดเยี่ยม</span></div>
          </div>
          <div class="slider-wrap" style="margin-top:10px">
            <div class="slider-row"><span class="slider-label">⚖️ น้ำหนักมือ</span><span class="slider-val" id="bWtV">5</span></div>
            <input type="range" min="1" max="10" value="5" class="fslider" id="bWtS">
            <div class="slider-hints"><span>1 เบามาก</span><span>5 กลางๆ</span><span>10 หนักมาก</span></div>
          </div>
          <div style="font-size:9px;color:#AAA;margin-top:6px;font-style:italic">* คะแนนใช้เพื่อประเมินนักศึกษาและพัฒนาคุณภาพบริการ</div>
        </div>

        <div class="modal-actions">
          <button class="btn-cancel" onclick="closeModal()">ยกเลิก</button>
          <button class="btn-confirm" onclick="saveBooking()">✅ ยืนยันนัดหมาย</button>
        </div>
      </div><!-- /formBook -->

      <!-- ─── EDIT/VIEW FORM ─── -->
      <div id="formEdit" style="display:none">
        <!-- Record view -->
        <div id="recView"></div>

        <!-- Edit fields (Admin only) -->
        <div id="editFields" style="display:none">
          <div class="msec">
            <div class="msec-title"><i class="ti ti-edit"></i>แก้ไขข้อมูล</div>
            <div class="frow"><label>ชื่อ-นามสกุล</label><input class="finput" id="eN" type="text"></div>
            <div class="fgrid2">
              <div class="frow"><label>อายุ</label><input class="finput" id="eAge" type="number"></div>
              <div class="frow"><label>เบอร์โทร (Admin)</label><input class="finput" id="eP" type="tel"></div>
            </div>
            <div class="frow"><label>อาการสำคัญ</label><textarea class="finput" id="eCC"></textarea></div>
            <div class="frow"><label>ระยะเวลาที่มีอาการ</label><input class="finput" id="eDur" type="text"></div>
            <div class="frow"><label>โรคประจำตัว</label><textarea class="finput" id="eChr"></textarea></div>
            <div class="frow"><label>ประวัติการผ่าตัด</label><textarea class="finput" id="eSrg"></textarea></div>
            <div class="frow"><label>ประวัติการแพ้ยา</label><input class="finput" id="eDA" type="text"></div>
            <div class="frow"><label>ประวัติการแพ้อาหาร</label><input class="finput" id="eFA" type="text"></div>
          </div>
          <div class="msec">
            <div class="msec-title"><i class="ti ti-star"></i>คะแนนความพึงพอใจ</div>
            <div class="slider-wrap">
              <div class="slider-row"><span class="slider-label">✋ ฝีมือ</span><span class="slider-val" id="eSkV">5</span></div>
              <input type="range" min="1" max="10" value="5" class="fslider" id="eSkS">
              <div class="slider-hints"><span>1 ปรับปรุง</span><span>5 ปานกลาง</span><span>10 ยอดเยี่ยม</span></div>
            </div>
            <div class="slider-wrap" style="margin-top:10px">
              <div class="slider-row"><span class="slider-label">⚖️ น้ำหนักมือ</span><span class="slider-val" id="eWtV">5</span></div>
              <input type="range" min="1" max="10" value="5" class="fslider" id="eWtS">
              <div class="slider-hints"><span>1 เบามาก</span><span>5 กลางๆ</span><span>10 หนักมาก</span></div>
            </div>
          </div>
          <div class="modal-actions">
            <button class="btn-cancel" onclick="closeModal()">ยกเลิก</button>
            <button class="btn-confirm" onclick="saveEdit()">💾 บันทึก</button>
          </div>
          <button class="btn-delete" onclick="deleteBooking()">🗑 ลบการนัดหมายนี้</button>
        </div>

        <!-- Non-admin: read-only + close -->
        <div id="editReadOnly">
          <button class="btn-full" onclick="closeModal()">ปิด</button>
        </div>
      </div><!-- /formEdit -->

    </div><!-- /modal-body -->
  </div>
</div>

<!-- ═══ DAY DETAIL MODAL ════════════════════════════════════ -->
<div class="daydet-bg" id="daydetBg">
  <div class="daydet-ov" id="daydetOv" onclick="closeDayDetail()"></div>
  <div class="daydet-box">
    <div class="daydet-hdr">
      <div class="daydet-title" id="daydetTitle">รายละเอียดวันที่</div>
      <div class="daydet-sub" id="daydetSub"></div>
      <div class="daydet-close" onclick="closeDayDetail()"><i class="ti ti-x"></i></div>
    </div>
    <div class="daydet-body" id="daydetBody"></div>
  </div>
</div>

<!-- ═══ ADMIN LOGIN MODAL ══════════════════════════════════ -->
<div class="admlog-bg" id="admlogBg">
  <div class="modal-ov" onclick="closeAdminLogin()"></div>
  <div class="admlog-box">
    <div class="admlog-ico"><i class="ti ti-lock" style="font-size:20px;color:#fff"></i></div>
    <div class="admlog-h">Admin เข้าสู่ระบบ</div>
    <div class="admlog-s">กรอกรหัสผ่านเพื่อดูข้อมูลส่วนตัวและแก้ไขการนัดหมาย</div>
    <div class="frow"><label>รหัสผ่าน</label><input class="finput" type="password" id="admPw" placeholder="••••••" onkeydown="if(event.key==='Enter')doLogin()"></div>
    <div class="admlog-err" id="admErr">รหัสผ่านไม่ถูกต้อง</div>
    <button class="btn-confirm" style="width:100%;margin-top:4px" onclick="doLogin()">🔓 เข้าสู่ระบบ</button>
    <button class="btn-cancel" style="width:100%;margin-top:8px" onclick="closeAdminLogin()">ยกเลิก</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<!-- ═══ SCRIPT ════════════════════════════════════════════ -->
<script>
// ═══════════════════════════════════════════════════════
// CONFIG
// ═══════════════════════════════════════════════════════
const SHEET_ID  = '11m5e8eedws48FopO8fX-payl4ZqYhgP5xE0GCiixpHM';
const SHEET_NAME= 'Booking';
let adminToken = 'Ttm12345';

// ═══════════════════════════════════════════════════════
// STATE
// ═══════════════════════════════════════════════════════
let isAdmin = false;
let currentView = 'schedule';
let activeDay   = '';
let _d, _t, _s; // modal context

// ═══════════════════════════════════════════════════════
// DAYS: วันทำการ จ–ศ เดือน ก.ย. 2569
// ═══════════════════════════════════════════════════════
const DY = [
  {l:'1 ก.ย.',  k:'d0901', w:'สัปดาห์ที่ 1'},
  {l:'2 ก.ย.',  k:'d0902', w:'สัปดาห์ที่ 1'},
  {l:'3 ก.ย.',  k:'d0903', w:'สัปดาห์ที่ 1'},
  {l:'4 ก.ย.',  k:'d0904', w:'สัปดาห์ที่ 1'},
  {l:'5 ก.ย.',  k:'d0905', w:'สัปดาห์ที่ 1'},
  {l:'8 ก.ย.',  k:'d0908', w:'สัปดาห์ที่ 2'},
  {l:'9 ก.ย.',  k:'d0909', w:'สัปดาห์ที่ 2'},
  {l:'10 ก.ย.', k:'d0910', w:'สัปดาห์ที่ 2'},
  {l:'11 ก.ย.', k:'d0911', w:'สัปดาห์ที่ 2'},
  {l:'12 ก.ย.', k:'d0912', w:'สัปดาห์ที่ 2'},
  {l:'15 ก.ย.', k:'d0915', w:'สัปดาห์ที่ 3'},
  {l:'16 ก.ย.', k:'d0916', w:'สัปดาห์ที่ 3'},
  {l:'17 ก.ย.', k:'d0917', w:'สัปดาห์ที่ 3'},
  {l:'18 ก.ย.', k:'d0918', w:'สัปดาห์ที่ 3'},
  {l:'19 ก.ย.', k:'d0919', w:'สัปดาห์ที่ 3'},
  {l:'22 ก.ย.', k:'d0922', w:'สัปดาห์ที่ 4'},
  {l:'23 ก.ย.', k:'d0923', w:'สัปดาห์ที่ 4'},
  {l:'24 ก.ย.', k:'d0924', w:'สัปดาห์ที่ 4'},
  {l:'25 ก.ย.', k:'d0925', w:'สัปดาห์ที่ 4'},
  {l:'26 ก.ย.', k:'d0926', w:'สัปดาห์ที่ 4'},
  {l:'29 ก.ย.', k:'d0929', w:'สัปดาห์ที่ 5'},
  {l:'30 ก.ย.', k:'d0930', w:'สัปดาห์ที่ 5'},
];

const TM = [
  {k:'t900', l:'09:00–10:00 น.', c:'tc0', b:'bk-a'},
  {k:'t1000',l:'10:00–11:00 น.', c:'tc1', b:'bk-b'},
  {k:'t1100',l:'11:00–12:00 น.', c:'tc2', b:'bk-a'},
  {k:'t1300',l:'13:00–14:00 น.', c:'tc3', b:'bk-b'},
  {k:'t1400',l:'14:00–15:00 น.', c:'tc0', b:'bk-a'},
  {k:'t1500',l:'15:00–16:00 น.', c:'tc1', b:'bk-b'},
];

const SL = ['A'];
const PC = ['sa'];
const BC = ['bk-a'];

// ═══════════════════════════════════════════════════════
// DATA MODEL
// ═══════════════════════════════════════════════════════
function emptyRecord(){
  return {
    n:'', age:'', gender:'', phone:'',
    chiefCC:'', duration:'', chronic:'', surgery:'',
    drugAllergy:'', foodAllergy:'',
    sk:0, wt:0
  };
}
function emptySlots(){
  return {A:emptyRecord()};
}

// Build DB
const DB = {};
DY.forEach(d=>{
  DB[d.k]={t900:emptySlots(),t1000:emptySlots(),t1100:emptySlots(),t1300:emptySlots(),t1400:emptySlots(),t1500:emptySlots()};
});
let db = JSON.parse(JSON.stringify(DB));
activeDay = DY[0].k;

// ═══════════════════════════════════════════════════════
// GOOGLE APPS SCRIPT BACKEND
// ═══════════════════════════════════════════════════════
function loadSheets(){
  setSyncState('spin','กำลังโหลดข้อมูลจาก Google Sheets...');
  google.script.run
    .withSuccessHandler(function(rows){
      db=JSON.parse(JSON.stringify(DB));
      (rows||[]).forEach(function(x){
        const dk=mapDate(x.date), tk=mapTime(x.time);
        if(dk&&tk&&db[dk]&&db[dk][tk]){
          db[dk][tk]['A']={
            n:String(x.name||''), age:String(x.age||''), gender:String(x.gender||''),
            phone:String(x.phone||''), chiefCC:String(x.chiefCC||''), duration:String(x.duration||''),
            chronic:String(x.chronic||''), surgery:String(x.surgery||''),
            drugAllergy:String(x.drugAllergy||''), foodAllergy:String(x.foodAllergy||''),
            sk:Number(x.sk)||0, wt:Number(x.wt)||0
          };
        }
      });
      setSyncState('ok','โหลดสำเร็จ '+new Date().toLocaleTimeString('th-TH')+' ('+(rows||[]).length+' รายการ)');
      renderAll();
    })
    .withFailureHandler(function(err){
      setSyncState('err','โหลดข้อมูลไม่สำเร็จ');
      toast('⚠️ '+(err&&err.message?err.message:'ไม่สามารถเชื่อมต่อ Google Sheets'));
      renderAll();
    })
    .getBookings();
}
function mapDate(s){
  const all={'ม.ค.':'01','ก.พ.':'02','มี.ค.':'03','เม.ย.':'04','พ.ค.':'05','มิ.ย.':'06','ก.ค.':'07','ส.ค.':'08','ก.ย.':'09','ต.ค.':'10','พ.ย.':'11','ธ.ค.':'12'};
  s=String(s||'').trim();
  for(const[mn,mv]of Object.entries(all)){
    if(s.includes(mn)){
      const d=s.replace(mn,'').replace(/\d{4}/,'').trim().padStart(2,'0');
      return 'd'+mv+d;
    }
  }
  const slsh=s.match(/^(\d{1,2})[\/\-](\d{1,2})/);
  if(slsh)return 'd'+slsh[2].padStart(2,'0')+slsh[1].padStart(2,'0');
  const iso=s.match(/(\d{4})[\/\-](\d{2})[\/\-](\d{2})/);
  if(iso)return 'd'+iso[2]+iso[3];
  return null;
}
function mapTime(s){
  s=String(s||'').replace(/[^\d:]/g,'').replace(':','');
  const m={'0900':'t900','900':'t900','1000':'t1000','1100':'t1100','1300':'t1300','1400':'t1400','1500':'t1500'};
  return m[s]||null;
}
function setSyncState(state,txt){
  const dot=document.getElementById('syncDot');
  dot.className='sync-dot'+(state==='ok'?' ok':state==='spin'?' spin':state==='err'?' err':'');
  document.getElementById('syncTxt').textContent=txt;
}

// ═══════════════════════════════════════════════════════
// STATS
// ═══════════════════════════════════════════════════════
function getStats(){
  let b=0,tot=0,ss=0,sc=0;
  DY.forEach(d=>TM.forEach(t=>{
    tot++;
    const r=db[d.k]&&db[d.k][t.k]&&db[d.k][t.k]['A'];
    if(r&&r.n.trim()){b++;if(r.sk>0){ss+=r.sk;sc++;}}
  }));
  return{b,tot,f:tot-b,avg:sc>0?(ss/sc).toFixed(1):'–'};
}
function updateTopStats(){
  const{b,tot,f,avg}=getStats();
  const p=tot?Math.round(b/tot*100):0;
  document.getElementById('stB').textContent=b;
  document.getElementById('stF').textContent=f;
  document.getElementById('stP').textContent=p+'%';
  document.getElementById('stA').textContent=avg;
  document.getElementById('progFill').style.width=p+'%';
  document.getElementById('progTxt').textContent=b+' จาก '+tot+' slot (22 วันทำการ · 6 ช่วงเวลา)';
}
function countDay(k){let b=0;TM.forEach(t=>{if(db[k]&&db[k][t.k]&&db[k][t.k]['A']&&db[k][t.k]['A'].n.trim())b++;});return b;}
function countTime(dk,tk){return(db[dk]&&db[dk][tk]&&db[dk][tk]['A']&&db[dk][tk]['A'].n.trim())?1:0;}
function tLabel(k){return{t900:'09:00–10:00',t1000:'10:00–11:00',t1100:'11:00–12:00',t1300:'13:00–14:00',t1400:'14:00–15:00',t1500:'15:00–16:00'}[k]+' น.';}

// ═══════════════════════════════════════════════════════
// RENDER ALL
// ═══════════════════════════════════════════════════════
function renderAll(){
  updateTopStats();
  if(currentView==='schedule') renderSchedule();
  else if(currentView==='calendar') renderCalendar();
  else if(currentView==='records') renderRecords();
  else if(currentView==='stats') renderStats();
}

// ── SCHEDULE VIEW ────────────────────────────────────
function renderSchedule(){
  const nav=document.getElementById('dayNav');
  nav.innerHTML='';
  DY.forEach(d=>{
    const cnt=countDay(d.k);
    const btn=document.createElement('button');
    btn.className='day-btn'+(d.k===activeDay?' act':cnt>0?' has':'');
    btn.innerHTML=d.l+(cnt?` <span style="font-size:9px;opacity:.75">(${cnt})</span>`:'');
    btn.onclick=()=>{activeDay=d.k;renderSchedule();};
    nav.appendChild(btn);
  });
  const con=document.getElementById('dayCard');
  con.innerHTML='';
  const cur=DY.find(d=>d.k===activeDay);
  const wl=document.createElement('div');wl.className='week-label';
  wl.textContent='📅 '+cur.w+' · กันยายน 2569';
  con.appendChild(wl);
  const card=document.createElement('div');card.className='day-card';
  const dh=document.createElement('div');dh.className='day-hdr';
  dh.innerHTML=`<i class="ti ti-calendar-event" style="font-size:14px;color:var(--teal-bright)"></i>วันที่ ${cur.l} 2569<div class="day-hdr-line"></div><span class="day-hdr-count">${countDay(activeDay)}/6 นัด</span>`;
  card.appendChild(dh);
  TM.forEach(t=>{
    const tg=document.createElement('div');tg.className='tg';
    const th=document.createElement('div');th.className='tg-head';
    th.innerHTML=`<span class="tg-label ${t.c}">${t.l}</span><div class="tg-line"></div><span class="tg-count">${countTime(activeDay,t.k)?'จองแล้ว':'ว่าง'}</span>`;
    tg.appendChild(th);
    const gr=document.createElement('div');gr.className='slots';
    SL.forEach((s,si)=>{
      const r=db[activeDay][t.k][s];
      const bk=r.n.trim()!=='';
      const hasRating=bk&&r.sk>0;
      const cls='slot '+(bk?(hasRating?BC[si]:t.b):'emp')+(hasRating?' rated':'');
      const c=document.createElement('div');c.className=cls;
      let inner='';
      if(bk){
        inner+=`<div class="slot-dot"></div>`;
        inner+=`<div class="slot-ar"><div class="slot-ap ${PC[si]}">${s}</div></div>`;
        inner+=`<div class="slot-name">${r.n}</div>`;
        if(r.chiefCC) inner+=`<div class="slot-cc">${r.chiefCC.substring(0,22)}${r.chiefCC.length>22?'…':''}</div>`;
        // เบอร์โทร: Admin เท่านั้น
        if(r.phone){
          inner+=isAdmin
            ?`<div class="slot-ph">📞 ${r.phone}</div>`
            :`<div class="slot-ph">📞 ••• (Admin)</div>`;
        }
        if(hasRating) inner+=`<div class="slot-stars"><span class="star-tag">✋${r.sk}</span><span class="star-tag">⚖️${r.wt}</span></div>`;
      }else{
        inner+=`<div class="slot-ar"><div class="slot-ap ${PC[si]}">${s}</div></div>`;
        inner+=`<div class="slot-name empty">ว่าง – คลิกเพื่อจองนัดหมาย</div>`;
      }
      inner+=`<div class="slot-foot">นักศึกษาฝึกประสบการณ์</div>`;
      c.innerHTML=inner;
      c.onclick=()=>openModal(activeDay,t.k,s,bk);
      gr.appendChild(c);
    });
    tg.appendChild(gr);card.appendChild(tg);
  });
  con.appendChild(card);
}

// ── RECORDS VIEW ─────────────────────────────────────
function renderRecords(){
  const con=document.getElementById('recordsList');
  con.innerHTML='';
  const all=[];
  DY.forEach(d=>TM.forEach(t=>{
    const r=db[d.k]&&db[d.k][t.k]&&db[d.k][t.k]['A'];
    if(r&&r.n.trim()) all.push({d,t,s:'A',r});
  }));
  if(!all.length){
    con.innerHTML='<div class="src" style="margin:0;text-align:center;padding:30px 0;background:#fff;border-radius:var(--radius);border:1px solid var(--border)"><i class="ti ti-notes-off" style="font-size:32px;color:#CCC"></i><div style="color:#AAA;font-size:13px;margin-top:8px">ยังไม่มีการนัดหมาย</div></div>';
    return;
  }
  const wl=document.createElement('div');wl.className='week-label';wl.textContent=`📋 บันทึกการรักษา ${all.length} รายการ`;con.appendChild(wl);
  all.forEach(({d,t,s,r})=>{
    const card=document.createElement('div');
    card.className='scard';
    const hasAlert=[r.chronic,r.surgery,r.drugAllergy,r.foodAllergy].filter(x=>x&&x!=='-'&&x!=='ไม่มี').length>0;
    card.innerHTML=`
      <div class="scn">${r.n}${r.age?` (${r.age} ปี)`:''}</div>
      <div class="scm">📅 ${d.l} 2569 · ${tLabel(t.k)} · นักศึกษา ${s}</div>
      ${r.chiefCC?`<div class="sc-cc">🩺 ${r.chiefCC}${r.duration?' ('+r.duration+')':''}</div>`:''}
      ${hasAlert?`<div style="margin-top:4px"><span class="alert-tag">⚠️ มีประวัติสุขภาพสำคัญ</span></div>`:''}
      ${r.phone?(isAdmin?`<div class="scm">📞 ${r.phone}</div>`:`<div class="sc-ph">📞 ••• (Admin เท่านั้น)</div>`):''}
      ${r.sk>0?`<div class="sc-cc">✋ ฝีมือ ${r.sk}/10 · ⚖️ น้ำหนัก ${r.wt}/10</div>`:''}
    `;
    card.onclick=()=>openModal(d.k,t.k,s,true);
    con.appendChild(card);
  });
}

// ── STATS VIEW ───────────────────────────────────────
function renderStats(){
  const con=document.getElementById('statsPanel');
  con.innerHTML='';
  const{b,tot,f,avg}=getStats();
  const p=tot?Math.round(b/tot*100):0;
  // Collect clinical data
  let totalCC=0,hasAllergy=0,hasChronic=0,hasSurgery=0;
  const ccList={},gList={'ชาย':0,'หญิง':0,'อื่นๆ':0};
  DY.forEach(d=>TM.forEach(t=>{
    const r=db[d.k]&&db[d.k][t.k]&&db[d.k][t.k]['A'];
    if(!r||!r.n.trim()) return;
    totalCC++;
    if(r.chronic&&r.chronic!=='-'&&r.chronic!=='ไม่มี') hasChronic++;
    if(r.surgery&&r.surgery!=='-'&&r.surgery!=='ไม่มี') hasSurgery++;
    if((r.drugAllergy&&r.drugAllergy!=='ไม่มี')||(r.foodAllergy&&r.foodAllergy!=='ไม่มี')) hasAllergy++;
    if(r.gender&&gList[r.gender]!==undefined) gList[r.gender]++;
    if(r.chiefCC){const cc=r.chiefCC.split(/[,\/]/)[0].trim().substring(0,20);ccList[cc]=(ccList[cc]||0)+1;}
  }));
  const topCC=Object.entries(ccList).sort((a,b)=>b[1]-a[1]).slice(0,5);
  const wl=document.createElement('div');wl.className='week-label';wl.textContent='📊 สถิติภาพรวม';con.appendChild(wl);
  const card=document.createElement('div');card.className='day-card';
  let html=`
    <div class="msec-title" style="font-size:13px;margin-bottom:12px"><i class="ti ti-chart-pie" style="font-size:14px"></i>สรุปการใช้บริการ</div>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:14px">
      <div style="background:var(--teal-pale);border-radius:10px;padding:11px;text-align:center;border:1px solid var(--teal-light)">
        <div style="font-size:26px;font-weight:700;color:var(--teal-deep)">${b}</div>
        <div style="font-size:10px;color:var(--text-sub)">นัดหมายทั้งหมด</div>
      </div>
      <div style="background:var(--teal-pale);border-radius:10px;padding:11px;text-align:center;border:1px solid var(--teal-light)">
        <div style="font-size:26px;font-weight:700;color:var(--teal-deep)">${p}%</div>
        <div style="font-size:10px;color:var(--text-sub)">อัตราการใช้ slot</div>
      </div>
      <div style="background:var(--gold-light);border-radius:10px;padding:11px;text-align:center;border:1px solid #F5D98A">
        <div style="font-size:26px;font-weight:700;color:var(--gold)">${avg}</div>
        <div style="font-size:10px;color:var(--text-sub)">avg คะแนนฝีมือ</div>
      </div>
      <div style="background:#FEF0F0;border-radius:10px;padding:11px;text-align:center;border:1px solid #F5C1C1">
        <div style="font-size:26px;font-weight:700;color:var(--red-soft)">${hasAllergy}</div>
        <div style="font-size:10px;color:var(--text-sub)">ผู้มีประวัติแพ้</div>
      </div>
    </div>
    <div class="msec-title" style="font-size:11px;margin-bottom:8px"><i class="ti ti-heartbeat"></i>ข้อมูลสุขภาพ</div>
    <div style="display:flex;flex-direction:column;gap:6px;margin-bottom:14px">
      ${renderStatBar('โรคประจำตัว',hasChronic,totalCC,'var(--gold)')}
      ${renderStatBar('ประวัติการผ่าตัด',hasSurgery,totalCC,'var(--teal-mid)')}
      ${renderStatBar('ประวัติการแพ้',hasAllergy,totalCC,'var(--red-soft)')}
    </div>
    <div class="msec-title" style="font-size:11px;margin-bottom:8px"><i class="ti ti-gender-bigender"></i>เพศผู้รับบริการ</div>
    <div style="display:flex;gap:6px;margin-bottom:14px;flex-wrap:wrap">
      ${Object.entries(gList).filter(([,v])=>v>0).map(([k,v])=>`<div style="background:var(--teal-pale);border:1px solid var(--teal-light);border-radius:8px;padding:7px 12px;font-size:11px"><b>${k}</b> ${v} คน</div>`).join('')}
    </div>`;
  if(topCC.length){
    html+=`<div class="msec-title" style="font-size:11px;margin-bottom:8px"><i class="ti ti-list"></i>อาการที่พบบ่อย</div><div style="display:flex;flex-direction:column;gap:5px">`;
    topCC.forEach(([cc,cnt])=>{
      html+=`<div style="display:flex;align-items:center;justify-content:space-between;background:var(--teal-pale);border-radius:8px;padding:7px 11px;border:1px solid var(--teal-light)"><span style="font-size:11px">${cc}</span><span style="font-size:11px;font-weight:700;color:var(--teal-deep)">${cnt} ราย</span></div>`;
    });
    html+=`</div>`;
  }
  card.innerHTML=html;
  con.appendChild(card);
}
function renderStatBar(label,val,tot,color){
  const p=tot?Math.round(val/tot*100):0;
  return`<div><div style="display:flex;justify-content:space-between;font-size:10px;color:var(--text-sub);margin-bottom:3px"><span>${label}</span><span>${val}/${tot} (${p}%)</span></div><div style="background:#EEE;border-radius:4px;height:6px;overflow:hidden"><div style="height:6px;width:${p}%;background:${color};border-radius:4px;transition:width .6s"></div></div></div>`;
}

// ═══════════════════════════════════════════════════════
// VIEW SWITCHER
// ═══════════════════════════════════════════════════════

// ═══════════════════════════════════════════════════════
// CALENDAR VIEW
// ═══════════════════════════════════════════════════════
// September 2569: starts Monday (day index in week: Mon=0)
// Sep 1 = Monday → offset 0
function renderCalendar(){
  const con = document.getElementById('calGrid');
  con.innerHTML = '';

  // Header: month nav (static Sep 2569)
  const nav = document.createElement('div');
  nav.className = 'cal-month-nav';
  nav.innerHTML = `<div class="cal-month-title">📅 กันยายน 2569</div>`;
  con.appendChild(nav);

  // Day-of-week headers
  const grid = document.createElement('div');
  grid.className = 'cal-grid';
  const dows = ['จ','อ','พ','พฤ','ศ','ส','อ'];
  const dowClass = ['','','','','','wknd','wknd'];
  dows.forEach((d,i) => {
    const h = document.createElement('div');
    h.className = 'cal-dow ' + (dowClass[i] || '');
    h.textContent = d;
    grid.appendChild(h);
  });

  // Sep 1 2569 = Monday (offset 0, Mon=col0)
  // Sep has 30 days
  const totalDays = 30;
  const startOffset = 0; // Monday

  // Build all work-day keys
  const workKeys = {};
  DY.forEach(d => { workKeys[d.k] = d; });

  // Fill empty cells before day 1 (none for Sep since starts Monday)
  for(let i = 0; i < startOffset; i++){
    const e = document.createElement('div');
    e.className = 'cal-cell empty';
    grid.appendChild(e);
  }

  for(let day = 1; day <= totalDays; day++){
    const mm = '09';
    const dd = String(day).padStart(2,'0');
    const key = 'd' + mm + dd;
    const col = (startOffset + day - 1) % 7; // 0=Mon .. 6=Sun
    const isWeekend = (col === 5 || col === 6);
    const isWork = workKeys[key] !== undefined;

    const cell = document.createElement('div');

    if(isWeekend || !isWork){
      cell.className = 'cal-cell off-day';
      cell.innerHTML = `<div class="cal-date">${day}</div><div class="status-off">${isWeekend ? 'หยุด' : '–'}</div>`;
    } else {
      // Count booked slots
      const total = TM.length; // 6
      let booked = 0;
      TM.forEach(t => {
        const r = db[key] && db[key][t.k] && db[key][t.k]['A'];
        if(r && r.n.trim()) booked++;
      });
      const free = total - booked;
      const isFull = free === 0;
      const isEmpty = booked === 0;

      cell.className = 'cal-cell ' + (isFull ? 'full' : (booked > 0 ? 'partial' : 'work'));

      let statusHtml = '';
      let pipHtml = '';
      if(isFull){
        statusHtml = `<div class="cal-status status-full">เต็ม</div>`;
      } else if(booked > 0){
        // show pip dots for booked slots
        pipHtml = `<div class="cal-pip">${TM.map(t => {
          const r2 = db[key] && db[key][t.k] && db[key][t.k]['A'];
          return `<div class="cal-pip-dot" style="${r2&&r2.n.trim()?'':'background:#C8DDD0;opacity:.5'}"></div>`;
        }).join('')}</div>`;
        statusHtml = `<div class="cal-status status-partial">ว่าง ${free} ช่วง</div>`;
      } else {
        statusHtml = `<div class="cal-status status-free">ว่าง</div>`;
      }

      cell.innerHTML = `<div class="cal-date">${day}</div>${pipHtml}${statusHtml}`;
      cell.onclick = () => openDayDetail(key);
    }
    grid.appendChild(cell);
  }

  // Fill remaining cells to complete last row
  const totalCells = startOffset + totalDays;
  const remainder = totalCells % 7;
  if(remainder !== 0){
    for(let i = 0; i < 7 - remainder; i++){
      const e = document.createElement('div');
      e.className = 'cal-cell empty';
      grid.appendChild(e);
    }
  }

  con.appendChild(grid);
}

// ─── DAY DETAIL MODAL ────────────────────────────────
function openDayDetail(dk){
  const day = DY.find(d => d.k === dk);
  if(!day) return;

  const total = TM.length;
  let booked = 0;
  TM.forEach(t => {
    const r = db[dk] && db[dk][t.k] && db[dk][t.k]['A'];
    if(r && r.n.trim()) booked++;
  });
  const free = total - booked;

  document.getElementById('daydetTitle').textContent = 'วันที่ ' + day.l + ' 2569';
  document.getElementById('daydetSub').innerHTML =
    `จอง <b>${booked}</b> ช่วง · ว่าง <b style="color:#9FE1CB">${free}</b> ช่วง จากทั้งหมด ${total} ช่วงเวลา`;

  const body = document.getElementById('daydetBody');
  body.innerHTML = '';

  TM.forEach(t => {
    const r = db[dk] && db[dk][t.k] && db[dk][t.k]['A'];
    const isBooked = r && r.n.trim();

    const row = document.createElement('div');
    row.className = 'time-row ' + (isBooked ? 'booked' : '');

    if(isBooked){
      const hasAlert = [r.chronic,r.surgery,r.drugAllergy,r.foodAllergy]
        .filter(x => x && x !== '-' && x !== 'ไม่มี').length > 0;
      row.innerHTML = `
        <div class="tr-time">${t.l}</div>
        <div class="tr-info">
          <div class="tr-name">${r.n}</div>
          ${r.chiefCC ? `<div class="tr-cc">🩺 ${r.chiefCC.substring(0,28)}${r.chiefCC.length>28?'…':''}</div>` : ''}
          ${hasAlert ? `<div class="tr-cc">⚠️ มีประวัติสุขภาพสำคัญ</div>` : ''}
        </div>
        <span class="tr-badge-book">จอง</span>
        <i class="ti ti-chevron-right tr-arrow"></i>`;
      row.onclick = () => { closeDayDetail(); openModal(dk, t.k, 'A', true); };
    } else {
      row.innerHTML = `
        <div class="tr-time">${t.l}</div>
        <div class="tr-info">
          <div class="tr-name" style="color:var(--text-sub)">ว่าง – คลิกเพื่อจองนัดหมาย</div>
        </div>
        <span class="tr-badge-free">ว่าง</span>
        <i class="ti ti-chevron-right tr-arrow"></i>`;
      row.onclick = () => { closeDayDetail(); openModal(dk, t.k, 'A', false); };
    }
    body.appendChild(row);
  });

  document.getElementById('daydetBg').classList.add('open');
}

function closeDayDetail(){
  document.getElementById('daydetBg').classList.remove('open');
}

function switchView(v){
  currentView=v;
  document.getElementById('viewSchedule').style.display=v==='schedule'?'block':'none';
  document.getElementById('viewCalendar').style.display=v==='calendar'?'block':'none';
  document.getElementById('viewRecords').style.display=v==='records'?'block':'none';
  document.getElementById('viewStats').style.display=v==='stats'?'block':'none';
  ['Schedule','Calendar','Records','Stats'].forEach(n=>{
    document.getElementById('nav'+n).classList.toggle('act',n.toLowerCase()===v);
  });
  renderAll();
}

// ═══════════════════════════════════════════════════════
// MODAL
// ═══════════════════════════════════════════════════════
function openModal(day,time,slot,booked){
  _d=day;_t=time;_s=slot;
  const cur=DY.find(d=>d.k===day);
  const r=db[day][time][slot];
  document.getElementById('modalTitle').textContent=booked?'บันทึกการรักษา':'จองนัดหมายใหม่';
  document.getElementById('modalInfo').innerHTML=`<b>วันที่:</b> ${cur.l} 2569 &nbsp;|&nbsp; <b>เวลา:</b> ${tLabel(time)}<br><b>นักศึกษา:</b> ${slot}`;
  document.getElementById('modalInfoCard').innerHTML=`<b>วันที่:</b> ${cur.l} 2569 &nbsp;|&nbsp; <b>เวลา:</b> ${tLabel(time)} &nbsp;|&nbsp; <b>นักศึกษา:</b> ${slot}`;

  if(booked){
    document.getElementById('formBook').style.display='none';
    document.getElementById('formEdit').style.display='block';
    buildRecordView(r);
    if(isAdmin){
      document.getElementById('editFields').style.display='block';
      document.getElementById('editReadOnly').style.display='none';
      // fill fields
      document.getElementById('eN').value=r.n;
      document.getElementById('eAge').value=r.age||'';
      document.getElementById('eP').value=r.phone||'';
      document.getElementById('eCC').value=r.chiefCC||'';
      document.getElementById('eDur').value=r.duration||'';
      document.getElementById('eChr').value=r.chronic||'';
      document.getElementById('eSrg').value=r.surgery||'';
      document.getElementById('eDA').value=r.drugAllergy||'';
      document.getElementById('eFA').value=r.foodAllergy||'';
      document.getElementById('eSkS').value=r.sk||5;document.getElementById('eSkV').textContent=r.sk||5;
      document.getElementById('eWtS').value=r.wt||5;document.getElementById('eWtV').textContent=r.wt||5;
    }else{
      document.getElementById('editFields').style.display='none';
      document.getElementById('editReadOnly').style.display='block';
    }
  }else{
    document.getElementById('formBook').style.display='block';
    document.getElementById('formEdit').style.display='none';
    // reset
    document.getElementById('bName').value='';
    document.getElementById('bAge').value='';
    document.getElementById('bPhone').value='';
    document.getElementById('bChiefCC').value='';
    document.getElementById('bDuration').value='';
    document.getElementById('bChronic').value='';
    document.getElementById('bSurgery').value='';
    document.getElementById('bDrugAllergy').value='';
    document.getElementById('bFoodAllergy').value='';
    document.querySelectorAll('input[name=bGender]').forEach(r=>r.checked=false);
    document.getElementById('bSkS').value=5;document.getElementById('bSkV').textContent=5;
    document.getElementById('bWtS').value=5;document.getElementById('bWtV').textContent=5;
  }
  document.getElementById('bookingModal').classList.add('open');
}

function buildRecordView(r){
  const div=document.getElementById('recView');
  const mask=(v)=>isAdmin?v:(v?'ซ่อน (Admin เท่านั้น)':'–');
  const hasAlert=[r.chronic,r.surgery,r.drugAllergy,r.foodAllergy].filter(x=>x&&x!=='-'&&x!=='ไม่มี').length>0;
  div.innerHTML=`
    <div class="rec-block">
      <div class="rec-block-title"><i class="ti ti-user" style="font-size:12px"></i>ข้อมูลผู้รับบริการ</div>
      <div class="rec-row"><span class="rec-lbl">ชื่อ-นามสกุล</span><span class="rec-val">${r.n}</span></div>
      ${r.age?`<div class="rec-row"><span class="rec-lbl">อายุ</span><span class="rec-val">${r.age} ปี</span></div>`:''}
      ${r.gender?`<div class="rec-row"><span class="rec-lbl">เพศ</span><span class="rec-val">${r.gender}</span></div>`:''}
      <div class="rec-row"><span class="rec-lbl">เบอร์โทร</span><span class="${isAdmin?'rec-val':'rec-hidden'}">${r.phone?mask(r.phone):'–'}</span></div>
    </div>
    <div class="rec-block">
      <div class="rec-block-title"><i class="ti ti-heartbeat" style="font-size:12px"></i>อาการและประวัติ ${hasAlert?'<span class="alert-tag">⚠️ มีประวัติสุขภาพสำคัญ</span>':''}</div>
      <div class="rec-row"><span class="rec-lbl">อาการสำคัญ</span><span class="rec-val">${r.chiefCC||'–'}</span></div>
      <div class="rec-row"><span class="rec-lbl">ระยะเวลา</span><span class="rec-val">${r.duration||'–'}</span></div>
      <div class="rec-row"><span class="rec-lbl">โรคประจำตัว</span><span class="rec-val${r.chronic&&r.chronic!=='-'&&r.chronic!=='ไม่มี'?' alert':''}">${r.chronic||'–'}</span></div>
      <div class="rec-row"><span class="rec-lbl">ประวัติผ่าตัด</span><span class="rec-val${r.surgery&&r.surgery!=='-'&&r.surgery!=='ไม่มี'?' alert':''}">${r.surgery||'–'}</span></div>
      <div class="rec-row"><span class="rec-lbl">แพ้ยา</span><span class="rec-val${r.drugAllergy&&r.drugAllergy!=='ไม่มี'?' alert':''}">${r.drugAllergy||'–'}</span></div>
      <div class="rec-row"><span class="rec-lbl">แพ้อาหาร</span><span class="rec-val${r.foodAllergy&&r.foodAllergy!=='ไม่มี'?' alert':''}">${r.foodAllergy||'–'}</span></div>
    </div>
    ${r.sk>0?`<div class="rec-block"><div class="rec-block-title"><i class="ti ti-star" style="font-size:12px"></i>คะแนนความพึงพอใจ</div><div class="rec-row"><span class="rec-lbl">ฝีมือการนวด</span><span class="rec-val">${r.sk}/10</span></div><div class="rec-row"><span class="rec-lbl">น้ำหนักมือ</span><span class="rec-val">${r.wt}/10</span></div></div>`:''}
  `;
}

function closeModal(){document.getElementById('bookingModal').classList.remove('open');}

function saveBooking(){
  const n=document.getElementById('bName').value.trim();
  if(!n){document.getElementById('bName').focus();toast('⚠️ กรุณาระบุชื่อ-นามสกุล');return;}
  const gender=document.querySelector('input[name=bGender]:checked');
  const rec={
    date:DY.find(d=>d.k===_d).l+' 2569', time:tLabel(_t), slot:_s,
    name:n, age:document.getElementById('bAge').value.trim(),
    gender:gender?gender.value:'',
    phone:document.getElementById('bPhone').value.trim(),
    chiefCC:document.getElementById('bChiefCC').value.trim(),
    duration:document.getElementById('bDuration').value.trim(),
    chronic:document.getElementById('bChronic').value.trim(),
    surgery:document.getElementById('bSurgery').value.trim(),
    drugAllergy:document.getElementById('bDrugAllergy').value.trim(),
    foodAllergy:document.getElementById('bFoodAllergy').value.trim(),
    sk:parseInt(document.getElementById('bSkS').value)||0,
    wt:parseInt(document.getElementById('bWtS').value)||0
  };
  google.script.run
    .withSuccessHandler(function(result){
      if(!result||!result.ok){toast('⚠️ '+(result&&result.message||'บันทึกไม่สำเร็จ'));return;}
      closeModal();loadSheets();toast('✅ บันทึกการนัดหมายเรียบร้อย: '+n);
    })
    .withFailureHandler(function(err){toast('⚠️ '+(err&&err.message?err.message:'บันทึกไม่สำเร็จ'));})
    .createBooking(rec);
}
function saveEdit(){
  const n=document.getElementById('eN').value.trim();
  if(!n){document.getElementById('eN').focus();return;}
  const old=db[_d][_t][_s];
  const rec={
    date:DY.find(d=>d.k===_d).l+' 2569', time:tLabel(_t), slot:_s,
    name:n, age:document.getElementById('eAge').value.trim(), gender:old.gender,
    phone:document.getElementById('eP').value.trim(),
    chiefCC:document.getElementById('eCC').value.trim(),
    duration:document.getElementById('eDur').value.trim(),
    chronic:document.getElementById('eChr').value.trim(),
    surgery:document.getElementById('eSrg').value.trim(),
    drugAllergy:document.getElementById('eDA').value.trim(),
    foodAllergy:document.getElementById('eFA').value.trim(),
    sk:parseInt(document.getElementById('eSkS').value)||0,
    wt:parseInt(document.getElementById('eWtS').value)||0
  };
  google.script.run
    .withSuccessHandler(function(result){
      if(!result||!result.ok){toast('⚠️ '+(result&&result.message||'บันทึกไม่สำเร็จ'));return;}
      closeModal();loadSheets();toast('💾 บันทึกข้อมูลเรียบร้อย');
    })
    .withFailureHandler(function(err){toast('⚠️ '+(err&&err.message?err.message:'บันทึกไม่สำเร็จ'));})
    .updateBooking(adminToken,rec);
}
function deleteBooking(){
  if(!confirm('ยืนยันการลบการนัดหมายนี้หรือไม่?')) return;
  const rec={date:DY.find(d=>d.k===_d).l+' 2569',time:tLabel(_t),slot:_s};
  google.script.run
    .withSuccessHandler(function(result){
      if(!result||!result.ok){toast('⚠️ '+(result&&result.message||'ลบไม่สำเร็จ'));return;}
      closeModal();loadSheets();toast('🗑 ลบการนัดหมายเรียบร้อย');
    })
    .withFailureHandler(function(err){toast('⚠️ '+(err&&err.message?err.message:'ลบไม่สำเร็จ'));})
    .deleteBooking(adminToken,rec);
}

// ═══════════════════════════════════════════════════════
// ADMIN
// ═══════════════════════════════════════════════════════
function openAdminLogin(){
  if(isAdmin){logoutAdmin();return;}
  document.getElementById('admlogBg').classList.add('open');
  document.getElementById('admPw').value='';
  document.getElementById('admErr').style.display='none';
  setTimeout(()=>document.getElementById('admPw').focus(),100);
}
function closeAdminLogin(){document.getElementById('admlogBg').classList.remove('open');}
function doLogin(){
  const pw=document.getElementById('admPw').value;
  google.script.run
    .withSuccessHandler(function(result){
      if(result&&result.ok){
        adminToken=result.token||'';
        isAdmin=true;closeAdminLogin();updateAdminBar();renderAll();toast('🔓 เข้าสู่ระบบ Admin สำเร็จ');loadSheets();
      }else{
        document.getElementById('admErr').style.display='block';
        document.getElementById('admPw').select();
      }
    })
    .withFailureHandler(function(){
      document.getElementById('admErr').style.display='block';
    })
    .loginAdmin(pw);
}
function logoutAdmin(){
  adminToken='';
  isAdmin=false;updateAdminBar();renderAll();toast('🔒 ออกจากระบบ Admin แล้ว');
}
function updateAdminBar(){
  const dot=document.getElementById('admDot');
  const txt=document.getElementById('admTxt');
  const btn=document.getElementById('admLoginBtn');
  if(isAdmin){
    dot.className='adm-dot on';
    txt.innerHTML='Admin <span class="adm-badge">🔓</span>';
    btn.textContent='🔒 ออกจากระบบ';
  }else{
    dot.className='adm-dot';
    txt.textContent='ผู้ใช้ทั่วไป';
    btn.textContent='🔐 Admin';
  }
}

// ═══════════════════════════════════════════════════════
// SEARCH
// ═══════════════════════════════════════════════════════
function doSearch(q){
  const sr=document.getElementById('searchResults');
  const mw=document.getElementById('mainWrap');
  if(!q.trim()){sr.style.display='none';mw.style.display='block';return;}
  mw.style.display='none';sr.style.display='block';
  const kw=q.toLowerCase(),res=[];
  DY.forEach(d=>TM.forEach(t=>{
    const r=db[d.k]&&db[d.k][t.k]&&db[d.k][t.k]['A'];
    if(!r||!r.n) return;
    const haystack=[r.n,r.chiefCC,r.chronic,r.surgery,r.drugAllergy,r.foodAllergy].join(' ').toLowerCase();
    if(haystack.includes(kw)) res.push({d,t,s:'A',r});
  }));
  let h=`<div class="src"><div class="srl">พบ <b>${res.length}</b> รายการ สำหรับ "<b>${q}</b>"</div>`;
  if(!res.length) h+=`<div class="sce">🔍 ไม่พบข้อมูลที่ค้นหา</div>`;
  else res.forEach(({d,t,s,r})=>{
    const hasAlert=[r.chronic,r.surgery,r.drugAllergy,r.foodAllergy].filter(x=>x&&x!=='-'&&x!=='ไม่มี').length>0;
    h+=`<div class="scard" onclick="jumpTo('${d.k}','${t.k}','${s}')">
      <div class="scn">${r.n}${r.age?` (${r.age} ปี)`:''}</div>
      <div class="scm">📅 ${d.l} 2569 · ${tLabel(t.k)} · นักศึกษา ${s}</div>
      ${r.chiefCC?`<div class="sc-cc">🩺 ${r.chiefCC}${r.duration?' ('+r.duration+')':''}</div>`:''}
      ${hasAlert?`<span class="alert-tag">⚠️ ประวัติสุขภาพสำคัญ</span>`:''}
      ${r.phone?(isAdmin?`<div class="scm" style="margin-top:3px">📞 ${r.phone}</div>`:`<div class="sc-ph">📞 ••• (Admin เท่านั้น)</div>`):''}
    </div>`;
  });
  sr.innerHTML=h+'</div>';
}
function jumpTo(dk,tk,s){
  document.getElementById('searchInput').value='';
  document.getElementById('searchClr').style.display='none';
  doSearch('');
  activeDay=dk;
  switchView('schedule');
  setTimeout(()=>{
    const slots=document.querySelectorAll('.slot');
    let i=0;
    TM.forEach(t=>{
      if(t.k===tk&&slots[i]){
        slots[i].classList.add('hl');
        slots[i].scrollIntoView({behavior:'smooth',block:'center'});
        setTimeout(()=>slots[i].classList.remove('hl'),2500);
      }
      i++;
    });
  },100);
}

// ═══════════════════════════════════════════════════════
// SLIDERS
// ═══════════════════════════════════════════════════════
document.getElementById('bSkS').oninput=function(){document.getElementById('bSkV').textContent=this.value;};
document.getElementById('bWtS').oninput=function(){document.getElementById('bWtV').textContent=this.value;};
document.getElementById('eSkS').oninput=function(){document.getElementById('eSkV').textContent=this.value;};
document.getElementById('eWtS').oninput=function(){document.getElementById('eWtV').textContent=this.value;};

// Modal overlay close
document.getElementById('bookingOv').onclick=closeModal;

// Search
document.getElementById('searchInput').oninput=function(){
  const v=this.value;
  document.getElementById('searchClr').style.display=v?'block':'none';
  doSearch(v);
};
document.getElementById('searchClr').onclick=function(){
  document.getElementById('searchInput').value='';
  this.style.display='none';
  doSearch('');
};

// Toast
function toast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg;t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),2600);
}

// ═══════════════════════════════════════════════════════
// INIT
// ═══════════════════════════════════════════════════════
updateAdminBar();
renderAll();
loadSheets();
</script>
</body>
</html>
