<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Sopaan · Torque and Rotational Dynamics</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,700&family=Source+Sans+3:wght@400;600;700&family=IBM+Plex+Mono:wght@500;600&display=swap">
<style>
  html,body{margin:0;} img{max-width:100%;} [hidden]{display:none !important;}
  :root{
    --navy:#16264A; --navy-2:#1F3B6B; --gold:#C79A3E; --gold-soft:#F3E7C9;
    --paper:#FBF9F4; --paper-2:#F1EAD8; --card:#FFFFFF;
    --ink:#211E1A; --ink-soft:#655D4C; --rule:#E4DCC8;
    --success:#2E8B57; --success-soft:#E1F2E7;
    --danger:#BF4B45; --danger-soft:#FAE3E1;
    --locked:#C7BEA9;
    --focus:#1F3B6B;
    --accent-text:#1F3B6B; --retry-text:#8A661F;
    color-scheme:light;
  }
  @media (prefers-color-scheme: dark){
    :root:not([data-theme="light"]){
      --navy:#0E1830; --navy-2:#16264A; --gold:#E0B75B; --gold-soft:#3A2F16;
      --paper:#141210; --paper-2:#1C1914; --card:#1C1914;
      --ink:#EDE7DA; --ink-soft:#B2A996; --rule:#3A342A;
      --success:#7BC79A; --success-soft:#1B2E22;
      --danger:#E38884; --danger-soft:#331D1B;
      --locked:#4A4436;
      --focus:#E0B75B;
      --accent-text:#E0B75B; --retry-text:#E0B75B;
      color-scheme:dark;
    }
  }
  :root[data-theme="dark"]{
    --navy:#0E1830; --navy-2:#16264A; --gold:#E0B75B; --gold-soft:#3A2F16;
    --paper:#141210; --paper-2:#1C1914; --card:#1C1914;
    --ink:#EDE7DA; --ink-soft:#B2A996; --rule:#3A342A;
    --success:#7BC79A; --success-soft:#1B2E22;
    --danger:#E38884; --danger-soft:#331D1B;
    --locked:#4A4436;
    --focus:#E0B75B;
    --accent-text:#E0B75B; --retry-text:#E0B75B;
    color-scheme:dark;
  }

  *{box-sizing:border-box;}
  body{background:var(--paper); color:var(--ink); font-family:'Source Sans 3',-apple-system,'Segoe UI',sans-serif; padding-inline:0; padding-block:0;}
  h1,h2,h3{font-family:'Fraunces',Georgia,serif; text-wrap:balance; margin:0;}
  .num{font-family:'IBM Plex Mono',ui-monospace,monospace; font-variant-numeric:tabular-nums;}

  header.brand{
    background:linear-gradient(155deg,var(--navy) 0%, var(--navy-2) 100%);
    color:#F4EFDF; padding-block:calc(20px + env(safe-area-inset-top,0px)) 18px; padding-inline:20px;
  }
  .brand-row{display:flex; align-items:center; gap:12px; max-width:900px; margin:0 auto;}
  .crest{
    width:42px; height:42px; border-radius:10px; background:var(--gold); color:var(--navy);
    display:flex; align-items:center; justify-content:center; font-family:'Fraunces',serif; font-weight:700; font-size:18px; flex-shrink:0;
  }
  .brand-name{font-size:12px; letter-spacing:.08em; text-transform:uppercase; color:var(--gold); font-weight:700;}
  .series-name{font-size:19px; font-weight:700; font-family:'Fraunces',serif;}
  .chapter-eyebrow{max-width:900px; margin:14px auto 0; font-size:12px; letter-spacing:.06em; text-transform:uppercase; color:#AEB9D6;}
  .chapter-title{font-size:28px; max-width:900px; margin:2px auto 0;}
  .chapter-sub{font-size:14.5px; color:var(--gold); font-weight:700; max-width:900px; margin:3px auto 0;}

  .chapter-progress{max-width:900px; margin:14px auto 0; display:flex; align-items:center; gap:10px;}
  .cp-track{flex:1; height:6px; border-radius:99px; background:rgba(255,255,255,.18); overflow:hidden;}
  .cp-fill{height:100%; background:var(--gold); border-radius:99px; transition:width .3s ease;}
  .cp-label{font-size:11.5px; color:#CFD7EA; white-space:nowrap;}

  .tabbar{position:sticky; top:env(safe-area-inset-top,0px); z-index:15; background:var(--paper); border-bottom:1px solid var(--rule); overflow-x:auto; white-space:nowrap;}
  .tabbar-inner{max-width:900px; margin:0 auto; display:flex; padding-inline:16px;}
  .tab-btn{font-family:inherit; font-size:14px; font-weight:600; color:var(--ink-soft); background:none; border:none; padding:13px 16px; cursor:pointer; position:relative; flex-shrink:0;}
  .tab-btn.active{color:var(--accent-text);}
  .tab-btn.active::after{content:''; position:absolute; left:12px; right:12px; bottom:0; height:3px; background:var(--gold); border-radius:3px 3px 0 0;}
  .tab-count{font-size:11px; color:var(--ink-soft); margin-left:5px;}

  .slide-progress{max-width:900px; margin:0 auto; padding:10px 16px 0; display:flex; align-items:center; gap:10px;}
  .sp-track{flex:1; height:5px; border-radius:99px; background:var(--rule); overflow:hidden;}
  .sp-fill{height:100%; background:var(--accent-text); border-radius:99px; transition:width .3s ease;}
  .sp-label{font-size:12px; color:var(--ink-soft); white-space:nowrap; font-weight:600;}

  .wrap{max-width:900px; margin:0 auto; padding:16px 16px calc(110px + env(safe-area-inset-bottom,0px));}

  .qcard{background:var(--card); border:1px solid var(--rule); border-radius:14px; padding:18px;}
  .qhead{display:flex; align-items:flex-start; gap:10px; margin-bottom:10px;}
  .qnum{width:32px; height:32px; border-radius:8px; background:var(--gold-soft); color:var(--accent-text); display:flex; align-items:center; justify-content:center; font-weight:700; font-family:'Fraunces',serif; flex-shrink:0; font-size:14px;}
  .qtext{font-size:16px; line-height:1.55; flex:1;}
  .qtag{font-size:10.5px; color:var(--gold); background:var(--gold-soft); padding:2px 7px; border-radius:99px; font-weight:700; margin-left:6px; white-space:nowrap;}
  .marks-pill{font-size:11px; font-weight:700; color:var(--ink-soft); border:1px solid var(--rule); padding:3px 9px; border-radius:99px; margin-left:auto; flex-shrink:0; white-space:nowrap;}
  .step-badge{font-size:11px; font-weight:700; color:var(--accent-text); background:var(--paper-2); border:1px solid var(--rule); padding:3px 9px; border-radius:99px; display:inline-block; margin-bottom:12px;}

  .options{display:flex; flex-direction:column; gap:8px; margin-top:10px;}
  .opt{display:flex; align-items:center; gap:10px; padding:11px 12px; border:1.5px solid var(--rule); border-radius:10px; cursor:pointer; font-size:15px; background:var(--paper);}
  .opt input{accent-color:var(--navy-2);}
  .opt.is-correct{border-color:var(--success); background:var(--success-soft);}
  .opt.is-wrong{border-color:var(--danger); background:var(--danger-soft);}
  .opt.locked{cursor:not-allowed;}

  .subpart-label{font-weight:700; font-size:12.5px; color:var(--accent-text); text-transform:uppercase; letter-spacing:.03em; margin:14px 0 6px;}
  .or-divider{text-align:center; font-size:11px; font-weight:700; letter-spacing:.08em; color:var(--ink-soft); margin:8px 0;}

  .steps{display:flex; flex-direction:column; gap:10px;}
  .step-line{font-size:14.5px; line-height:1.9; background:var(--paper); border:1px dashed var(--rule); border-radius:10px; padding:9px 12px;}
  .step-line.resolved{opacity:.9;}
  .blank-input{font-family:'IBM Plex Mono',monospace; font-size:14px; border:1.5px solid var(--rule); border-radius:6px; padding:3px 8px; width:120px; background:var(--card); color:var(--ink); margin:0 2px;}
  .blank-input:focus{outline:none; border-color:var(--focus); box-shadow:0 0 0 3px var(--gold-soft);}
  .blank-input.is-correct{border-color:var(--success); background:var(--success-soft);}
  .blank-input.is-wrong{border-color:var(--danger); background:var(--danger-soft);}
  .blank-input[disabled]{opacity:.85;}
  .reveal-note{font-size:12px; color:var(--danger); padding-left:2px; margin-top:-2px;}

  .feedback{font-size:13.5px; padding:10px 12px; border-radius:9px; margin-top:14px; display:none;}
  .feedback.show{display:block;}
  .feedback.ok{background:var(--success-soft); color:var(--success);}
  .feedback.retry{background:var(--gold-soft); color:var(--retry-text);}
  .feedback.reveal{background:var(--danger-soft); color:var(--danger);}
  .feedback.err{background:var(--danger-soft); color:var(--danger);}
  .attempts-dots{display:inline-flex; gap:4px; margin-left:2px;}
  .attempts-dots span{width:6px; height:6px; border-radius:50%; background:var(--ink-soft); opacity:.3; display:inline-block;}
  .attempts-dots span.used{opacity:1; background:var(--danger);}

  .ar-box{background:var(--paper-2); border-radius:10px; padding:10px 12px; margin-bottom:12px; font-size:13px; color:var(--ink-soft);}

  .navbar{
    position:fixed; left:0; right:0; bottom:0; z-index:30; background:var(--paper);
    border-top:1px solid var(--rule); padding:10px 16px calc(10px + env(safe-area-inset-bottom,0px));
  }
  .navbar-inner{max-width:900px; margin:0 auto; display:flex; gap:8px; flex-wrap:wrap; align-items:center;}
  .btn{font-family:inherit; font-size:13.5px; font-weight:700; padding:10px 14px; border-radius:9px; border:1.5px solid var(--rule); background:var(--card); color:var(--ink); cursor:pointer;}
  .btn:hover{border-color:var(--navy-2);}
  .btn:disabled{opacity:.4; cursor:not-allowed;}
  .btn-primary{background:var(--navy-2); color:#fff; border-color:var(--navy-2);}
  .navbar .spacer{flex:1;}

  .fab{position:fixed; right:16px; bottom:calc(74px + env(safe-area-inset-bottom,0px)); background:var(--gold); color:var(--navy); border:none; border-radius:999px; padding:11px 16px; font-family:inherit; font-weight:700; font-size:13px; display:flex; align-items:center; gap:7px; cursor:pointer; box-shadow:0 6px 16px rgba(0,0,0,.18); z-index:35;}
  .fab-badge{background:rgba(255,255,255,.55); border-radius:99px; padding:1px 7px; font-size:11px;}

  .overlay{position:fixed; inset:0; background:rgba(20,18,14,.45); z-index:50; display:none; align-items:flex-end; justify-content:center;}
  .overlay.show{display:flex;}
  .palette{background:var(--card); width:min(480px,100%); max-height:78vh; overflow-y:auto; border-radius:18px 18px 0 0; padding:18px 18px calc(18px + env(safe-area-inset-bottom,0px)); border:1px solid var(--rule); border-bottom:none;}
  @media (min-width:640px){ .overlay{align-items:center;} .palette{border-radius:18px; border-bottom:1px solid var(--rule); max-height:74vh;} }
  .palette-head{display:flex; align-items:center; justify-content:space-between; margin-bottom:6px;}
  .palette-head h2{font-size:16px;}
  .palette-close{background:none; border:none; font-size:20px; color:var(--ink-soft); cursor:pointer; padding:4px;}
  .palette-legend{display:flex; gap:12px; flex-wrap:wrap; font-size:11px; color:var(--ink-soft); margin:10px 0 14px;}
  .palette-legend span{display:inline-flex; align-items:center; gap:5px;}
  .dot{width:9px; height:9px; border-radius:50%; display:inline-block;}
  .dot.correct{background:var(--success);} .dot.revealed{background:var(--danger);}
  .dot.skipped{background:var(--gold);} .dot.locked{background:var(--locked);}
  .dot.current{background:var(--navy-2);}
  .chip-grid{display:grid; grid-template-columns:repeat(auto-fill,minmax(48px,1fr)); gap:8px;}
  .chip{border:1.5px solid var(--rule); border-radius:9px; padding:8px 4px; text-align:center; font-family:'IBM Plex Mono',monospace; font-size:12.5px; font-weight:600; cursor:pointer; background:var(--paper); color:var(--ink);}
  .chip[data-status="correct"]{border-color:var(--success); background:var(--success-soft); color:var(--success);}
  .chip[data-status="revealed"]{border-color:var(--danger); background:var(--danger-soft); color:var(--danger);}
  .chip[data-status="skipped"]{border-color:var(--gold); background:var(--gold-soft); color:var(--retry-text);}
  .chip[data-status="locked"]{border-color:var(--rule); color:var(--locked); cursor:not-allowed; background:var(--paper-2);}
  .chip[data-status="current"]{outline:2px solid var(--navy-2); outline-offset:1px;}

  .toast{position:fixed; left:50%; bottom:calc(140px + env(safe-area-inset-bottom,0px)); transform:translateX(-50%); background:var(--ink); color:var(--paper); padding:9px 16px; border-radius:99px; font-size:13px; opacity:0; pointer-events:none; transition:opacity .25s ease; z-index:60;}
  .toast.show{opacity:1;}

  .done-card{text-align:center; padding:36px 20px;}
  .done-card .big{font-size:38px; margin-bottom:6px;}
  .score-pills{display:flex; gap:10px; justify-content:center; flex-wrap:wrap; margin:16px 0;}
  .score-pill{padding:8px 16px; border-radius:99px; font-size:13px; font-weight:700;}

  footer.brandfoot{max-width:900px; margin:14px auto 0; padding:0 16px calc(110px + env(safe-area-inset-bottom,0px)); text-align:center; font-size:12px; color:var(--ink-soft);}

  .mode-pill{font-family:inherit; font-size:12.5px; font-weight:700; color:var(--navy); background:var(--gold); border:none; border-radius:99px; padding:6px 14px; cursor:pointer; margin-top:12px; display:inline-flex; align-items:center; gap:6px;}
  .mode-pick{display:flex; flex-direction:column; gap:14px; padding:8px 0 24px;}
  .mode-card{background:var(--card); border:1.5px solid var(--rule); border-radius:16px; padding:22px; cursor:pointer; text-align:left;}
  .mode-card:hover{border-color:var(--navy-2);}
  .mode-card .mode-icon{font-size:26px; margin-bottom:8px;}
  .mode-card h3{font-size:18px; margin-bottom:6px;}
  .mode-card p{font-size:13.5px; color:var(--ink-soft); line-height:1.5; margin:0;}
  .quiz-hint{font-size:12.5px; color:var(--ink-soft); background:var(--paper-2); border-radius:9px; padding:8px 12px; margin-bottom:14px;}
  .review-row{border:1px solid var(--rule); border-radius:10px; padding:11px 13px; margin-bottom:10px;}
  .review-tag{font-size:11.5px; font-weight:700; margin-bottom:4px; display:block;}
  .review-tag.ok{color:var(--success);} .review-tag.bad{color:var(--danger);} .review-tag.na{color:var(--ink-soft);}
  .review-q{font-size:13.5px; margin-bottom:6px; color:var(--ink-soft);}
  .review-ans{font-size:13px; margin-bottom:2px;}

  .who-row{max-width:900px; margin:12px auto 0; display:flex; flex-wrap:wrap; gap:8px; align-items:center;}
  .who-row .mode-pill{margin-top:0;}
  .who{display:inline-flex; flex-wrap:wrap; align-items:center; gap:6px; font-size:12.5px; color:#CFD7EA;}
  .who b{color:#F4EFDF;}
  .who button{font-family:inherit; font-size:12px; font-weight:700; color:#F4EFDF; background:rgba(255,255,255,.12); border:1px solid rgba(255,255,255,.22); border-radius:99px; padding:5px 11px; cursor:pointer;}
  .who button:hover{background:rgba(255,255,255,.2);}
  .login-card{max-width:460px; margin:8px auto 0; background:var(--card); border:1px solid var(--rule); border-radius:16px; padding:22px;}
  .login-card h2{font-size:21px; margin-bottom:4px;}
  .login-card p.lead{font-size:13.5px; color:var(--ink-soft); margin:0 0 16px; line-height:1.5;}
  .fld{display:flex; flex-direction:column; gap:5px; margin-bottom:12px;}
  .fld label{font-size:12px; font-weight:700; letter-spacing:.04em; text-transform:uppercase; color:var(--ink-soft);}
  .fld input{font-family:inherit; font-size:15px; padding:10px 12px; border:1.5px solid var(--rule); border-radius:9px; background:var(--paper); color:var(--ink);}
  .fld input:focus{outline:none; border-color:var(--focus); box-shadow:0 0 0 3px var(--gold-soft);}
  .fld-row{display:grid; grid-template-columns:1fr 1fr; gap:10px;}
  .login-err{font-size:13px; color:var(--danger); background:var(--danger-soft); border-radius:8px; padding:8px 10px; margin-bottom:12px;}
  .login-note{font-size:12px; color:var(--ink-soft); margin-top:12px; line-height:1.5;}
  .known{margin:0 0 16px; display:flex; flex-direction:column; gap:6px;}
  .known-label{font-size:12px; font-weight:700; letter-spacing:.04em; text-transform:uppercase; color:var(--ink-soft);}
  .known-btn{display:flex; justify-content:space-between; align-items:center; gap:10px; text-align:left; font-family:inherit; font-size:14.5px; padding:10px 12px; border:1.5px solid var(--rule); border-radius:10px; background:var(--paper); color:var(--ink); cursor:pointer;}
  .known-btn:hover{border-color:var(--navy-2);}
  .known-btn small{font-size:12px; color:var(--ink-soft);}
  .rec-card{background:var(--card); border:1px solid var(--rule); border-radius:14px; padding:18px; margin-bottom:14px;}
  .rec-card h2{font-size:19px; margin-bottom:10px;}
  .rec-card h3{font-size:15px; margin:4px 0 8px;}
  .rec-table-wrap{overflow-x:auto;}
  .rec-table{width:100%; border-collapse:collapse; font-size:13.5px; font-variant-numeric:tabular-nums;}
  .rec-table th{text-align:left; font-size:11.5px; text-transform:uppercase; letter-spacing:.04em; color:var(--ink-soft); padding:6px 8px; border-bottom:1px solid var(--rule); white-space:nowrap;}
  .rec-table td{padding:8px; border-bottom:1px solid var(--rule); white-space:nowrap;}
  .rec-bar{display:inline-block; width:70px; height:6px; border-radius:99px; background:var(--rule); overflow:hidden; vertical-align:middle; margin-right:6px;}
  .rec-bar i{display:block; height:100%; background:var(--success);}
  .rec-empty{font-size:13.5px; color:var(--ink-soft);}
  .sec-sub{max-width:900px; margin:0 auto; padding:10px 16px 0; font-size:12.5px; font-weight:700; color:var(--accent-text); letter-spacing:.02em;}
  .opt{flex-wrap:wrap;}
  .opt-l{font-family:'IBM Plex Mono',monospace; font-size:13px; font-weight:600; color:var(--ink-soft);}
  .opt-t{flex:1; min-width:0;}
  .opt.is-chosen:not(.is-correct):not(.is-wrong){border-color:var(--navy-2); background:var(--gold-soft);}
  .opt-tag{font-size:11px; font-weight:700; padding:2px 8px; border-radius:99px; background:var(--card); border:1px solid currentColor; white-space:nowrap;}
  .opt.is-correct .opt-tag{color:var(--success);} .opt.is-wrong .opt-tag{color:var(--danger);} .opt.is-chosen:not(.is-correct):not(.is-wrong) .opt-tag{color:var(--accent-text);}
  .chosen{font-size:13.5px; margin-top:10px; padding:8px 12px; border-radius:9px;}
  .chosen.ok{background:var(--success-soft); color:var(--success);} .chosen.bad{background:var(--danger-soft); color:var(--danger);}
  .chosen + .chosen{margin-top:6px;}
  .solution{margin-top:14px; border:1px solid var(--rule); border-left:4px solid var(--gold); background:var(--paper-2); border-radius:10px; padding:12px 14px;}
  .sol-h{font-family:'Fraunces',Georgia,serif; font-weight:700; font-size:14px; color:var(--accent-text); margin-bottom:6px;}
  .sol-line{font-size:14.5px; line-height:1.7;}
  .rev-sol summary{cursor:pointer; font-size:12.5px; font-weight:700; color:var(--accent-text); margin-top:6px;}
  .rev-sol .solution{margin-top:8px;}
  .chapter-credit{max-width:900px; margin:4px auto 0; font-size:12px; color:#CFD7EA;}
  footer.brandfoot a{color:inherit;}
  .blank-input.wide{width:260px; max-width:100%; font-family:'Source Sans 3',sans-serif;}
  .blank-input.expr{width:170px; max-width:100%;}
  /*fix-layout*/ .cp-fill,.sp-fill{width:0;} .fld{min-width:0;} .fld input{width:100%; min-width:0; box-sizing:border-box;}
  .fig{margin:6px 0 10px; display:flex; justify-content:center;}
  .figsvg{width:100%; max-width:340px; height:auto; overflow:visible;}
  .figsvg .ln,.figsvg .arm{stroke:var(--ink); stroke-width:1.6; fill:none;}
  .figsvg .arm{stroke:var(--accent-text); stroke-width:2;}
  .figsvg .pt{fill:var(--ink);}
  .figsvg .lb{fill:var(--ink); font:600 13px 'Source Sans 3',sans-serif;}
  .figsvg .al{fill:var(--accent-text); font:700 12px 'Source Sans 3',sans-serif;}
  .figsvg .wg{fill:var(--gold-soft); stroke:var(--gold); stroke-width:1.2;}
  .figsvg .wg2{fill:rgba(199,154,62,.35); stroke:var(--gold); stroke-width:1.2;}
  .figsvg .ra{fill:none; stroke:var(--ink); stroke-width:1.2;}
  .figsvg .ahd{fill:var(--ink);}
  .figsvg .pr{fill:var(--paper-2); stroke:var(--ink-soft); stroke-width:1.2;}
  .figsvg .tk{stroke:var(--ink-soft); stroke-width:.8;}
  .figsvg .po{fill:var(--ink); font:600 8.5px 'IBM Plex Mono',monospace;}
  .figsvg .pi{fill:var(--danger); font:600 7.5px 'IBM Plex Mono',monospace;}
  .figsvg .sh{fill:var(--gold-soft); stroke:var(--ink); stroke-width:1.5; stroke-linejoin:round;}
  .figsvg .sh2{fill:rgba(199,154,62,.38); stroke:var(--ink); stroke-width:1.5; stroke-linejoin:round;}
  .figsvg .sh3{fill:rgba(199,154,62,.22); stroke:var(--ink); stroke-width:1.5; stroke-linejoin:round;}
  .figsvg .none{fill:none;}
  .figsvg .hid{stroke-dasharray:5 4; fill:none;}
  .figsvg .tick{stroke:var(--ink); stroke-width:1.4; fill:none;}
  .figsvg .shd{fill:var(--accent-text); fill-opacity:.55;}
  .figsvg .cell{fill:var(--card); stroke:var(--ink); stroke-width:1.1;}
  .figsvg .dr{fill:var(--danger);} .figsvg .dw{fill:var(--card); stroke:var(--ink); stroke-width:1.2;}
  .figsvg .dotfill{fill:var(--danger);}
  .tt{border-collapse:collapse; font-size:13.5px; margin:4px auto; font-variant-numeric:tabular-nums;} .tt th,.tt td{border:1px solid var(--rule); padding:4px 9px; text-align:left;} .tt th{background:var(--paper-2); font-weight:700;} .ttw{overflow-x:auto; max-width:100%;}
  .fq{display:inline-flex; flex-direction:column; vertical-align:middle; text-align:center; font-size:.82em; line-height:1.12; margin:0 2px;}
  .fq>span:first-child{border-bottom:1.5px solid currentColor; padding:0 2px;}
  .fq>span:last-child{padding:0 2px;}
  .mx{white-space:nowrap;}
  .blank-input.fr{width:110px;}
  @media (prefers-reduced-motion:reduce){ *{transition:none !important;} }
.datline{display:block;margin-top:8px;padding:8px 10px;border-radius:8px;background:var(--paper-2);font:500 14px/1.7 'IBM Plex Mono',monospace;word-spacing:2px;overflow-wrap:anywhere;}

  .theory{display:flex;flex-direction:column;gap:16px;padding-bottom:40px;}
  .hub{display:grid;grid-template-columns:1fr;gap:12px;}
  @media (min-width:640px){ .hub{grid-template-columns:repeat(3,1fr);} }
  .hub-card{background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:16px;display:flex;flex-direction:column;gap:8px;}
  .hub-card h3{font-family:'Fraunces',serif;font-size:18px;margin:0;color:var(--accent-text);}
  .hub-card p{margin:0;font-size:14px;color:var(--ink-soft);line-height:1.5;}
  .hub-btns{display:flex;flex-wrap:wrap;gap:8px;margin-top:auto;}
  .hub-btn{min-height:40px;border:1.5px solid var(--rule);background:var(--paper-2);color:var(--ink);border-radius:10px;padding:8px 12px;font:600 14px 'Source Sans 3',sans-serif;cursor:pointer;text-align:left;}
  .hub-btn:hover{border-color:var(--gold);}
  .hub-btn.primary{background:var(--navy);color:#F4EFDF;border-color:var(--navy);}
  .note{background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:18px;scroll-margin-top:120px;}
  .note h2{font-family:'Fraunces',serif;font-size:22px;margin:0 0 4px;color:var(--ink);}
  .note .lt{font-size:13.5px;color:var(--ink-soft);margin:0 0 12px;}
  .note .lt b{color:var(--accent-text);}
  .note h4{font:700 15px 'Source Sans 3',sans-serif;margin:16px 0 6px;color:var(--accent-text);}
  .note p,.note li{font-size:15.5px;line-height:1.6;}
  .note ul{padding-left:20px;margin:6px 0;}
  .keybox{background:var(--gold-soft);border-left:4px solid var(--gold);border-radius:8px;padding:10px 12px;margin:10px 0;font-size:15px;line-height:1.6;}
  .keybox b{color:var(--ink);}
  .ex{background:var(--paper-2);border-radius:10px;padding:12px;margin:10px 0;}
  .ex .exh{font-family:'Fraunces',serif;font-weight:700;color:var(--accent-text);margin-bottom:4px;}
  .ex .exl{font-size:15px;line-height:1.7;}
  .ttab{width:100%;border-collapse:collapse;font-size:14.5px;margin:8px 0;}
  .ttab th,.ttab td{border:1px solid var(--rule);padding:7px 8px;text-align:left;vertical-align:top;}
  .ttab th{background:var(--paper-2);}
  .tscroll{overflow-x:auto;}
  .mono{font-family:'IBM Plex Mono',monospace;font-size:.92em;}
  @media (max-width:480px){ .ttab{font-size:13.5px;} .ttab th,.ttab td{padding:6px;} }
  .note .figsvg{max-width:100%;height:auto;display:block;margin:8px auto;}


  .rep-top{display:flex;flex-wrap:wrap;gap:18px;align-items:center;justify-content:center;margin-top:8px;}
  .rep-legend{flex:1;min-width:220px;display:flex;flex-direction:column;gap:8px;}
  .rep-cap{font:700 13px 'Source Sans 3',sans-serif;color:var(--ink-soft);text-transform:uppercase;letter-spacing:.05em;}
  .rep-li{display:flex;align-items:center;gap:6px;font-size:15px;}
  .rep-n{margin-left:auto;white-space:nowrap;padding-left:8px;font-family:'IBM Plex Mono',monospace;font-weight:600;}
  .sw{display:inline-block;width:12px;height:12px;border-radius:3px;flex:none;}
  .rep-grid{display:grid;grid-template-columns:1fr;gap:12px;}
  @media (min-width:640px){ .rep-grid{grid-template-columns:1fr 1fr;} }
  .rep-card{background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:14px;display:flex;flex-direction:column;gap:10px;}
  .rep-h{display:flex;align-items:center;gap:8px;font:700 16px 'Source Sans 3',sans-serif;}
  .crit{display:inline-grid;place-items:center;width:28px;height:28px;border-radius:50%;color:var(--card);font:700 15px Fraunces,serif;flex:none;}
  .rep-row{display:flex;gap:14px;align-items:center;}
  .rep-stats{display:flex;flex-direction:column;gap:5px;font-size:14.5px;}
  .rep-stats div{display:flex;align-items:center;gap:6px;}
  .rep-lvl{margin-top:4px;color:var(--accent-text);} .rep-lvl span{color:var(--ink-soft);font-size:13px;}
  .rep-note{font-size:13px;color:var(--ink-soft);}


  .skip-chips{display:flex;flex-wrap:wrap;gap:8px;justify-content:center;margin:12px 0;}
  .skip-chips .chip{min-width:48px;min-height:40px;cursor:pointer;border:1.5px solid var(--gold);background:var(--gold-soft);color:var(--retry-text);border-radius:10px;font:600 14px 'IBM Plex Mono',monospace;}
  .skip-btns{display:flex;flex-wrap:wrap;gap:10px;justify-content:center;margin-top:12px;}


  .kb-toggle{border:none;background:transparent;font-size:15px;cursor:pointer;padding:0 2px;vertical-align:middle;opacity:.7;}
  .vkb{position:fixed;left:0;right:0;bottom:0;z-index:80;background:var(--paper-2);border-top:1px solid var(--rule);box-shadow:0 -6px 20px rgba(0,0,0,.12);padding:6px 6px 8px;display:none;}
  .vkb.show{display:block;}
  .vkb-top{display:flex;justify-content:space-between;align-items:center;max-width:640px;margin:0 auto 4px;font:600 12px 'Source Sans 3',sans-serif;color:var(--ink-soft);}
  .vkb-link{border:none;background:none;color:var(--accent-text);font:600 12px 'Source Sans 3',sans-serif;cursor:pointer;text-decoration:underline;}
  .vkb-row{display:flex;gap:5px;max-width:640px;margin:0 auto 5px;}
  .vkb-k{flex:1;min-height:42px;border:1px solid var(--rule);border-radius:8px;background:var(--card);color:var(--ink);font:600 17px 'IBM Plex Mono',monospace;cursor:pointer;padding:0;}
  .vkb-k.fn{font:600 13px 'Source Sans 3',sans-serif;background:var(--gold-soft);}
  .vkb-k.done{background:var(--navy);color:#F4EFDF;border-color:var(--navy);}
  .vkb-k:active{transform:scale(.96);}
  body.kb-open .wrap{padding-bottom:360px;}
  body.kb-open .fab{display:none;}
  .tool-bar{display:flex;gap:8px;flex-wrap:wrap;margin:-4px 0 10px;}
  .tool-btn{min-height:36px;border:1.5px solid var(--gold);background:var(--gold-soft);color:var(--ink);border-radius:18px;padding:4px 12px;font:600 13.5px 'Source Sans 3',sans-serif;cursor:pointer;}
  .tool-panel{position:fixed;z-index:85;background:var(--card);border:1px solid var(--rule);border-radius:14px;box-shadow:0 10px 30px rgba(0,0,0,.25);display:none;}
  .tool-panel.show{display:block;}
  .tp-head{display:flex;align-items:center;gap:8px;padding:8px 10px;border-bottom:1px solid var(--rule);font:600 14px 'Source Sans 3',sans-serif;}
  .tp-note{font-size:12px;color:var(--ink-soft);font-weight:500;}
  .tp-x{margin-left:auto;border:1px solid var(--rule);background:var(--paper-2);color:var(--ink);border-radius:8px;min-height:32px;padding:2px 10px;cursor:pointer;font:600 13px 'Source Sans 3',sans-serif;}
  .tp-x + .tp-x{margin-left:0;}
  .calc{right:12px;bottom:84px;width:min(330px,calc(100vw - 24px));}
  body.calc-open .wrap{padding-bottom:440px;}
  body.calc-open .fab{display:none;}
  @media (max-width:640px){ .calc{left:6px;right:6px;width:auto;} .calc-keys button{min-height:36px;} }
  .calc-disp{padding:8px 12px;text-align:right;background:var(--paper-2);}
  .calc-expr{font:500 13px 'IBM Plex Mono',monospace;color:var(--ink-soft);min-height:18px;word-break:break-all;}
  .calc-res{font:600 24px 'IBM Plex Mono',monospace;color:var(--ink);word-break:break-all;}
  .calc-keys{display:grid;grid-template-columns:repeat(6,1fr);gap:5px;padding:8px;}
  .calc-keys button{min-height:40px;border:1px solid var(--rule);border-radius:8px;background:var(--card);color:var(--ink);font:600 14px 'IBM Plex Mono',monospace;cursor:pointer;padding:0;}
  .calc-keys button.fn{background:var(--gold-soft);font-family:'Source Sans 3',sans-serif;font-size:13px;}
  .calc-keys button.eq{grid-column:span 2;background:var(--navy);color:#F4EFDF;border-color:var(--navy);}
  .desmos{left:50%;top:50%;transform:translate(-50%,-50%);width:min(760px,calc(100vw - 16px));height:min(560px,calc(100vh - 90px));flex-direction:column;}
  .desmos.show{display:flex;}
  #desmosBox{flex:1;min-height:0;border-radius:0 0 14px 14px;overflow:hidden;}
  .desmos-msg{padding:24px;text-align:center;color:var(--ink-soft);}
  .chip[data-status="skipped"]{cursor:pointer;}
  .chip[data-status="answered"]{border-color:var(--accent-text);color:var(--accent-text);}


  /* larger reading sizes */
  .chapter-eyebrow{font-size:13.5px;}
  .chapter-title{font-size:36px;line-height:1.15;}
  .chapter-sub{font-size:16.5px;}
  .tab-btn{font-size:15.5px;}
  .sec-sub{font-size:15px;}
  .qtext{font-size:19px;line-height:1.55;}
  .qnum{width:36px;height:36px;font-size:16px;}
  .opt{font-size:17.5px;padding:12px 14px;}
  .step-line{font-size:17.5px;line-height:2;}
  .blank-input{font-size:16px;}
  .sol-line{font-size:16.5px;}
  .feedback{font-size:15px;}
  .note h2{font-size:28px;}
  .note h4{font-size:19px;}
  .note p,.note li{font-size:17.5px;line-height:1.65;}
  .note .lt{font-size:15.5px;}
  .ex .exh{font-size:18px;} .ex .exl{font-size:16.5px;}
  .keybox{font-size:16.5px;}
  .ttab{font-size:15.5px;}
  .hub-card h3{font-size:21px;} .hub-card p{font-size:15.5px;} .hub-btn{font-size:15px;}
  @media (max-width:480px){ .chapter-title{font-size:30px;} .qtext{font-size:18px;} .opt,.step-line{font-size:16.5px;} .note h2{font-size:24px;} .note p,.note li{font-size:16.5px;} }
</style>
</head>
<body>
<header class="brand">
  <div class="brand-row">
    <div class="crest">BM</div>
    <div>
      <div class="brand-name">Brain &amp; Mind Academy</div>
      <div class="series-name">Sopaan <span style="font-weight:400;font-size:14px;color:#CFD7EA;">Practice Series</span></div>
    </div>
  </div>
  <div class="chapter-eyebrow">AP Physics 1 · Chapter 5</div>
  <div class="chapter-title">Torque and Rotational Dynamics</div>
  <div class="chapter-sub">Theory Notes · Practice by Learning Objective · Tests A–D</div><div class="chapter-credit">Organised by AP Physics 1 CED learning objectives · Unit 5</div>
  <div class="chapter-progress">
    <div class="cp-track"><div class="cp-fill" id="cpFill"></div></div>
    <div class="cp-label" id="cpLabel">0% complete</div>
  </div>
  <div class="who-row"><button class="mode-pill" id="modePill" hidden>Choose a mode</button><span class="who" id="whoBar" hidden></span></div>
</header>

<nav class="tabbar"><div class="tabbar-inner" id="tabbar"></div></nav>
<div class="sec-sub" id="secSub"></div><div class="slide-progress"><div class="sp-track"><div class="sp-fill" id="spFill"></div></div><div class="sp-label" id="spLabel">Question 1 of 49</div></div>
<div class="wrap" id="wrap"></div>
<footer class="brandfoot">Brain &amp; Mind Academy · Sopaan Practice Series · AP Physics 1 · Chapter 5<br>Organised by the topics and learning objectives of the AP Physics 1 Course and Exam Description (College Board, 2024), Unit 5. Theory notes, questions, tests and worked solutions are written by Brain &amp; Mind Academy; no workbook or College Board questions are reproduced.</footer>

<div class="navbar"><div class="navbar-inner" id="navbarInner"></div></div>
<button class="fab" id="paletteFab"><span>Questions</span><span class="fab-badge" id="fabBadge">0/49</span></button>

<div class="overlay" id="overlay">
  <div class="palette" role="dialog" aria-label="Question palette">
    <div class="palette-head"><h2 id="paletteTitle">Question palette</h2><button class="palette-close" id="paletteClose">&times;</button></div>
    <div class="palette-legend">
      <span><i class="dot current"></i>Current</span><span><i class="dot correct"></i>Correct</span>
      <span><i class="dot revealed"></i>Revealed</span><span><i class="dot skipped"></i>Skipped</span>
      <span><i class="dot locked"></i>Locked</span>
    </div>
    <div class="chip-grid" id="paletteBody"></div>
  </div>
</div>
<div class="toast" id="toast"></div>

<script>
(function(){
"use strict";

/* ================= audio ================= */
var audioCtx=null;
function ensureAudio(){ if(!audioCtx){ try{ audioCtx=new (window.AudioContext||window.webkitAudioContext)(); }catch(e){} } return audioCtx; }
function playTone(freqs,dur,type){
  var ctx=ensureAudio(); if(!ctx) return;
  try{
    var t0=ctx.currentTime;
    freqs.forEach(function(f,i){
      var o=ctx.createOscillator(), g=ctx.createGain();
      o.type=type||'sine'; o.frequency.value=f;
      var start=t0+i*dur;
      g.gain.setValueAtTime(0.0001,start);
      g.gain.exponentialRampToValueAtTime(0.16,start+0.02);
      g.gain.exponentialRampToValueAtTime(0.0001,start+dur);
      o.connect(g); g.connect(ctx.destination);
      o.start(start); o.stop(start+dur+0.02);
    });
  }catch(e){}
}
function playSuccess(){ playTone([523.25,659.25,783.99],0.11,'sine'); }
function playWrong(){ playTone([220,185],0.14,'square'); }
function playReveal(){ playTone([300,220],0.16,'triangle'); }

/* ================= helpers ================= */
function norm(s){
  return String(s===undefined||s===null?'':s).trim().toLowerCase().replace(/\s+/g,'')
    .replace(/[×∗*·]/g,'x').replace(/÷/g,'/').replace(/–|—|−/g,'-')
    .replace(/[²]/g,'^2').replace(/[³]/g,'^3').replace(/[⁴]/g,'^4').replace(/[⁵]/g,'^5')
    .replace(/[⁶]/g,'^6').replace(/[⁷]/g,'^7').replace(/[⁸]/g,'^8').replace(/[⁹]/g,'^9')
    .replace(/\^/g,'');
}
function parseNum(s){
  if(s===undefined||s===null) return null;
  var t=String(s).trim().replace(/,/g,'').replace(/[−–—]/g,'-').replace(/\s+/g,''); if(t==='') return null;
  var neg=false; if(t[0]==='-'){neg=true;t=t.slice(1);}
  var m=t.match(/^(\d+)\/(\d+)$/);
  if(m){ var v=parseInt(m[1],10)/parseInt(m[2],10); return neg?-v:v; }
  if(/^\d+(\.\d+)?$/.test(t)){ var v2=parseFloat(t); return neg?-v2:v2; }
  return null;
}
/* ---- expression checker: compares answers like (P-2w)/2 and P/2-w at random values ---- */
function exprPrep(s){
  return String(s).toLowerCase().replace(/\s+/g,'').replace(/^[a-z]\(x\)=/i,'').replace(/^y=/i,'').replace(/[×∗·]/g,'*').replace(/÷/g,'/').replace(/[−–—]/g,'-')
    .replace(/²/g,'^2').replace(/³/g,'^3').replace(/⁴/g,'^4').replace(/⁵/g,'^5').replace(/⁶/g,'^6').replace(/⁷/g,'^7').replace(/⁸/g,'^8').replace(/⁹/g,'^9').replace(/π/g,'#').replace(/pi/gi,'#').replace(/½/g,'(1/2)');
}
function exprCompile(src){
  var s=exprPrep(src), i=0, toks=[], absDepth=0;
  while(i<s.length){
    var ch=s[i];
    if(/[0-9.]/.test(ch)){ var j=i; while(j<s.length && /[0-9.]/.test(s[j])) j++; toks.push({k:'n',v:parseFloat(s.slice(i,j))}); i=j; continue; }
    if(/[a-zA-Z#]/.test(ch)){ toks.push({k:'v',v:ch}); i++; continue; }
    if(ch==='|'){ var pv=toks[toks.length-1]; var opn=(absDepth===0)||!pv||('+-*/^(['.indexOf(pv.k)>=0); if(opn){ absDepth++; toks.push({k:'['}); } else { absDepth--; toks.push({k:']'}); } i++; continue; }
    if('+-*/^()'.indexOf(ch)>=0){ toks.push({k:ch}); i++; continue; }
    return null;
  }
  var out=[];
  for(var t=0;t<toks.length;t++){
    var a=toks[t], b=out[out.length-1];
    if(b && (b.k==='n'||b.k==='v'||b.k===')'||b.k===']') && (a.k==='n'||a.k==='v'||a.k==='('||a.k==='[')) out.push({k:'&'});
    out.push(a);
  }
  var p=0;
  function peek(){ return out[p]; }
  function parseE(){ var n=parseT(); while(peek() && (peek().k==='+'||peek().k==='-')){ var o=out[p++].k, r=parseT(); n=(function(l,r,o){return function(e){ return o==='+'?l(e)+r(e):l(e)-r(e); };})(n,r,o); } return n; }
  function parseI(){ var n=parseU(); while(peek() && peek().k==='&'){ p++; var r=parseP(); n=(function(l,r){return function(e){ return l(e)*r(e); };})(n,r); } return n; }
  function parseT(){ var n=parseI(); while(peek() && (peek().k==='*'||peek().k==='/')){ var o=out[p++].k, r=parseI(); n=(function(l,r,o){return function(e){ return o==='*'?l(e)*r(e):l(e)/r(e); };})(n,r,o); } return n; }
  function parseU(){ if(peek() && peek().k==='-'){ p++; var u=parseU(); return function(e){ return -u(e); }; } if(peek() && peek().k==='+'){ p++; return parseU(); } return parseP(); }
  function parseP(){ var b=parseA(); if(peek() && peek().k==='^'){ p++; var x=parseU(); return function(e){ return Math.pow(b(e),x(e)); }; } return b; }
  function parseA(){
    var t=out[p++]; if(!t) throw 0;
    if(t.k==='n') return function(){ return t.v; };
    if(t.k==='v') return t.v==='#' ? function(){ return Math.PI; } : function(e){ return e[t.v]; };
    if(t.k==='('){ var n=parseE(); if(!peek()||peek().k!==')') throw 0; p++; return n; }
    if(t.k==='['){ var m=parseE(); if(!peek()||peek().k!==']') throw 0; p++; return function(e){ return Math.abs(m(e)); }; }
    throw 0;
  }
  try{ var f=parseE(); if(p!==out.length) return null; return f; }catch(e){ return null; }
}
function exprEqual(input,answer){
  var f=exprCompile(input), g=exprCompile(answer); if(!f||!g) return false;
  var hits=0;
  for(var trial=0;trial<8;trial++){
    var env={}; 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('').forEach(function(c){ env[c]=1.3+Math.random()*4.1; });
    var x=f(env), y=g(env);
    if(!isFinite(x)||!isFinite(y)) continue;
    if(Math.abs(x-y)>1e-7*Math.max(1,Math.abs(x),Math.abs(y))) return false;
    hits++;
  }
  return hits>=3;
}
function wordsNorm(s){ return String(s||'').toLowerCase().replace(/\band\b/g,' ').replace(/[^a-z]/g,''); }
function listNorm(s){ return (String(s||'').replace(/[−–—]/g,'-').replace(/(\d) (?=\d{3}\b)/g,'$1').match(/-?\d+(\.\d+)?/g)||[]).join(','); }
function powNorm(s){ return String(s||'').toLowerCase().replace(/\s+/g,'').replace(/[×∗*·.]/g,'x').replace(/[⁰¹²³⁴⁵⁶⁷⁸⁹]+/g,function(m){ return '^'+m.split('').map(function(c){ return '⁰¹²³⁴⁵⁶⁷⁸⁹'.indexOf(c); }).join(''); }).replace(/\^1(?!\d)/g,''); }
function fr(s){ return String(s).replace(/\{(\d+) (\d+)\/(\d+)\}/g,'<span class="mx">$1<span class="fq"><span>$2</span><span>$3</span></span></span>').replace(/\{(\d+)\/(\d+)\}/g,'<span class="fq"><span>$1</span><span>$2</span></span>'); }
function gcdI(a,b){ a=Math.abs(a); b=Math.abs(b); while(b){ var t=a%b; a=b; b=t; } return a; }
function fracParse(s){ s=String(s===undefined||s===null?'':s).trim().replace(/[\u2044\u2215\u00f7]/g,'/').replace(/\s+/g,' ').replace(/\s*\/\s*/g,'/'); var m;
  if((m=s.match(/^(-?\d+)$/))) return {v:+m[1],form:'int',n:+m[1],d:1};
  if((m=s.match(/^(-?\d+)\/(\d+)$/))){ if(+m[2]===0) return null; return {v:m[1]/m[2],form:'frac',n:+m[1],d:+m[2]}; }
  if((m=s.match(/^(\d+)(?: |\+|-)(\d+)\/(\d+)$/))){ if(+m[3]===0) return null; return {v:+m[1]+m[2]/m[3],form:'mixed',w:+m[1],n:+m[2],d:+m[3]}; }
  if((m=s.match(/^-?\d*\.\d+$/))) return {v:parseFloat(s),form:'dec'};
  return null; }
function fracOK(mode,input,answer){
  if(mode==='flist'){ var A=String(input).split(/[,;]/).map(function(x){return x.trim();}).filter(Boolean), K=String(answer).split(/[,;]/).map(function(x){return x.trim();});
    if(A.length!==K.length) return false; return A.every(function(x,i){ var a=fracParse(x), k=fracParse(K[i]); return a&&k&&Math.abs(a.v-k.v)<1e-9; }); }
  var a=fracParse(input), k=fracParse(answer); if(!a||!k) return false;
  var same=Math.abs(a.v-k.v)<1e-9; if(!same) return false;
  if(mode==='fv') return true;
  if(mode==='dec') return a.form==='dec'||a.form==='int';
  if(mode==='fe') return (a.form==='frac'&&k.form==='frac'&&a.n===k.n&&a.d===k.d)||(a.form==='int'&&k.form==='int');
  if(mode==='fi') return a.form==='frac'||(a.form==='int'&&k.form==='int');
  if(mode==='fl') return a.form==='int'||(a.form==='frac'&&gcdI(a.n,a.d)===1)||(a.form==='mixed'&&a.n<a.d&&gcdI(a.n,a.d)===1);
  if(mode==='fm') return a.form==='int'||(a.form==='mixed'&&a.n>0&&a.n<a.d&&gcdI(a.n,a.d)===1);
  return false; }
function answerMatches(input,answer,accept,expr){
  if(expr==='dec'||expr==='fv'||expr==='fe'||expr==='fl'||expr==='fm'||expr==='fi'||expr==='flist'){ if(input===undefined||input===null||String(input).trim()==='') return false; return fracOK(expr,input,answer); }
  if(input===undefined||input===null||String(input).trim()==='') return false;
  if(expr==='words'){ return [answer].concat(accept||[]).some(function(a){ if(/\d/.test(a)) return norm(a)===norm(input); var w=wordsNorm(a); return w!=='' && w===wordsNorm(input); }); }
  if(expr==='list'){ return [answer].concat(accept||[]).some(function(a){ return listNorm(a)===listNorm(input); }); }
  if(expr==='pow'){ return [answer].concat(accept||[]).some(function(a){ return powNorm(a)===powNorm(input); }); }
  if(expr==='time'){ var tn=function(x){ var t=String(x).toLowerCase().replace(/[\s.]/g,'').replace(/[:h]/g,''); if(/^\d{3}(am|pm)?$/.test(t)) t='0'+t; return t; }; return [answer].concat(accept||[]).some(function(a){ return tn(a)===tn(input); }); }
  if(expr==='glist'){ var gn=function(x){ return String(x).toUpperCase().split(/[\s,;]+/).filter(Boolean).sort().join(','); }; return [answer].concat(accept||[]).some(function(a){ return gn(a)===gn(input); }); }
  if(expr==='coord'){ var cn=function(x){ return String(x).replace(/[\u2212\u2013\u2014]/g,'-').replace(/[\s()\[\]]/g,''); }; return [answer].concat(accept||[]).some(function(a){ return cn(a)===cn(input); }); }
  if(expr==='dlist'){ var dn=function(x){ return (String(x).replace(/[−–—]/g,'-').match(/-?\d*\.?\d+/g)||[]).map(Number).join(','); }; return [answer].concat(accept||[]).some(function(a){ return dn(a)===dn(input); }); }
  if(expr==='set'){ var sn=function(x){ return (String(x).match(/-?\d+/g)||[]).map(Number).sort(function(a,b){return a-b;}).join(','); }; return [answer].concat(accept||[]).some(function(a){ return sn(a)===sn(input); }); }
  if(expr==='primes'){ var pn=function(x){ var t=powNorm(x).replace(/[^0-9x^]/g,''); var out=[]; t.split('x').forEach(function(tok){ if(!tok) return; var m=tok.split('^'); var b=+m[0], e=m[1]?+m[1]:1; for(var i=0;i<e;i++) out.push(b); }); return out.sort(function(a,b){return a-b;}).join(','); }; return [answer].concat(accept||[]).some(function(a){ return pn(a)===pn(input); }); }
  if(expr==='angle'){ var an=function(x){ return String(x).toUpperCase().replace(/[^A-Z]/g,''); }; var k=an(answer), v=an(input); return v===k || v===k.split('').reverse().join(''); }
  if(expr==='trans'){ var tv=function(x){ var s=String(x).toLowerCase().replace(/[−–—]/g,'-').replace(/\b(units?|squares?|and|then|steps?|to|the|a)\b/g,' ').replace(/[,;.]/g,' '); var re=/(\d+)\s*(right|left|up|down|r|l|u|d)\b/g, m, dx=0, dy=0, n=0; while((m=re.exec(s))){ var v=+m[1], d=m[2][0]; if(d==='r')dx+=v; else if(d==='l')dx-=v; else if(d==='u')dy+=v; else dy-=v; n++; } if(!n||s.replace(re,'').replace(/\s+/g,'')!=='') return null; return dx+','+dy; }; var ti=tv(input); return ti!==null && [answer].concat(accept||[]).some(function(a){ return tv(a)===ti; }); }
  if(expr==='rot'){ var rv=function(x){ var s=String(x).toLowerCase().replace(/quarter[\s-]*turn/g,'90').replace(/half[\s-]*turn/g,'180').replace(/three[\s-]*quarter[\s-]*turn/g,'270'); var m=s.match(/\b(90|180|270)\b/); if(!m) return null; var a=+m[1]; if(a===180) return '180'; var dir=/anti|counter|acw|ccw/.test(s)?-1:(/clockwise|\bcw\b/.test(s)?1:0); if(!dir) return null; return String(((a*dir)%360+360)%360); }; var ri=rv(input); return ri!==null && ri===rv(answer); }
  if(expr==='expanded'){ var f=function(x){ return String(x).replace(/\s+/g,'').replace(/,/g,''); }; return [answer].concat(accept||[]).some(function(a){ return f(a)===f(input); }); }
  if(expr===true && input!==undefined && input!==null && String(input).trim()!==''){
    if(exprEqual(input,answer)) return true;
    var alt=String(input).replace(/\/\s*(\d+(?:\.\d+)?)\s*([a-zA-Z(])/g,'/$1*$2'); if(alt!==String(input) && exprEqual(alt,answer)) return true;
    return (accept||[]).some(function(a){ return exprEqual(input,a); });
  }
  if(input===undefined||input===null||String(input).trim()==='') return false;
  var n1=parseNum(input), n2=parseNum(answer);
  if(n1!==null && n2!==null){ if(Math.abs(n1-n2)<1e-3) return true; return (accept||[]).some(function(a){ var n3=parseNum(a); return n3!==null && Math.abs(n1-n3)<1e-3; }); }
  var cands=[answer].concat(accept||[]); var ni=norm(input);
  for(var i=0;i<cands.length;i++){ if(norm(cands[i])===ni) return true; }
  return false;
}
function esc(s){ return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;'); }

/* ================= DATA ================= */
var AR_OPTIONS = ["Both A and R are true, and R is the correct explanation of A","Both A and R are true, but R is NOT the correct explanation of A","A is true but R is false","A is false but R is true"];
var THEORY = "<div class=\"hub\"><div class=\"hub-card\"><h3>📖 Theory notes</h3><p>Notes for each CED topic, with the learning objectives, equations, force diagrams, graphs and worked examples.</p><div class=\"hub-btns\"><button class=\"hub-btn\" data-jump=\"n51\">Topic 5.1</button><button class=\"hub-btn\" data-jump=\"n52\">Topic 5.2</button><button class=\"hub-btn\" data-jump=\"n53\">Topic 5.3</button><button class=\"hub-btn\" data-jump=\"n54\">Topic 5.4</button><button class=\"hub-btn\" data-jump=\"n55\">Topic 5.5</button><button class=\"hub-btn\" data-jump=\"n56\">Topic 5.6</button></div></div><div class=\"hub-card\"><h3>🎯 Practice by learning objective</h3><p>One practice sheet per learning objective: multiple choice first, then step-by-step blanks. 🧮 and 📈 appear where a calculator or graph helps.</p><div class=\"hub-btns\"><div class=\"hub-grp\">Topic 5.1 · Rotational Kinematics</div><button class=\"hub-btn\" data-go=\"s1\">5.1.A (i) · Angular quantities and equations</button><button class=\"hub-btn\" data-go=\"s2\">5.1.A (ii) · Graphs of rotational motion</button><div class=\"hub-grp\">Topic 5.2 · Connecting Linear and Rotational Motion</div><button class=\"hub-btn\" data-go=\"s3\">5.2.A · Linear motion of a rotating point</button><div class=\"hub-grp\">Topic 5.3 · Torque</div><button class=\"hub-btn\" data-go=\"s4\">5.3.A · Identifying torques</button><button class=\"hub-btn\" data-go=\"s5\">5.3.B · Describing torques</button><div class=\"hub-grp\">Topic 5.4 · Rotational Inertia</div><button class=\"hub-btn\" data-go=\"s6\">5.4.A · Rotational inertia</button><button class=\"hub-btn\" data-go=\"s7\">5.4.B · Parallel-axis theorem</button><div class=\"hub-grp\">Topic 5.5 · Rotational Equilibrium and Newton's First Law in Rotational Form</div><button class=\"hub-btn\" data-go=\"s8\">5.5.A (i) · Balanced torques</button><button class=\"hub-btn\" data-go=\"s9\">5.5.A (ii) · Static equilibrium</button><div class=\"hub-grp\">Topic 5.6 · Newton's Second Law in Rotational Form</div><button class=\"hub-btn\" data-go=\"s10\">5.6.A (i) · Net torque and angular acceleration</button><button class=\"hub-btn\" data-go=\"s11\">5.6.A (ii) · Linked rotation and translation</button></div></div><div class=\"hub-card\"><h3>📝 Unit test</h3><p>Four tests, one per category. Take them in Quiz mode, then open the report for your pie charts.</p><div class=\"hub-btns\"><button class=\"hub-btn\" data-go=\"s12\">Test A · Knowing and understanding</button><button class=\"hub-btn\" data-go=\"s13\">Test B · Investigating patterns</button><button class=\"hub-btn\" data-go=\"s14\">Test C · Communicating</button><button class=\"hub-btn\" data-go=\"s15\">Test D · Applying physics in real-life contexts</button><button class=\"hub-btn primary\" data-go=\"report\">📊 View my report</button></div></div></div><style>.hub-grp{width:100%;font:700 12px 'Source Sans 3',sans-serif;color:var(--ink-soft);text-transform:uppercase;letter-spacing:.05em;margin-top:6px;}</style><section class=\"note\" id=\"nintro\"><h2>About this unit</h2><p>Unit 5 of AP Physics 1 moves from objects (points) to <b>rigid systems</b> that can turn. It describes rotation with angles, explains what makes things start or stop turning (<b>torque</b>) and how hard they are to turn (<b>rotational inertia</b>). The practice tabs follow the College Board course framework, one tab for each learning objective (5.1.A, 5.5.A and 5.6.A are large, so each has two tabs).</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Topic</th><th>Learning objective</th><th>Practice tab</th></tr><tr><td>5.1</td><td><b>5.1.A</b> Describe the rotation of a system with respect to time using angular displacement, angular velocity, and angular acceleration.</td><td>5.1.A (i)</td></tr><tr><td>5.1</td><td><b>5.1.A</b> Describe the rotation of a system with respect to time using graphs of angular position, angular velocity and angular acceleration.</td><td>5.1.A (ii)</td></tr><tr><td>5.2</td><td><b>5.2.A</b> Describe the linear motion of a point on a rotating rigid system that corresponds to the rotational motion of that point, and vice versa.</td><td>5.2.A</td></tr><tr><td>5.3</td><td><b>5.3.A</b> Identify the torques exerted on a rigid system.</td><td>5.3.A</td></tr><tr><td>5.3</td><td><b>5.3.B</b> Describe the torques exerted on a rigid system.</td><td>5.3.B</td></tr><tr><td>5.4</td><td><b>5.4.A</b> Describe the rotational inertia of a rigid system relative to a given axis of rotation.</td><td>5.4.A</td></tr><tr><td>5.4</td><td><b>5.4.B</b> Describe the rotational inertia of a rigid system rotating about an axis that does not pass through the system's center of mass.</td><td>5.4.B</td></tr><tr><td>5.5</td><td><b>5.5.A</b> Describe the conditions under which a system's angular velocity remains constant.</td><td>5.5.A (i)</td></tr><tr><td>5.5</td><td><b>5.5.A</b> Describe the conditions under which a system's angular velocity remains constant (forces and torques together).</td><td>5.5.A (ii)</td></tr><tr><td>5.6</td><td><b>5.6.A</b> Describe the conditions under which a system's angular velocity changes.</td><td>5.6.A (i)</td></tr><tr><td>5.6</td><td><b>5.6.A</b> Describe the conditions under which a system's angular velocity changes, for systems that both rotate and translate.</td><td>5.6.A (ii)</td></tr></table></div><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Quantity or law</th><th>Equation</th></tr><tr><td>average angular velocity / acceleration</td><td class=\"mono\">ω<sub>avg</sub>&nbsp;=&nbsp;Δθ ÷ Δt, α<sub>avg</sub>&nbsp;=&nbsp;Δω ÷ Δt</td></tr><tr><td>constant α</td><td class=\"mono\">ω = ω₀ + αt<br>θ = θ₀ + ω₀t + ½αt²<br>ω² = ω₀² + 2α(θ − θ₀)</td></tr><tr><td>point at radius r</td><td class=\"mono\">s&nbsp;=&nbsp;rθ, v&nbsp;=&nbsp;rω, a<sub>T</sub>&nbsp;=&nbsp;rα</td></tr><tr><td>torque</td><td class=\"mono\">τ&nbsp;=&nbsp;rF sin θ&nbsp;=&nbsp;r<sub>⊥</sub>F</td></tr><tr><td>rotational inertia</td><td class=\"mono\">I&nbsp;=&nbsp;Σmr², I&nbsp;=&nbsp;I<sub>cm</sub> + Md²</td></tr><tr><td>Newton's laws in rotational form</td><td class=\"mono\">Στ&nbsp;=&nbsp;0 ⇔ ω constant; α&nbsp;=&nbsp;Στ ÷ I</td></tr></table></div><p>Angles are in <b>radians</b> (1 rev = 2π rad = 360°). Use <b>g ≈ 10 m/s²</b> and ignore friction and air resistance unless told otherwise. Counterclockwise (ccw) is taken as positive unless a question says otherwise. Rounded answers are accepted within about 1%.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Test</th><th>Category</th><th>What it checks</th></tr><tr><td>A</td><td>Knowing and understanding</td><td>Definitions, equations and diagrams used correctly.</td></tr><tr><td>B</td><td>Investigating patterns</td><td>Relationships in data tables and graphs; predicting.</td></tr><tr><td>C</td><td>Communicating</td><td>Units, signs, significant figures, representations, spotting errors.</td></tr><tr><td>D</td><td>Applying physics in real-life contexts</td><td>Tools, cranes, fans and wells; judging reasonableness.</td></tr></table></div><p><b>Tools:</b> every blank opens an on-screen keyboard (⌨️ brings it back). 🧮 opens a scientific calculator (degrees; Insert puts the result in the blank). 📈 opens a Desmos graph set up for the question.</p></section><section class=\"note\" id=\"n51\"><h2>Topic 5.1 · Rotational Kinematics</h2><p class=\"lt\"><b>Learning objectives</b></p><ul class=\"lt\"><li><b>5.1.A</b> Describe the rotation of a system with respect to time using angular displacement, angular velocity, and angular acceleration.</li></ul><p>A <b>rigid system</b> keeps its shape, but it cannot be modelled as a single object (a point): different parts of a turning wheel move in different directions. We describe its rotation about a fixed axis with <b>angular</b> quantities.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Linear</th><th>Angular</th><th>Unit</th></tr><tr><td>position x</td><td>angular position θ</td><td>rad</td></tr><tr><td>velocity v = Δx ÷ Δt</td><td>angular velocity ω = Δθ ÷ Δt</td><td>rad/s</td></tr><tr><td>acceleration a = Δv ÷ Δt</td><td>angular acceleration α = Δω ÷ Δt</td><td>rad/s²</td></tr></table></div><p>One direction of rotation, usually <b>counterclockwise</b>, is taken as positive; clockwise is then negative. If ω and α have the same sign the rotation speeds up; opposite signs mean it slows down. For <b>constant α</b> the three rotational kinematic equations (see the table at the top) have exactly the same form as the linear ones.</p><p>1 revolution = 2π rad. To change revolutions per minute (rpm) to rad/s, multiply by 2π and divide by 60.</p><div class=\"ex\"><div class=\"exh\">Worked example 1 · Spinning up</div><div class=\"exl\">A grinding wheel starts from rest with α = 4.0 rad/s² for 5.0 s.<br>ω = 0 + 4.0 × 5.0 = <b>20 rad/s</b>; θ = ½ × 4.0 × 5.0² = <b>50 rad</b> ≈ 50 ÷ 2π ≈ 8.0 revolutions.</div></div><h4>Graphs</h4><p>Rotational graphs work like motion graphs: the slope of θ–t is ω, the slope of ω–t is α, the area under ω–t is Δθ and the area under α–t is Δω.</p><div class=\"fig\"><svg class=\"figsvg\" viewBox=\"0 0 320 230\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line style=\"stroke:var(--rule)\" x1=\"46.0\" y1=\"196\" x2=\"46.0\" y2=\"18\"/><text class=\"po\" x=\"46.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">0</text><line style=\"stroke:var(--rule)\" x1=\"77.8\" y1=\"196\" x2=\"77.8\" y2=\"18\"/><text class=\"po\" x=\"77.8\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><line style=\"stroke:var(--rule)\" x1=\"109.5\" y1=\"196\" x2=\"109.5\" y2=\"18\"/><text class=\"po\" x=\"109.5\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">4</text><line style=\"stroke:var(--rule)\" x1=\"141.2\" y1=\"196\" x2=\"141.2\" y2=\"18\"/><text class=\"po\" x=\"141.2\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">6</text><line style=\"stroke:var(--rule)\" x1=\"173.0\" y1=\"196\" x2=\"173.0\" y2=\"18\"/><text class=\"po\" x=\"173.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">8</text><line style=\"stroke:var(--rule)\" x1=\"204.8\" y1=\"196\" x2=\"204.8\" y2=\"18\"/><text class=\"po\" x=\"204.8\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">10</text><line style=\"stroke:var(--rule)\" x1=\"236.5\" y1=\"196\" x2=\"236.5\" y2=\"18\"/><text class=\"po\" x=\"236.5\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">12</text><line style=\"stroke:var(--rule)\" x1=\"268.2\" y1=\"196\" x2=\"268.2\" y2=\"18\"/><text class=\"po\" x=\"268.2\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">14</text><line style=\"stroke:var(--rule)\" x1=\"300.0\" y1=\"196\" x2=\"300.0\" y2=\"18\"/><text class=\"po\" x=\"300.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">16</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"196.0\" x2=\"300\" y2=\"196.0\"/><text class=\"po\" x=\"40.0\" y=\"196.0\" text-anchor=\"end\" dominant-baseline=\"middle\">0</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"170.6\" x2=\"300\" y2=\"170.6\"/><text class=\"po\" x=\"40.0\" y=\"170.6\" text-anchor=\"end\" dominant-baseline=\"middle\">2</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"145.1\" x2=\"300\" y2=\"145.1\"/><text class=\"po\" x=\"40.0\" y=\"145.1\" text-anchor=\"end\" dominant-baseline=\"middle\">4</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"119.7\" x2=\"300\" y2=\"119.7\"/><text class=\"po\" x=\"40.0\" y=\"119.7\" text-anchor=\"end\" dominant-baseline=\"middle\">6</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"94.3\" x2=\"300\" y2=\"94.3\"/><text class=\"po\" x=\"40.0\" y=\"94.3\" text-anchor=\"end\" dominant-baseline=\"middle\">8</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"68.9\" x2=\"300\" y2=\"68.9\"/><text class=\"po\" x=\"40.0\" y=\"68.9\" text-anchor=\"end\" dominant-baseline=\"middle\">10</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"43.4\" x2=\"300\" y2=\"43.4\"/><text class=\"po\" x=\"40.0\" y=\"43.4\" text-anchor=\"end\" dominant-baseline=\"middle\">12</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"18.0\" x2=\"300\" y2=\"18.0\"/><text class=\"po\" x=\"40.0\" y=\"18.0\" text-anchor=\"end\" dominant-baseline=\"middle\">14</text><line class=\"ln\" x1=\"46.0\" y1=\"196.0\" x2=\"308.0\" y2=\"196.0\" marker-end=\"url(#ah)\"/><line class=\"ln\" x1=\"46.0\" y1=\"196.0\" x2=\"46.0\" y2=\"10.0\" marker-end=\"url(#ah)\"/><text class=\"lb\" x=\"300.0\" y=\"188.0\" text-anchor=\"end\" dominant-baseline=\"middle\">t (s)</text><text class=\"lb\" x=\"52.0\" y=\"12.0\" text-anchor=\"start\" dominant-baseline=\"middle\">ω (rad/s)</text><polyline fill=\"none\" style=\"stroke:var(--danger);stroke-width:2.2\" points=\"46.0,196.0 109.5,43.4 204.8,43.4 268.2,196.0\"/><circle cx=\"46.0\" cy=\"196.0\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"109.5\" cy=\"43.4\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"204.8\" cy=\"43.4\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"268.2\" cy=\"196.0\" r=\"3\" style=\"fill:var(--danger)\"/></svg></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · Reading an ω–t graph</div><div class=\"exl\">0–4 s: α = 12 ÷ 4 = 3 rad/s². 4–10 s: α = 0. 10–14 s: α = −3 rad/s².<br>Area = ½(4)(12) + 6(12) + ½(4)(12) = <b>120 rad</b> ≈ 19 revolutions.</div></div><div class=\"keybox\"><b>Watch the units and signs.</b> The rotational equations need radians, not degrees or revolutions. A negative ω means clockwise; a negative α does not always mean “slowing down” — compare it with the sign of ω.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s1\">Practise 5.1.A (i) →</button><button class=\"hub-btn primary\" data-go=\"s2\">Practise 5.1.A (ii) →</button></div></section><section class=\"note\" id=\"n52\"><h2>Topic 5.2 · Connecting Linear and Rotational Motion</h2><p class=\"lt\"><b>Learning objectives</b></p><ul class=\"lt\"><li><b>5.2.A</b> Describe the linear motion of a point on a rotating rigid system that corresponds to the rotational motion of that point, and vice versa.</li></ul><p>Every point of a rigid rotating system has the <b>same</b> angular displacement, angular velocity and angular acceleration. But a point farther from the axis travels a longer arc, so its <b>linear</b> quantities are bigger:</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Arc length</th><th>Linear (tangential) speed</th><th>Tangential acceleration</th></tr><tr><td class=\"mono\">s&nbsp;=&nbsp;rθ</td><td class=\"mono\">v&nbsp;=&nbsp;rω</td><td class=\"mono\">a<sub>T</sub>&nbsp;=&nbsp;rα</td></tr></table></div><p>These only work with θ in radians. The linear velocity of a point is tangent to its circular path.</p><div class=\"fig\"><svg class=\"figsvg\" viewBox=\"0 0 260 180\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><circle class=\"sh3\" cx=\"130.0\" cy=\"90.0\" r=\"60\"/><circle class=\"pt\" cx=\"130.0\" cy=\"90.0\" r=\"2.8\"/><text class=\"lb\" x=\"121.0\" y=\"100.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">O</text><line class=\"hid\" x1=\"130.0\" y1=\"90.0\" x2=\"109.2\" y2=\"78.0\"/><circle class=\"pt\" cx=\"109.2\" cy=\"78.0\" r=\"2.8\"/><text class=\"lb\" x=\"98.8\" y=\"72.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">A</text><line class=\"hid\" x1=\"130.0\" y1=\"90.0\" x2=\"184.4\" y2=\"115.4\"/><circle class=\"pt\" cx=\"184.4\" cy=\"115.4\" r=\"2.8\"/><text class=\"lb\" x=\"195.3\" y=\"120.4\" text-anchor=\"middle\" dominant-baseline=\"middle\">B</text><path class=\"arm\" d=\"M199.5,71.4 A72,72 0 0 0 154.6,22.3\" marker-end=\"url(#ah)\"/></svg></div><div class=\"ex\"><div class=\"exh\">Worked example 1 · Two points on one disc</div><div class=\"exl\">A is 0.10 m and B is 0.25 m from the axis of a disc turning at 8.0 rad/s.<br>Both have ω = 8.0 rad/s. v<sub>A</sub> = 0.10 × 8.0 = <b>0.80 m/s</b>; v<sub>B</sub> = 0.25 × 8.0 = <b>2.0 m/s</b>.</div></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · Rope on a drum</div><div class=\"exl\">A rope winds onto a drum of radius 0.15 m while the drum turns through 20 rad.<br>Length wound = rθ = 0.15 × 20 = <b>3.0 m</b>.</div></div><div class=\"keybox\"><b>Same ω, different v.</b> On a merry-go-round a child at the rim and one near the centre complete a turn in the same time, but the rim child moves much faster.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s3\">Practise 5.2.A →</button></div></section><section class=\"note\" id=\"n53\"><h2>Topic 5.3 · Torque</h2><p class=\"lt\"><b>Learning objectives</b></p><ul class=\"lt\"><li><b>5.3.A</b> Identify the torques exerted on a rigid system.</li><li><b>5.3.B</b> Describe the torques exerted on a rigid system.</li></ul><p>A <b>torque</b> is the turning effect of a force about an axis. Only the component of the force <b>perpendicular</b> to the position vector r (from the axis to the point where the force acts) produces torque:</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Form</th><th>Equation</th></tr><tr><td>angle form</td><td class=\"mono\">τ&nbsp;=&nbsp;rF sin θ</td></tr><tr><td>perpendicular force</td><td class=\"mono\">τ&nbsp;=&nbsp;rF<sub>⊥</sub></td></tr><tr><td>lever arm form</td><td class=\"mono\">τ&nbsp;=&nbsp;r<sub>⊥</sub>F</td></tr></table></div><p>θ is the angle between r and F. The <b>lever arm</b> r<sub>⊥</sub> is the perpendicular distance from the axis to the <b>line of action</b> of the force. A force whose line of action passes through the axis exerts no torque. Torque is measured in N·m and is described as clockwise or counterclockwise.</p><p>A <b>force diagram</b> is like a free-body diagram, but each force is drawn <b>at the point where it is exerted</b> on the rigid system, so you can see its lever arm. The weight acts at the centre of mass.</p><div class=\"fig\"><svg class=\"figsvg\" viewBox=\"0 0 320 130\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect class=\"sh\" x=\"32.0\" y=\"41.0\" width=\"256.0\" height=\"10\"/><line class=\"arm\" x1=\"288.0\" y1=\"51.0\" x2=\"311.0\" y2=\"90.8\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"317.5\" y=\"101.2\" text-anchor=\"middle\" dominant-baseline=\"middle\">F</text><circle cx=\"32.0\" cy=\"46\" r=\"4\" style=\"fill:var(--ink)\"/><text class=\"lb\" x=\"32.0\" y=\"30.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">axis</text></svg></div><div class=\"ex\"><div class=\"exh\">Worked example 1 · Spanner at an angle</div><div class=\"exl\">F = 50 N pulls at the end of a 0.30 m spanner at 60° to the handle (as drawn).<br>τ = rF sin θ = 0.30 × 50 × sin 60° ≈ <b>13 N·m</b>, clockwise. At 90° it would be 15 N·m.</div></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · Net torque</div><div class=\"exl\">A wheel of radius 0.50 m has 10 N pulling ccw at its rim and 6 N pulling cw at its rim.<br>Στ = (10 − 6) × 0.50 = <b>2.0 N·m counterclockwise</b>.</div></div><div class=\"keybox\"><b>Long spanner, same force, more torque.</b> Pushing a door near its hinge, or along the door towards the hinge, barely turns it: the lever arm is small or zero.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s4\">Practise 5.3.A →</button><button class=\"hub-btn primary\" data-go=\"s5\">Practise 5.3.B →</button></div></section><section class=\"note\" id=\"n54\"><h2>Topic 5.4 · Rotational Inertia</h2><p class=\"lt\"><b>Learning objectives</b></p><ul class=\"lt\"><li><b>5.4.A</b> Describe the rotational inertia of a rigid system relative to a given axis of rotation.</li><li><b>5.4.B</b> Describe the rotational inertia of a rigid system rotating about an axis that does not pass through the system's center of mass.</li></ul><p><b>Rotational inertia</b> I measures how hard it is to change a rigid system's rotation. It depends on the <b>mass</b> and on <b>how far the mass is from the axis</b>:</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>System</th><th>Rotational inertia</th></tr><tr><td>point mass m at distance r</td><td class=\"mono\">I&nbsp;=&nbsp;mr²</td></tr><tr><td>collection of masses</td><td class=\"mono\">I&nbsp;=&nbsp;Σm<sub>i</sub>r<sub>i</sub>²</td></tr><tr><td>hoop, all mass at R (given)</td><td class=\"mono\">I&nbsp;=&nbsp;MR²</td></tr><tr><td>solid disc about its axis (given)</td><td class=\"mono\">I&nbsp;=&nbsp;½MR²</td></tr><tr><td>uniform rod about its centre (given)</td><td class=\"mono\">I&nbsp;=&nbsp;ML² ÷ 12</td></tr><tr><td>parallel-axis theorem</td><td class=\"mono\">I&nbsp;=&nbsp;I<sub>cm</sub> + Md²</td></tr></table></div><p>The unit is kg·m². Formulas for extended objects are always given on the exam; you should be able to add up I for a few point masses. The same object has <b>different</b> I about different axes, and, of all axes pointing in the same direction, I is <b>smallest</b> about the one through the centre of mass. For an axis parallel to one through the centre of mass, a distance d away, I = I<sub>cm</sub> + Md².</p><div class=\"fig\"><svg class=\"figsvg\" viewBox=\"0 0 320 120\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line x1=\"32.0\" y1=\"64\" x2=\"288.0\" y2=\"64\" style=\"stroke:var(--ink);stroke-width:3\"/><line x1=\"160.0\" y1=\"24\" x2=\"160.0\" y2=\"92\" style=\"stroke:var(--danger);stroke-width:1.6;stroke-dasharray:5 4\"/><text class=\"al\" x=\"160.0\" y=\"16.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">axis through centre</text><line x1=\"32.0\" y1=\"24\" x2=\"32.0\" y2=\"92\" style=\"stroke:var(--danger);stroke-width:1.6;stroke-dasharray:5 4\"/><text class=\"al\" x=\"32.0\" y=\"16.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">end axis</text><circle cx=\"32.0\" cy=\"64\" r=\"8\" style=\"fill:var(--accent-text);stroke:var(--ink);stroke-width:1\"/><text class=\"lb\" x=\"32.0\" y=\"46.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">m</text><circle cx=\"160.0\" cy=\"64\" r=\"8\" style=\"fill:var(--accent-text);stroke:var(--ink);stroke-width:1\"/><text class=\"lb\" x=\"160.0\" y=\"46.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">m</text><circle cx=\"288.0\" cy=\"64\" r=\"8\" style=\"fill:var(--accent-text);stroke:var(--ink);stroke-width:1\"/><text class=\"lb\" x=\"288.0\" y=\"46.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">m</text></svg></div><div class=\"ex\"><div class=\"exh\">Worked example 1 · Three masses on a rod</div><div class=\"exl\">Three 0.50 kg masses sit at 0, 0.40 m and 0.80 m on a light rod.<br>About the centre: I = 0.50(0.40²) + 0 + 0.50(0.40²) = <b>0.16 kg·m²</b>.<br>About the end: I = 0 + 0.50(0.40²) + 0.50(0.80²) = <b>0.40 kg·m²</b>.</div></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · Parallel-axis theorem</div><div class=\"exl\">A uniform rod, M = 1.2 kg, L = 1.0 m, has I<sub>cm</sub> = ML² ÷ 12 = 0.10 kg·m².<br>About one end (d = 0.50 m): I = 0.10 + 1.2 × 0.50² = <b>0.40 kg·m²</b> (= ML² ÷ 3).</div></div><div class=\"keybox\"><b>Mass is not the whole story.</b> Doubling a mass doubles its I, but doubling its distance from the axis multiplies I by four.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s6\">Practise 5.4.A →</button><button class=\"hub-btn primary\" data-go=\"s7\">Practise 5.4.B →</button></div></section><section class=\"note\" id=\"n55\"><h2>Topic 5.5 · Rotational Equilibrium and Newton's First Law in Rotational Form</h2><p class=\"lt\"><b>Learning objectives</b></p><ul class=\"lt\"><li><b>5.5.A</b> Describe the conditions under which a system's angular velocity remains constant.</li></ul><p><b>Rotational equilibrium</b> means the net torque is zero: the clockwise torques balance the counterclockwise torques. Newton's first law in rotational form: <b>a rigid system's angular velocity stays constant (possibly zero) only if the net torque on it is zero</b>. If the torques are not balanced, ω must be changing.</p><p>Rotational and translational equilibrium are separate conditions. A non-spinning ball in free fall has zero net torque but a net force; two equal, opposite forces along different lines (a <b>couple</b>) give zero net force but a net torque. A system in <b>static equilibrium</b> needs both: ΣF = 0 and Στ = 0 about <b>any</b> axis.</p><div class=\"fig\"><svg class=\"figsvg\" viewBox=\"0 0 320 130\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect class=\"sh\" x=\"32.0\" y=\"43.0\" width=\"256.0\" height=\"10\"/><path class=\"sh3\" d=\"M160.0,53.0 L149.0,71.0 L171.0,71.0 Z\"/><line class=\"ln\" x1=\"32.0\" y1=\"53.0\" x2=\"32.0\" y2=\"78.0\"/><rect class=\"sh2\" x=\"12.0\" y=\"78.0\" width=\"40\" height=\"20\"/><text class=\"lb\" x=\"32.0\" y=\"88.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">40 kg</text><line class=\"ln\" x1=\"245.3\" y1=\"53.0\" x2=\"245.3\" y2=\"78.0\"/><rect class=\"sh2\" x=\"225.3\" y=\"78.0\" width=\"40\" height=\"20\"/><text class=\"lb\" x=\"245.3\" y=\"88.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">?</text><line class=\"ln\" x1=\"32.0\" y1=\"22.0\" x2=\"160.0\" y2=\"22.0\" marker-end=\"url(#ah)\" marker-start=\"url(#ahs)\"/><text class=\"lb\" x=\"96.0\" y=\"14.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1.5 m</text><line class=\"ln\" x1=\"160.0\" y1=\"22.0\" x2=\"245.3\" y2=\"22.0\" marker-end=\"url(#ah)\" marker-start=\"url(#ahs)\"/><text class=\"lb\" x=\"202.7\" y=\"14.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x</text></svg></div><div class=\"ex\"><div class=\"exh\">Worked example 1 · See-saw</div><div class=\"exl\">A 40 kg child sits 1.5 m from the pivot. Where must a 30 kg child sit on the other side?<br>400 × 1.5 = 300 × x, so x = <b>2.0 m</b>.</div></div><div class=\"fig\"><svg class=\"figsvg\" viewBox=\"0 0 260 210\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line x1=\"10\" y1=\"190\" x2=\"220\" y2=\"190\" style=\"stroke:var(--ink);stroke-width:2\"/><line x1=\"220\" y1=\"190\" x2=\"220\" y2=\"14\" style=\"stroke:var(--ink);stroke-width:2\"/><line x1=\"100\" y1=\"190\" x2=\"220\" y2=\"30\" style=\"stroke:var(--gold);stroke-width:7;stroke-linecap:round\"/><line class=\"arm\" x1=\"160.0\" y1=\"110.0\" x2=\"160.0\" y2=\"154.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"172.0\" y=\"150.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">W</text><line class=\"arm\" x1=\"220.0\" y1=\"30.0\" x2=\"176.0\" y2=\"30.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"170.0\" y=\"18.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">N₂</text><line class=\"arm\" x1=\"100.0\" y1=\"190.0\" x2=\"100.0\" y2=\"144.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"88.0\" y=\"140.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">N₁</text><line class=\"arm\" x1=\"100.0\" y1=\"190.0\" x2=\"140.0\" y2=\"190.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"144.0\" y=\"202.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">f</text></svg></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · Ladder against a smooth wall</div><div class=\"exl\">A uniform 5.0 m, 200 N ladder has its foot 3.0 m from the wall (top 4.0 m up).<br>Torques about the foot: N₂ × 4.0 = 200 × 1.5, so N₂ = <b>75 N</b>.<br>ΣF = 0: friction f = 75 N and N₁ = 200 N.</div></div><div class=\"keybox\"><b>Choose the axis cleverly.</b> Put the axis where an unknown force acts; its torque is then zero and it drops out of the equation. Any axis gives the same answer.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s8\">Practise 5.5.A (i) →</button><button class=\"hub-btn primary\" data-go=\"s9\">Practise 5.5.A (ii) →</button></div></section><section class=\"note\" id=\"n56\"><h2>Topic 5.6 · Newton's Second Law in Rotational Form</h2><p class=\"lt\"><b>Learning objectives</b></p><ul class=\"lt\"><li><b>5.6.A</b> Describe the conditions under which a system's angular velocity changes.</li></ul><p>When the net torque is not zero, the angular velocity changes. The angular acceleration is <b>proportional to the net torque</b> (and in the same direction) and <b>inversely proportional to the rotational inertia</b>:</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Law</th><th>Equation</th></tr><tr><td>Newton's second law (rotation)</td><td class=\"mono\">α&nbsp;=&nbsp;Στ ÷ I</td></tr><tr><td>Newton's second law (translation)</td><td class=\"mono\">a&nbsp;=&nbsp;ΣF ÷ m</td></tr><tr><td>string that does not slip</td><td class=\"mono\">a&nbsp;=&nbsp;Rα</td></tr></table></div><div class=\"ex\"><div class=\"exh\">Worked example 1 · Potter's wheel</div><div class=\"exl\">I = 1.5 kg·m², net torque 6.0 N·m from rest for 3.0 s.<br>α = 6.0 ÷ 1.5 = <b>4.0 rad/s²</b>; ω = 4.0 × 3.0 = <b>12 rad/s</b>.</div></div><p>To describe a system that both rotates and translates (a bucket on a rope round a drum, an Atwood machine with a heavy pulley) apply <b>ΣF = ma</b> to the moving block and <b>Στ = Iα</b> to the pulley <b>separately</b>, then link them with a = Rα.</p><div class=\"fig\"><svg class=\"figsvg\" viewBox=\"0 0 260 200\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line x1=\"70.0\" y1=\"10\" x2=\"190.0\" y2=\"10\" style=\"stroke:var(--ink);stroke-width:2.4\"/><line class=\"ln\" x1=\"130.0\" y1=\"10.0\" x2=\"130.0\" y2=\"52.0\"/><circle class=\"sh3\" cx=\"130.0\" cy=\"52\" r=\"26\"/><circle class=\"pt\" cx=\"130.0\" cy=\"52.0\" r=\"2.8\"/><line class=\"hid\" x1=\"130.0\" y1=\"52.0\" x2=\"111.5\" y2=\"70.5\"/><text class=\"lb\" x=\"86.0\" y=\"56.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">R</text><line class=\"ln\" x1=\"156.0\" y1=\"52.0\" x2=\"156.0\" y2=\"138.0\"/><rect class=\"sh2\" x=\"134.0\" y=\"138.0\" width=\"44\" height=\"28\"/><text class=\"lb\" x=\"156.0\" y=\"152.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">m</text></svg></div><div class=\"ex\"><div class=\"exh\">Worked example 2 · Bucket and drum</div><div class=\"exl\">A 2.0 kg bucket hangs from a rope on a drum: M = 4.0 kg, R = 0.20 m, I = ½MR² = 0.080 kg·m².<br>Bucket: 20 − T = 2.0a. Drum: TR = Iα = I a ÷ R, so T = (I ÷ R²)a = 2.0a.<br>20 = 4.0a, a = <b>5.0 m/s²</b>, T = 10 N, α = a ÷ R = 25 rad/s².</div></div><div class=\"keybox\"><b>The tension is less than the weight.</b> The block accelerates down, so mg &gt; T. With a heavy pulley the tensions on the two sides of an Atwood machine are different — that difference supplies the pulley's net torque.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s10\">Practise 5.6.A (i) →</button><button class=\"hub-btn primary\" data-go=\"s11\">Practise 5.6.A (ii) →</button></div></section><section class=\"note\" id=\"nsum\"><h2>Unit checklist</h2><ul><li><b>5.1.A (i)</b> Describe the rotation of a system with respect to time using angular displacement, angular velocity, and angular acceleration.</li><li><b>5.1.A (ii)</b> Describe the rotation of a system with respect to time using graphs of angular position, angular velocity and angular acceleration.</li><li><b>5.2.A</b> Describe the linear motion of a point on a rotating rigid system that corresponds to the rotational motion of that point, and vice versa.</li><li><b>5.3.A</b> Identify the torques exerted on a rigid system.</li><li><b>5.3.B</b> Describe the torques exerted on a rigid system.</li><li><b>5.4.A</b> Describe the rotational inertia of a rigid system relative to a given axis of rotation.</li><li><b>5.4.B</b> Describe the rotational inertia of a rigid system rotating about an axis that does not pass through the system's center of mass.</li><li><b>5.5.A (i)</b> Describe the conditions under which a system's angular velocity remains constant.</li><li><b>5.5.A (ii)</b> Describe the conditions under which a system's angular velocity remains constant (forces and torques together).</li><li><b>5.6.A (i)</b> Describe the conditions under which a system's angular velocity changes.</li><li><b>5.6.A (ii)</b> Describe the conditions under which a system's angular velocity changes, for systems that both rotate and translate.</li></ul><div class=\"hub-btns\" style=\"margin-top:10px\"><button class=\"hub-btn\" data-go=\"s12\">Test A</button><button class=\"hub-btn\" data-go=\"s13\">Test B</button><button class=\"hub-btn\" data-go=\"s14\">Test C</button><button class=\"hub-btn\" data-go=\"s15\">Test D</button><button class=\"hub-btn primary\" data-go=\"report\">📊 Report</button></div></section>";
var REPORT = [["s12", "A", "Knowing and understanding"], ["s13", "B", "Investigating patterns"], ["s14", "C", "Communicating"], ["s15", "D", "Applying physics in real-life contexts"]];
var SECTIONS = [{"id": "s1", "label": "5.1.A (i)", "sub": "Angular quantities and equations — LO 5.1.A: describe the rotation of a system with respect to time using angular displacement, angular velocity, and angular acceleration.", "slides": [{"kind": "mcq", "text": "A ceiling fan turns through exactly 3 revolutions. What is its angular displacement?", "opts": ["3 rad", "1080 rad", "6π rad (about 18.8 rad)", "3π rad (about 9.4 rad)"], "correct": 2, "tag": "", "sol": "1 rev = 2π rad, so 3 rev = 6π ≈ 18.8 rad (1080 is the answer in degrees, not radians)."}, {"kind": "mcq", "text": "A wheel's angular velocity rises from 4.0 rad/s to 16 rad/s in 3.0 s. What is its average angular acceleration?", "opts": ["4.0 rad/s²", "5.3 rad/s²", "12 rad/s²", "36 rad/s²"], "correct": 0, "tag": "", "sol": "α = Δω ÷ Δt = (16 − 4.0) ÷ 3.0 = 4.0 rad/s²."}, {"kind": "mcq", "text": "Why can a spinning bicycle wheel NOT be modelled as a single object (a point)?", "opts": ["Different parts of it move in different directions at the same moment", "It has no centre of mass", "A point cannot have mass", "It has zero velocity"], "correct": 0, "tag": "", "sol": "A rigid system that rotates has parts moving in different directions, so its size and shape matter."}, {"kind": "mcq", "text": "Counterclockwise is positive. A wheel is turning clockwise and speeding up. Which signs are correct?", "opts": ["ω positive, α negative", "ω negative, α positive", "ω positive, α positive", "ω negative, α negative"], "correct": 3, "tag": "", "sol": "Clockwise ⇒ ω < 0. Speeding up ⇒ α has the same sign as ω, so α < 0."}, {"kind": "mcq", "text": "A disc turns counterclockwise at 10 rad/s with α = −2.0 rad/s² (ccw positive). What is happening after 5.0 s?", "opts": ["It turns counterclockwise at 8.0 rad/s", "It is momentarily at rest", "It turns counterclockwise at 20 rad/s", "It turns clockwise at 10 rad/s"], "correct": 1, "tag": "", "sol": "ω = 10 + (−2.0)(5.0) = 0."}, {"kind": "mcq", "text": "A grinding wheel starts from rest with a constant α = 3.0 rad/s². Through what angle does it turn in 4.0 s?", "opts": ["6.0 rad", "12 rad", "24 rad", "48 rad"], "correct": 2, "tag": "", "sol": "θ = ½αt² = ½ × 3.0 × 16 = 24 rad.", "tools": ["calc"]}, {"kind": "mcq", "text": "A motor turns at 600 revolutions per minute (rpm). What is its angular velocity in rad/s?", "opts": ["about 62.8 rad/s", "about 3770 rad/s", "10 rad/s", "600 rad/s"], "correct": 0, "tag": "", "sol": "600 rpm = 10 rev/s; × 2π ≈ 62.8 rad/s. (3770 forgets to divide by 60.)", "tools": ["calc"]}, {"kind": "mcq", "text": "Which rotational equation is the analogue of v² = v₀² + 2aΔx?", "opts": ["ω = ω₀ + αt", "ω² = ω₀² + 2αΔθ", "Δθ = ω₀t + ½αt²", "α = Δω ÷ Δt"], "correct": 1, "tag": "", "sol": "Replace v by ω, a by α and Δx by Δθ."}, {"kind": "blank", "p": "A potter's wheel starts from rest and speeds up uniformly to 6.0 rad/s in 4.0 s.", "tag": "", "marks": "", "flat": [{"t": "Angular acceleration = __B1__ rad/s²", "a": {"B1": "1.5"}}, {"t": "Angle turned = __B1__ rad", "a": {"B1": "12"}}, {"t": "Number of revolutions = __B1__", "a": {"B1": "1.91"}, "expr": "approx"}], "sol": "α = 6.0 ÷ 4.0 = 1.5 rad/s².\nθ = ½(0 + 6.0)(4.0) = 12 rad.\n12 ÷ 2π ≈ 1.91 rev.", "tools": ["calc"]}, {"kind": "blank", "p": "A fan turning at 20 rad/s is switched off. It slows uniformly and stops after turning through 50 rad. Take its direction of rotation as positive.", "tag": "", "marks": "", "flat": [{"t": "α = __B1__ rad/s²", "a": {"B1": "-4"}}, {"t": "Time to stop = __B1__ s", "a": {"B1": "5"}}, {"t": "Angle turned in the first 2.0 s = __B1__ rad", "a": {"B1": "32"}}], "sol": "0 = 20² + 2α(50), α = −4 rad/s².\n0 = 20 − 4t, t = 5 s.\nθ = 20(2.0) − ½(4)(2.0)² = 32 rad.", "tools": ["calc"]}, {"kind": "blank", "p": "A wheel (ccw positive) is at θ = 3.0 rad at t = 0 and at θ = 11.0 rad at t = 2.0 s. Its angular velocity rises steadily from 2.0 rad/s to 6.0 rad/s in that time.", "tag": "", "marks": "", "flat": [{"t": "Average angular velocity = __B1__ rad/s", "a": {"B1": "4"}}, {"t": "Average angular acceleration = __B1__ rad/s²", "a": {"B1": "2"}}, {"t": "The wheel turns __B1__ (clockwise / counterclockwise).", "a": {"B1": "counterclockwise"}, "expr": "words", "accept": ["anticlockwise", "ccw"]}], "sol": "(11.0 − 3.0) ÷ 2.0 = 4.0 rad/s.\n(6.0 − 2.0) ÷ 2.0 = 2.0 rad/s².\nθ increases (ω > 0): counterclockwise.", "tools": ["calc"]}]}, {"id": "s2", "label": "5.1.A (ii)", "sub": "Graphs of rotational motion — LO 5.1.A: describe the rotation of a system with respect to time using graphs of angular position, angular velocity and angular acceleration.", "slides": [{"kind": "mcq", "text": "What does the slope of an angular velocity–time graph give?", "opts": ["rotational inertia", "angular displacement", "angular acceleration", "torque"], "correct": 2, "tag": "", "sol": "Slope = Δω ÷ Δt = α."}, {"kind": "mcq", "text": "What does the area under an angular velocity–time graph give?", "opts": ["angular velocity", "time", "angular acceleration", "angular displacement"], "correct": 3, "tag": "", "sol": "Area = ω × t = Δθ."}, {"kind": "mcq", "text": "What does the slope of an angular position–time graph give?", "opts": ["angular displacement", "angular acceleration", "angular velocity", "net torque"], "correct": 2, "tag": "", "sol": "Slope = Δθ ÷ Δt = ω."}, {"kind": "mcq", "text": "For the ω–t graph shown, during which interval is the angular acceleration zero?", "opts": ["0 to 5 s", "5 s to 11 s", "11 s to 13 s", "never"], "correct": 1, "tag": "", "sol": "The graph is horizontal (ω constant) from 5 s to 11 s.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 230\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line style=\"stroke:var(--rule)\" x1=\"46.0\" y1=\"196\" x2=\"46.0\" y2=\"18\"/><text class=\"po\" x=\"46.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">0</text><line style=\"stroke:var(--rule)\" x1=\"61.9\" y1=\"196\" x2=\"61.9\" y2=\"18\"/><text class=\"po\" x=\"61.9\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><line style=\"stroke:var(--rule)\" x1=\"77.8\" y1=\"196\" x2=\"77.8\" y2=\"18\"/><text class=\"po\" x=\"77.8\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><line style=\"stroke:var(--rule)\" x1=\"93.6\" y1=\"196\" x2=\"93.6\" y2=\"18\"/><text class=\"po\" x=\"93.6\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><line style=\"stroke:var(--rule)\" x1=\"109.5\" y1=\"196\" x2=\"109.5\" y2=\"18\"/><text class=\"po\" x=\"109.5\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">4</text><line style=\"stroke:var(--rule)\" x1=\"125.4\" y1=\"196\" x2=\"125.4\" y2=\"18\"/><text class=\"po\" x=\"125.4\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">5</text><line style=\"stroke:var(--rule)\" x1=\"141.2\" y1=\"196\" x2=\"141.2\" y2=\"18\"/><text class=\"po\" x=\"141.2\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">6</text><line style=\"stroke:var(--rule)\" x1=\"157.1\" y1=\"196\" x2=\"157.1\" y2=\"18\"/><text class=\"po\" x=\"157.1\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">7</text><line style=\"stroke:var(--rule)\" x1=\"173.0\" y1=\"196\" x2=\"173.0\" y2=\"18\"/><text class=\"po\" x=\"173.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">8</text><line style=\"stroke:var(--rule)\" x1=\"188.9\" y1=\"196\" x2=\"188.9\" y2=\"18\"/><text class=\"po\" x=\"188.9\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">9</text><line style=\"stroke:var(--rule)\" x1=\"204.8\" y1=\"196\" x2=\"204.8\" y2=\"18\"/><text class=\"po\" x=\"204.8\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">10</text><line style=\"stroke:var(--rule)\" x1=\"220.6\" y1=\"196\" x2=\"220.6\" y2=\"18\"/><text class=\"po\" x=\"220.6\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">11</text><line style=\"stroke:var(--rule)\" x1=\"236.5\" y1=\"196\" x2=\"236.5\" y2=\"18\"/><text class=\"po\" x=\"236.5\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">12</text><line style=\"stroke:var(--rule)\" x1=\"252.4\" y1=\"196\" x2=\"252.4\" y2=\"18\"/><text class=\"po\" x=\"252.4\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">13</text><line style=\"stroke:var(--rule)\" x1=\"268.2\" y1=\"196\" x2=\"268.2\" y2=\"18\"/><text class=\"po\" x=\"268.2\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">14</text><line style=\"stroke:var(--rule)\" x1=\"284.1\" y1=\"196\" x2=\"284.1\" y2=\"18\"/><text class=\"po\" x=\"284.1\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">15</text><line style=\"stroke:var(--rule)\" x1=\"300.0\" y1=\"196\" x2=\"300.0\" y2=\"18\"/><text class=\"po\" x=\"300.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">16</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"196.0\" x2=\"300\" y2=\"196.0\"/><text class=\"po\" x=\"40.0\" y=\"196.0\" text-anchor=\"end\" dominant-baseline=\"middle\">0</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"166.3\" x2=\"300\" y2=\"166.3\"/><text class=\"po\" x=\"40.0\" y=\"166.3\" text-anchor=\"end\" dominant-baseline=\"middle\">2</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"136.7\" x2=\"300\" y2=\"136.7\"/><text class=\"po\" x=\"40.0\" y=\"136.7\" text-anchor=\"end\" dominant-baseline=\"middle\">4</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"107.0\" x2=\"300\" y2=\"107.0\"/><text class=\"po\" x=\"40.0\" y=\"107.0\" text-anchor=\"end\" dominant-baseline=\"middle\">6</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"77.3\" x2=\"300\" y2=\"77.3\"/><text class=\"po\" x=\"40.0\" y=\"77.3\" text-anchor=\"end\" dominant-baseline=\"middle\">8</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"47.7\" x2=\"300\" y2=\"47.7\"/><text class=\"po\" x=\"40.0\" y=\"47.7\" text-anchor=\"end\" dominant-baseline=\"middle\">10</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"18.0\" x2=\"300\" y2=\"18.0\"/><text class=\"po\" x=\"40.0\" y=\"18.0\" text-anchor=\"end\" dominant-baseline=\"middle\">12</text><line class=\"ln\" x1=\"46.0\" y1=\"196.0\" x2=\"308.0\" y2=\"196.0\" marker-end=\"url(#ah)\"/><line class=\"ln\" x1=\"46.0\" y1=\"196.0\" x2=\"46.0\" y2=\"10.0\" marker-end=\"url(#ah)\"/><text class=\"lb\" x=\"300.0\" y=\"188.0\" text-anchor=\"end\" dominant-baseline=\"middle\">t (s)</text><text class=\"lb\" x=\"52.0\" y=\"12.0\" text-anchor=\"start\" dominant-baseline=\"middle\">ω (rad/s)</text><polyline fill=\"none\" style=\"stroke:var(--danger);stroke-width:2.2\" points=\"46.0,196.0 125.4,47.7 220.6,47.7 252.4,196.0\"/><circle cx=\"46.0\" cy=\"196.0\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"125.4\" cy=\"47.7\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"220.6\" cy=\"47.7\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"252.4\" cy=\"196.0\" r=\"3\" style=\"fill:var(--danger)\"/></svg>"}, {"kind": "mcq", "text": "For the ω–t graph shown, what is the angular acceleration from 11 s to 13 s?", "opts": ["−10 rad/s²", "0", "−5 rad/s²", "+5 rad/s²"], "correct": 2, "tag": "", "sol": "Slope = (0 − 10) ÷ 2 = −5 rad/s².", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 230\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line style=\"stroke:var(--rule)\" x1=\"46.0\" y1=\"196\" x2=\"46.0\" y2=\"18\"/><text class=\"po\" x=\"46.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">0</text><line style=\"stroke:var(--rule)\" x1=\"61.9\" y1=\"196\" x2=\"61.9\" y2=\"18\"/><text class=\"po\" x=\"61.9\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><line style=\"stroke:var(--rule)\" x1=\"77.8\" y1=\"196\" x2=\"77.8\" y2=\"18\"/><text class=\"po\" x=\"77.8\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><line style=\"stroke:var(--rule)\" x1=\"93.6\" y1=\"196\" x2=\"93.6\" y2=\"18\"/><text class=\"po\" x=\"93.6\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><line style=\"stroke:var(--rule)\" x1=\"109.5\" y1=\"196\" x2=\"109.5\" y2=\"18\"/><text class=\"po\" x=\"109.5\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">4</text><line style=\"stroke:var(--rule)\" x1=\"125.4\" y1=\"196\" x2=\"125.4\" y2=\"18\"/><text class=\"po\" x=\"125.4\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">5</text><line style=\"stroke:var(--rule)\" x1=\"141.2\" y1=\"196\" x2=\"141.2\" y2=\"18\"/><text class=\"po\" x=\"141.2\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">6</text><line style=\"stroke:var(--rule)\" x1=\"157.1\" y1=\"196\" x2=\"157.1\" y2=\"18\"/><text class=\"po\" x=\"157.1\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">7</text><line style=\"stroke:var(--rule)\" x1=\"173.0\" y1=\"196\" x2=\"173.0\" y2=\"18\"/><text class=\"po\" x=\"173.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">8</text><line style=\"stroke:var(--rule)\" x1=\"188.9\" y1=\"196\" x2=\"188.9\" y2=\"18\"/><text class=\"po\" x=\"188.9\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">9</text><line style=\"stroke:var(--rule)\" x1=\"204.8\" y1=\"196\" x2=\"204.8\" y2=\"18\"/><text class=\"po\" x=\"204.8\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">10</text><line style=\"stroke:var(--rule)\" x1=\"220.6\" y1=\"196\" x2=\"220.6\" y2=\"18\"/><text class=\"po\" x=\"220.6\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">11</text><line style=\"stroke:var(--rule)\" x1=\"236.5\" y1=\"196\" x2=\"236.5\" y2=\"18\"/><text class=\"po\" x=\"236.5\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">12</text><line style=\"stroke:var(--rule)\" x1=\"252.4\" y1=\"196\" x2=\"252.4\" y2=\"18\"/><text class=\"po\" x=\"252.4\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">13</text><line style=\"stroke:var(--rule)\" x1=\"268.2\" y1=\"196\" x2=\"268.2\" y2=\"18\"/><text class=\"po\" x=\"268.2\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">14</text><line style=\"stroke:var(--rule)\" x1=\"284.1\" y1=\"196\" x2=\"284.1\" y2=\"18\"/><text class=\"po\" x=\"284.1\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">15</text><line style=\"stroke:var(--rule)\" x1=\"300.0\" y1=\"196\" x2=\"300.0\" y2=\"18\"/><text class=\"po\" x=\"300.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">16</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"196.0\" x2=\"300\" y2=\"196.0\"/><text class=\"po\" x=\"40.0\" y=\"196.0\" text-anchor=\"end\" dominant-baseline=\"middle\">0</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"166.3\" x2=\"300\" y2=\"166.3\"/><text class=\"po\" x=\"40.0\" y=\"166.3\" text-anchor=\"end\" dominant-baseline=\"middle\">2</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"136.7\" x2=\"300\" y2=\"136.7\"/><text class=\"po\" x=\"40.0\" y=\"136.7\" text-anchor=\"end\" dominant-baseline=\"middle\">4</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"107.0\" x2=\"300\" y2=\"107.0\"/><text class=\"po\" x=\"40.0\" y=\"107.0\" text-anchor=\"end\" dominant-baseline=\"middle\">6</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"77.3\" x2=\"300\" y2=\"77.3\"/><text class=\"po\" x=\"40.0\" y=\"77.3\" text-anchor=\"end\" dominant-baseline=\"middle\">8</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"47.7\" x2=\"300\" y2=\"47.7\"/><text class=\"po\" x=\"40.0\" y=\"47.7\" text-anchor=\"end\" dominant-baseline=\"middle\">10</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"18.0\" x2=\"300\" y2=\"18.0\"/><text class=\"po\" x=\"40.0\" y=\"18.0\" text-anchor=\"end\" dominant-baseline=\"middle\">12</text><line class=\"ln\" x1=\"46.0\" y1=\"196.0\" x2=\"308.0\" y2=\"196.0\" marker-end=\"url(#ah)\"/><line class=\"ln\" x1=\"46.0\" y1=\"196.0\" x2=\"46.0\" y2=\"10.0\" marker-end=\"url(#ah)\"/><text class=\"lb\" x=\"300.0\" y=\"188.0\" text-anchor=\"end\" dominant-baseline=\"middle\">t (s)</text><text class=\"lb\" x=\"52.0\" y=\"12.0\" text-anchor=\"start\" dominant-baseline=\"middle\">ω (rad/s)</text><polyline fill=\"none\" style=\"stroke:var(--danger);stroke-width:2.2\" points=\"46.0,196.0 125.4,47.7 220.6,47.7 252.4,196.0\"/><circle cx=\"46.0\" cy=\"196.0\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"125.4\" cy=\"47.7\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"220.6\" cy=\"47.7\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"252.4\" cy=\"196.0\" r=\"3\" style=\"fill:var(--danger)\"/></svg>"}, {"kind": "mcq", "text": "Counterclockwise is positive. A θ–t graph is a straight line with a negative slope. The object is", "opts": ["turning clockwise at a constant rate", "turning counterclockwise and speeding up", "turning clockwise and slowing down", "at rest"], "correct": 0, "tag": "", "sol": "Constant negative slope: constant negative ω, i.e. steady clockwise rotation."}, {"kind": "mcq", "text": "An ω–t graph is a horizontal line at ω = 5 rad/s. What does the α–t graph look like?", "opts": ["a straight line through the origin with slope 5", "a line sloping down to zero", "a horizontal line at α = 5 rad/s²", "a horizontal line at α = 0"], "correct": 3, "tag": "", "sol": "ω is constant, so α = 0 at all times."}, {"kind": "mcq", "text": "On an ω–t graph (ccw positive) the line crosses the time axis from positive to negative values. At that moment the object", "opts": ["has zero angular acceleration", "reverses its direction of rotation", "stops for good", "has its greatest angular speed"], "correct": 1, "tag": "", "sol": "ω changes sign, so the rotation changes from ccw to cw. α is not zero there (the line is sloping)."}, {"kind": "blank", "p": "Use the ω–t graph of a spinning wheel.", "tag": "", "marks": "", "flat": [{"t": "α during the first 5 s = __B1__ rad/s²", "a": {"B1": "2"}}, {"t": "Total angular displacement = __B1__ rad", "a": {"B1": "95"}}, {"t": "Average angular velocity over 13 s = __B1__ rad/s", "a": {"B1": "7.31"}, "expr": "approx"}, {"t": "Number of revolutions = __B1__", "a": {"B1": "15.1"}, "expr": "approx"}], "sol": "10 ÷ 5 = 2 rad/s².\nArea = 25 + 60 + 10 = 95 rad.\n95 ÷ 13 ≈ 7.31 rad/s.\n95 ÷ 2π ≈ 15.1 rev.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 230\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line style=\"stroke:var(--rule)\" x1=\"46.0\" y1=\"196\" x2=\"46.0\" y2=\"18\"/><text class=\"po\" x=\"46.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">0</text><line style=\"stroke:var(--rule)\" x1=\"61.9\" y1=\"196\" x2=\"61.9\" y2=\"18\"/><text class=\"po\" x=\"61.9\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><line style=\"stroke:var(--rule)\" x1=\"77.8\" y1=\"196\" x2=\"77.8\" y2=\"18\"/><text class=\"po\" x=\"77.8\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><line style=\"stroke:var(--rule)\" x1=\"93.6\" y1=\"196\" x2=\"93.6\" y2=\"18\"/><text class=\"po\" x=\"93.6\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><line style=\"stroke:var(--rule)\" x1=\"109.5\" y1=\"196\" x2=\"109.5\" y2=\"18\"/><text class=\"po\" x=\"109.5\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">4</text><line style=\"stroke:var(--rule)\" x1=\"125.4\" y1=\"196\" x2=\"125.4\" y2=\"18\"/><text class=\"po\" x=\"125.4\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">5</text><line style=\"stroke:var(--rule)\" x1=\"141.2\" y1=\"196\" x2=\"141.2\" y2=\"18\"/><text class=\"po\" x=\"141.2\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">6</text><line style=\"stroke:var(--rule)\" x1=\"157.1\" y1=\"196\" x2=\"157.1\" y2=\"18\"/><text class=\"po\" x=\"157.1\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">7</text><line style=\"stroke:var(--rule)\" x1=\"173.0\" y1=\"196\" x2=\"173.0\" y2=\"18\"/><text class=\"po\" x=\"173.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">8</text><line style=\"stroke:var(--rule)\" x1=\"188.9\" y1=\"196\" x2=\"188.9\" y2=\"18\"/><text class=\"po\" x=\"188.9\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">9</text><line style=\"stroke:var(--rule)\" x1=\"204.8\" y1=\"196\" x2=\"204.8\" y2=\"18\"/><text class=\"po\" x=\"204.8\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">10</text><line style=\"stroke:var(--rule)\" x1=\"220.6\" y1=\"196\" x2=\"220.6\" y2=\"18\"/><text class=\"po\" x=\"220.6\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">11</text><line style=\"stroke:var(--rule)\" x1=\"236.5\" y1=\"196\" x2=\"236.5\" y2=\"18\"/><text class=\"po\" x=\"236.5\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">12</text><line style=\"stroke:var(--rule)\" x1=\"252.4\" y1=\"196\" x2=\"252.4\" y2=\"18\"/><text class=\"po\" x=\"252.4\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">13</text><line style=\"stroke:var(--rule)\" x1=\"268.2\" y1=\"196\" x2=\"268.2\" y2=\"18\"/><text class=\"po\" x=\"268.2\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">14</text><line style=\"stroke:var(--rule)\" x1=\"284.1\" y1=\"196\" x2=\"284.1\" y2=\"18\"/><text class=\"po\" x=\"284.1\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">15</text><line style=\"stroke:var(--rule)\" x1=\"300.0\" y1=\"196\" x2=\"300.0\" y2=\"18\"/><text class=\"po\" x=\"300.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">16</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"196.0\" x2=\"300\" y2=\"196.0\"/><text class=\"po\" x=\"40.0\" y=\"196.0\" text-anchor=\"end\" dominant-baseline=\"middle\">0</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"166.3\" x2=\"300\" y2=\"166.3\"/><text class=\"po\" x=\"40.0\" y=\"166.3\" text-anchor=\"end\" dominant-baseline=\"middle\">2</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"136.7\" x2=\"300\" y2=\"136.7\"/><text class=\"po\" x=\"40.0\" y=\"136.7\" text-anchor=\"end\" dominant-baseline=\"middle\">4</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"107.0\" x2=\"300\" y2=\"107.0\"/><text class=\"po\" x=\"40.0\" y=\"107.0\" text-anchor=\"end\" dominant-baseline=\"middle\">6</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"77.3\" x2=\"300\" y2=\"77.3\"/><text class=\"po\" x=\"40.0\" y=\"77.3\" text-anchor=\"end\" dominant-baseline=\"middle\">8</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"47.7\" x2=\"300\" y2=\"47.7\"/><text class=\"po\" x=\"40.0\" y=\"47.7\" text-anchor=\"end\" dominant-baseline=\"middle\">10</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"18.0\" x2=\"300\" y2=\"18.0\"/><text class=\"po\" x=\"40.0\" y=\"18.0\" text-anchor=\"end\" dominant-baseline=\"middle\">12</text><line class=\"ln\" x1=\"46.0\" y1=\"196.0\" x2=\"308.0\" y2=\"196.0\" marker-end=\"url(#ah)\"/><line class=\"ln\" x1=\"46.0\" y1=\"196.0\" x2=\"46.0\" y2=\"10.0\" marker-end=\"url(#ah)\"/><text class=\"lb\" x=\"300.0\" y=\"188.0\" text-anchor=\"end\" dominant-baseline=\"middle\">t (s)</text><text class=\"lb\" x=\"52.0\" y=\"12.0\" text-anchor=\"start\" dominant-baseline=\"middle\">ω (rad/s)</text><polyline fill=\"none\" style=\"stroke:var(--danger);stroke-width:2.2\" points=\"46.0,196.0 125.4,47.7 220.6,47.7 252.4,196.0\"/><circle cx=\"46.0\" cy=\"196.0\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"125.4\" cy=\"47.7\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"220.6\" cy=\"47.7\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"252.4\" cy=\"196.0\" r=\"3\" style=\"fill:var(--danger)\"/></svg>", "tools": ["calc"]}, {"kind": "blank", "p": "A turntable starts from rest. Its angular position is recorded.\nt (s): 0, 1, 2, 3   →   θ (rad): 0, 2, 8, 18", "tag": "", "marks": "", "flat": [{"t": "θ ÷ t² for each non-zero row = __B1__ rad/s²", "a": {"B1": "2"}}, {"t": "So α = __B1__ rad/s² (θ = ½αt²)", "a": {"B1": "4"}}, {"t": "ω at t = 3 s = __B1__ rad/s", "a": {"B1": "12"}}], "sol": "2 ÷ 1 = 8 ÷ 4 = 18 ÷ 9 = 2.\n½α = 2, α = 4 rad/s².\nω = αt = 4 × 3 = 12 rad/s.", "tools": ["calc", "desmos"], "desmos": [{"type": "table", "columns": [{"latex": "x_1", "values": ["0", "1", "2", "3"]}, {"latex": "y_1", "values": ["0", "2", "8", "18"]}]}, "y_1\\sim ax_1^2"]}, {"kind": "blank", "p": "A wheel spinning at 1.0 rad/s (ccw) has the α–t graph shown.", "tag": "", "marks": "", "flat": [{"t": "Change in ω from 0 to 5 s = __B1__ rad/s", "a": {"B1": "10"}}, {"t": "ω at t = 8 s = __B1__ rad/s", "a": {"B1": "11"}}, {"t": "From 5 s to 8 s the wheel turns at __B1__ angular velocity (increasing / constant / decreasing).", "a": {"B1": "constant"}, "expr": "words"}], "sol": "Area under α–t = 2 × 5 = 10 rad/s.\n1.0 + 10 = 11 rad/s; no change after 5 s.\nα = 0, so ω is constant.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 230\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line style=\"stroke:var(--rule)\" x1=\"46.0\" y1=\"196\" x2=\"46.0\" y2=\"18\"/><text class=\"po\" x=\"46.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">0</text><line style=\"stroke:var(--rule)\" x1=\"71.4\" y1=\"196\" x2=\"71.4\" y2=\"18\"/><text class=\"po\" x=\"71.4\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><line style=\"stroke:var(--rule)\" x1=\"96.8\" y1=\"196\" x2=\"96.8\" y2=\"18\"/><text class=\"po\" x=\"96.8\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><line style=\"stroke:var(--rule)\" x1=\"122.2\" y1=\"196\" x2=\"122.2\" y2=\"18\"/><text class=\"po\" x=\"122.2\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><line style=\"stroke:var(--rule)\" x1=\"147.6\" y1=\"196\" x2=\"147.6\" y2=\"18\"/><text class=\"po\" x=\"147.6\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">4</text><line style=\"stroke:var(--rule)\" x1=\"173.0\" y1=\"196\" x2=\"173.0\" y2=\"18\"/><text class=\"po\" x=\"173.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">5</text><line style=\"stroke:var(--rule)\" x1=\"198.4\" y1=\"196\" x2=\"198.4\" y2=\"18\"/><text class=\"po\" x=\"198.4\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">6</text><line style=\"stroke:var(--rule)\" x1=\"223.8\" y1=\"196\" x2=\"223.8\" y2=\"18\"/><text class=\"po\" x=\"223.8\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">7</text><line style=\"stroke:var(--rule)\" x1=\"249.2\" y1=\"196\" x2=\"249.2\" y2=\"18\"/><text class=\"po\" x=\"249.2\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">8</text><line style=\"stroke:var(--rule)\" x1=\"274.6\" y1=\"196\" x2=\"274.6\" y2=\"18\"/><text class=\"po\" x=\"274.6\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">9</text><line style=\"stroke:var(--rule)\" x1=\"300.0\" y1=\"196\" x2=\"300.0\" y2=\"18\"/><text class=\"po\" x=\"300.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">10</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"196.0\" x2=\"300\" y2=\"196.0\"/><text class=\"po\" x=\"40.0\" y=\"196.0\" text-anchor=\"end\" dominant-baseline=\"middle\">0</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"136.7\" x2=\"300\" y2=\"136.7\"/><text class=\"po\" x=\"40.0\" y=\"136.7\" text-anchor=\"end\" dominant-baseline=\"middle\">1</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"77.3\" x2=\"300\" y2=\"77.3\"/><text class=\"po\" x=\"40.0\" y=\"77.3\" text-anchor=\"end\" dominant-baseline=\"middle\">2</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"18.0\" x2=\"300\" y2=\"18.0\"/><text class=\"po\" x=\"40.0\" y=\"18.0\" text-anchor=\"end\" dominant-baseline=\"middle\">3</text><line class=\"ln\" x1=\"46.0\" y1=\"196.0\" x2=\"308.0\" y2=\"196.0\" marker-end=\"url(#ah)\"/><line class=\"ln\" x1=\"46.0\" y1=\"196.0\" x2=\"46.0\" y2=\"10.0\" marker-end=\"url(#ah)\"/><text class=\"lb\" x=\"300.0\" y=\"188.0\" text-anchor=\"end\" dominant-baseline=\"middle\">t (s)</text><text class=\"lb\" x=\"52.0\" y=\"12.0\" text-anchor=\"start\" dominant-baseline=\"middle\">α (rad/s²)</text><polyline fill=\"none\" style=\"stroke:var(--danger);stroke-width:2.2\" points=\"46.0,77.3 173.0,77.3\"/><circle cx=\"46.0\" cy=\"77.3\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"173.0\" cy=\"77.3\" r=\"3\" style=\"fill:var(--danger)\"/><polyline fill=\"none\" style=\"stroke:var(--success);stroke-width:2.2\" points=\"173.0,196.0 249.2,196.0\"/><circle cx=\"173.0\" cy=\"196.0\" r=\"3\" style=\"fill:var(--success)\"/><circle cx=\"249.2\" cy=\"196.0\" r=\"3\" style=\"fill:var(--success)\"/></svg>"}]}, {"id": "s3", "label": "5.2.A", "sub": "Linear motion of a rotating point — LO 5.2.A: describe the linear motion of a point on a rotating rigid system that corresponds to the rotational motion of that point, and vice versa.", "slides": [{"kind": "mcq", "text": "Points A (0.10 m from the axis) and B (0.30 m from the axis) are on the same rotating disc. Which is correct?", "opts": ["B has 3 times A's ω; same linear speed", "A has 3 times B's ω", "Same ω and same linear speed", "Same ω; B's linear speed is 3 times A's"], "correct": 3, "tag": "", "sol": "All points of a rigid system share ω; v = rω grows with r.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 260 180\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><circle class=\"sh3\" cx=\"130.0\" cy=\"90.0\" r=\"60\"/><circle class=\"pt\" cx=\"130.0\" cy=\"90.0\" r=\"2.8\"/><text class=\"lb\" x=\"121.0\" y=\"100.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">O</text><line class=\"hid\" x1=\"130.0\" y1=\"90.0\" x2=\"111.4\" y2=\"83.2\"/><circle class=\"pt\" cx=\"111.4\" cy=\"83.2\" r=\"2.8\"/><text class=\"lb\" x=\"100.1\" y=\"79.1\" text-anchor=\"middle\" dominant-baseline=\"middle\">A</text><line class=\"hid\" x1=\"130.0\" y1=\"90.0\" x2=\"184.4\" y2=\"115.4\"/><circle class=\"pt\" cx=\"184.4\" cy=\"115.4\" r=\"2.8\"/><text class=\"lb\" x=\"195.3\" y=\"120.4\" text-anchor=\"middle\" dominant-baseline=\"middle\">B</text><path class=\"arm\" d=\"M199.5,71.4 A72,72 0 0 0 154.6,22.3\" marker-end=\"url(#ah)\"/></svg>"}, {"kind": "mcq", "text": "A wheel of radius 0.40 m turns at 5.0 rad/s. How fast does a point on its rim move?", "opts": ["2.0 m/s", "0.080 m/s", "12.5 m/s", "5.0 m/s"], "correct": 0, "tag": "", "sol": "v = rω = 0.40 × 5.0 = 2.0 m/s."}, {"kind": "mcq", "text": "A rope is wound on a drum of radius 0.20 m. The drum turns through 10 rad. How much rope is unwound?", "opts": ["2.0 m", "50 m", "10 m", "0.020 m"], "correct": 0, "tag": "", "sol": "s = rθ = 0.20 × 10 = 2.0 m."}, {"kind": "mcq", "text": "A disc has α = 6.0 rad/s². What is the tangential acceleration of a point 0.50 m from its axis?", "opts": ["6.0 m/s²", "3.0 m/s²", "12 m/s²", "0.083 m/s²"], "correct": 1, "tag": "", "sol": "a_T = rα = 0.50 × 6.0 = 3.0 m/s²."}, {"kind": "mcq", "text": "A child on a merry-go-round moves from the rim to halfway to the centre while ω stays the same. Her linear speed", "opts": ["becomes a quarter", "stays the same", "halves", "doubles"], "correct": 2, "tag": "", "sol": "v = rω; halving r halves v."}, {"kind": "mcq", "text": "Earth turns once a day. Compared with a person in Delhi, a person in Chennai (closer to the equator, so farther from Earth's axis) has", "opts": ["the same angular velocity and the same linear speed", "the same angular velocity but a greater linear speed", "a greater angular velocity", "a smaller linear speed"], "correct": 1, "tag": "", "sol": "Same ω for the whole Earth; larger r gives larger v."}, {"kind": "mcq", "text": "For s = rθ and v = rω to be correct, the angle must be measured in", "opts": ["degrees", "radians", "any unit", "revolutions"], "correct": 1, "tag": "", "sol": "The radian is defined by s = rθ."}, {"kind": "mcq", "text": "The minute hand of a clock is 12 cm long. How fast does its tip move?", "opts": ["about 1.3 × 10⁻² m/s", "about 2.1 × 10⁻⁴ m/s", "about 3.3 × 10⁻⁵ m/s", "0.12 m/s"], "correct": 1, "tag": "", "sol": "ω = 2π ÷ 3600 s ≈ 1.75 × 10⁻³ rad/s; v = 0.12ω ≈ 2.1 × 10⁻⁴ m/s.", "tools": ["calc"]}, {"kind": "blank", "p": "A washing-machine drum of radius 0.25 m spins at 90 rad/s. It then slows uniformly to rest in 15 s.", "tag": "", "marks": "", "flat": [{"t": "Linear speed of the drum wall at 90 rad/s = __B1__ m/s", "a": {"B1": "22.5"}, "expr": "approx"}, {"t": "Magnitude of α while stopping = __B1__ rad/s²", "a": {"B1": "6"}}, {"t": "Magnitude of the tangential acceleration of the wall = __B1__ m/s²", "a": {"B1": "1.5"}}], "sol": "v = rω = 0.25 × 90 = 22.5 m/s.\n90 ÷ 15 = 6 rad/s².\na_T = rα = 0.25 × 6 = 1.5 m/s².", "tools": ["calc"]}, {"kind": "blank", "p": "A bucket is raised from a well by a rope wound on a drum of radius 0.10 m. The bucket rises 2.0 m in 4.0 s at a steady speed.", "tag": "", "marks": "", "flat": [{"t": "Speed of the rope = __B1__ m/s", "a": {"B1": "0.5"}}, {"t": "Angular velocity of the drum = __B1__ rad/s", "a": {"B1": "5"}}, {"t": "Angle turned by the drum = __B1__ rad", "a": {"B1": "20"}}, {"t": "Number of turns = __B1__", "a": {"B1": "3.18"}, "expr": "approx"}], "sol": "2.0 ÷ 4.0 = 0.50 m/s.\nω = v ÷ r = 0.50 ÷ 0.10 = 5.0 rad/s.\nθ = s ÷ r = 2.0 ÷ 0.10 = 20 rad.\n20 ÷ 2π ≈ 3.18 turns.", "tools": ["calc"]}, {"kind": "blank", "p": "Points P and Q are 0.20 m and 0.60 m from the axis of a rigid rotor. P moves at 1.2 m/s.", "tag": "", "marks": "", "flat": [{"t": "Angular velocity of the rotor = __B1__ rad/s", "a": {"B1": "6"}}, {"t": "Speed of Q = __B1__ m/s", "a": {"B1": "3.6"}}, {"t": "When the rotor speeds up, a_T at Q ÷ a_T at P = __B1__", "a": {"B1": "3"}}], "sol": "ω = 1.2 ÷ 0.20 = 6.0 rad/s.\nv = 0.60 × 6.0 = 3.6 m/s.\nSame α: a_T ∝ r, so 0.60 ÷ 0.20 = 3.", "tools": ["calc"]}]}, {"id": "s4", "label": "5.3.A", "sub": "Identifying torques — LO 5.3.A: identify the torques exerted on a rigid system.", "slides": [{"kind": "mcq", "text": "A force whose line of action passes through the axis of rotation exerts", "opts": ["zero torque about that axis", "the greatest possible torque", "a clockwise torque", "a torque equal to the force"], "correct": 0, "tag": "", "sol": "Its lever arm is zero, so τ = r⊥F = 0."}, {"kind": "mcq", "text": "Which part of a force produces torque about an axis?", "opts": ["the component along the axis", "the component parallel to the position vector", "the component perpendicular to the position vector from the axis", "only the vertical component"], "correct": 2, "tag": "", "sol": "τ = rF⊥; the parallel component pulls or pushes along r and cannot turn the object."}, {"kind": "mcq", "text": "The lever arm of a force is", "opts": ["the perpendicular distance from the axis to the line of action of the force", "the force multiplied by a distance", "the distance from the axis to the point where the force acts", "the length of the object"], "correct": 0, "tag": "", "sol": "Lever arm r⊥ = r sin θ, measured perpendicular to the line of action."}, {"kind": "mcq", "text": "You push on a door at its handle, far from the hinges. Which push opens it most easily?", "opts": ["perpendicular to the door but near the hinges", "perpendicular to the door", "along the door towards the hinges", "at 30° to the door"], "correct": 1, "tag": "", "sol": "Greatest lever arm and all of the force perpendicular."}, {"kind": "mcq", "text": "A uniform beam is pivoted at its centre. About the pivot, the beam's own weight exerts", "opts": ["a counterclockwise torque", "a torque that depends on the beam's length", "zero torque, because it acts at the centre of mass, which is on the axis", "a clockwise torque"], "correct": 2, "tag": "", "sol": "The weight acts at the centre of mass; here that point is on the axis, so its lever arm is zero."}, {"kind": "mcq", "text": "The rod in the force diagram is pivoted at its left end. Which force exerts zero torque about the pivot?", "opts": ["C only", "B and C", "A only", "none of them"], "correct": 0, "tag": "", "sol": "C acts at the pivot, so its lever arm is zero. A and B act away from the pivot.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 150\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect class=\"sh\" x=\"32.0\" y=\"65.0\" width=\"256.0\" height=\"10\"/><path class=\"sh3\" d=\"M32.0,75.0 L21.0,93.0 L43.0,93.0 Z\"/><line class=\"arm\" x1=\"288.0\" y1=\"75.0\" x2=\"288.0\" y2=\"115.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"288.0\" y=\"127.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">A</text><line class=\"arm\" x1=\"160.0\" y1=\"65.0\" x2=\"160.0\" y2=\"25.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"160.0\" y=\"13.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">B</text><line class=\"arm\" x1=\"32.0\" y1=\"65.0\" x2=\"32.0\" y2=\"29.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"32.0\" y=\"17.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">C</text></svg>"}, {"kind": "mcq", "text": "How does a force diagram differ from a free-body diagram?", "opts": ["Only vertical forces are drawn", "Each force is drawn at the point where it is exerted on the system", "All forces are drawn from a single central dot", "Forces are drawn without directions"], "correct": 1, "tag": "", "sol": "The points of application matter for torque, so a force diagram shows where each force acts."}, {"kind": "mcq", "text": "A spanner grips a nut at its left end. You pull DOWN on the right end of the spanner. The torque on the nut, as you look at it, is", "opts": ["clockwise", "counterclockwise", "zero", "impossible to tell"], "correct": 0, "tag": "", "sol": "A downward pull to the right of the axis turns the spanner clockwise."}, {"kind": "blank", "p": "Look at the force diagram of the rod pivoted at its left end.", "tag": "", "marks": "", "flat": [{"t": "Torque of A about the pivot: __B1__ (clockwise / counterclockwise / zero)", "a": {"B1": "clockwise"}, "expr": "words", "accept": ["cw"]}, {"t": "Torque of B: __B1__ (clockwise / counterclockwise / zero)", "a": {"B1": "counterclockwise"}, "expr": "words", "accept": ["anticlockwise", "ccw"]}, {"t": "Torque of C: __B1__ (clockwise / counterclockwise / zero)", "a": {"B1": "zero"}, "expr": "words", "accept": ["none", "no torque"]}], "sol": "A pulls down to the right of the pivot: clockwise.\nB pushes up to the right of the pivot: counterclockwise.\nC acts at the pivot: zero.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 150\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect class=\"sh\" x=\"32.0\" y=\"65.0\" width=\"256.0\" height=\"10\"/><path class=\"sh3\" d=\"M32.0,75.0 L21.0,93.0 L43.0,93.0 Z\"/><line class=\"arm\" x1=\"288.0\" y1=\"75.0\" x2=\"288.0\" y2=\"115.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"288.0\" y=\"127.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">A</text><line class=\"arm\" x1=\"160.0\" y1=\"65.0\" x2=\"160.0\" y2=\"25.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"160.0\" y=\"13.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">B</text><line class=\"arm\" x1=\"32.0\" y1=\"65.0\" x2=\"32.0\" y2=\"29.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"32.0\" y=\"17.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">C</text></svg>"}, {"kind": "blank", "p": "A wheel of radius 0.25 m can turn about its centre O. Force P is tangent to the rim, Q acts at the rim along a radius (pointing outward) and R acts at O.", "tag": "", "marks": "", "flat": [{"t": "Does P exert a torque about O? __B1__ (yes / no)", "a": {"B1": "yes"}, "expr": "words", "accept": ["y"]}, {"t": "Does Q exert a torque about O? __B1__ (yes / no)", "a": {"B1": "no"}, "expr": "words", "accept": ["n"]}, {"t": "Does R exert a torque about O? __B1__ (yes / no)", "a": {"B1": "no"}, "expr": "words", "accept": ["n"]}, {"t": "Lever arm of P = __B1__ m", "a": {"B1": "0.25"}}], "sol": "P is perpendicular to the radius: it has a lever arm.\nQ's line of action passes through O: no torque.\nR acts at the axis: no torque.\nA tangent force at the rim has lever arm r = 0.25 m.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 260 180\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><circle class=\"sh3\" cx=\"130.0\" cy=\"90.0\" r=\"60\"/><circle class=\"pt\" cx=\"130.0\" cy=\"90.0\" r=\"2.8\"/><text class=\"lb\" x=\"121.0\" y=\"100.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">O</text><line class=\"arm\" x1=\"130.0\" y1=\"30.0\" x2=\"86.0\" y2=\"30.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"74.0\" y=\"30.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">P</text><line class=\"arm\" x1=\"182.0\" y1=\"120.0\" x2=\"216.6\" y2=\"140.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"227.0\" y=\"146.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">Q</text><line class=\"arm\" x1=\"130.0\" y1=\"90.0\" x2=\"147.0\" y2=\"60.6\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"153.0\" y=\"50.2\" text-anchor=\"middle\" dominant-baseline=\"middle\">R</text></svg>"}, {"kind": "blank", "p": "A uniform ladder leans against a smooth wall (force diagram shown). Take the axis at the foot of the ladder, where N₁ and f act.", "tag": "", "marks": "", "flat": [{"t": "Does N₁ (floor, upward) exert a torque about the foot? __B1__ (yes / no)", "a": {"B1": "no"}, "expr": "words", "accept": ["n"]}, {"t": "Does f (floor friction) exert a torque about the foot? __B1__ (yes / no)", "a": {"B1": "no"}, "expr": "words", "accept": ["n"]}, {"t": "Does W (weight, at the middle) exert a torque about the foot? __B1__ (yes / no)", "a": {"B1": "yes"}, "expr": "words", "accept": ["y"]}, {"t": "Does N₂ (wall force at the top) exert a torque about the foot? __B1__ (yes / no)", "a": {"B1": "yes"}, "expr": "words", "accept": ["y"]}], "sol": "N₁ acts at the axis.\nf acts at the axis.\nW acts at the middle with a horizontal lever arm.\nN₂ is horizontal at the top; its lever arm is the height of the top.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 260 210\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line x1=\"10\" y1=\"190\" x2=\"220\" y2=\"190\" style=\"stroke:var(--ink);stroke-width:2\"/><line x1=\"220\" y1=\"190\" x2=\"220\" y2=\"14\" style=\"stroke:var(--ink);stroke-width:2\"/><line x1=\"100\" y1=\"190\" x2=\"220\" y2=\"30\" style=\"stroke:var(--gold);stroke-width:7;stroke-linecap:round\"/><line class=\"arm\" x1=\"160.0\" y1=\"110.0\" x2=\"160.0\" y2=\"154.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"172.0\" y=\"150.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">W</text><line class=\"arm\" x1=\"220.0\" y1=\"30.0\" x2=\"176.0\" y2=\"30.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"170.0\" y=\"18.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">N₂</text><line class=\"arm\" x1=\"100.0\" y1=\"190.0\" x2=\"100.0\" y2=\"144.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"88.0\" y=\"140.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">N₁</text><line class=\"arm\" x1=\"100.0\" y1=\"190.0\" x2=\"140.0\" y2=\"190.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"144.0\" y=\"202.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">f</text></svg>"}]}, {"id": "s5", "label": "5.3.B", "sub": "Describing torques — LO 5.3.B: describe the torques exerted on a rigid system.", "slides": [{"kind": "mcq", "text": "A 50 N force acts perpendicular to a 0.40 m spanner at its end. What torque does it exert?", "opts": ["0.0080 N·m", "125 N·m", "50 N·m", "20 N·m"], "correct": 3, "tag": "", "sol": "τ = rF = 0.40 × 50 = 20 N·m."}, {"kind": "mcq", "text": "The same 50 N force now acts at the end of the 0.40 m spanner at 30° to the handle. What torque does it exert?", "opts": ["5.0 N·m", "10 N·m", "20 N·m", "about 17.3 N·m"], "correct": 1, "tag": "", "sol": "τ = rF sin θ = 0.40 × 50 × sin 30° = 10 N·m (17.3 uses cos by mistake).", "tools": ["calc"]}, {"kind": "mcq", "text": "To produce the same torque with a lever arm half as long, the force must be", "opts": ["four times as large", "halved", "unchanged", "doubled"], "correct": 3, "tag": "", "sol": "τ = r⊥F: halve r⊥, double F."}, {"kind": "mcq", "text": "Which is the SI unit of torque?", "opts": ["kg·m²", "N·m", "kg·m/s", "N/m"], "correct": 1, "tag": "", "sol": "Torque = force × lever arm: newton × metre."}, {"kind": "mcq", "text": "A wheel of radius 0.40 m has a 12 N force pulling counterclockwise at its rim and a 7.0 N force pulling clockwise at its rim (both tangent). What is the net torque?", "opts": ["7.6 N·m counterclockwise", "2.0 N·m clockwise", "2.0 N·m counterclockwise", "5.0 N·m counterclockwise"], "correct": 2, "tag": "", "sol": "(12 − 7.0) × 0.40 = 2.0 N·m, ccw."}, {"kind": "mcq", "text": "A 30 N force acts 1.2 m from an axis, but its line of action passes 0.50 m from the axis. What is its torque?", "opts": ["25 N·m", "15 N·m", "60 N·m", "36 N·m"], "correct": 1, "tag": "", "sol": "Use the lever arm: τ = r⊥F = 0.50 × 30 = 15 N·m."}, {"kind": "mcq", "text": "A force F acts at distance r at angle θ to the position vector. Which change leaves the torque unchanged?", "opts": ["doubling r", "changing θ from 30° to 60°", "halving F", "changing θ from 30° to 150°"], "correct": 3, "tag": "", "sol": "sin 150° = sin 30°, so the torque is the same."}, {"kind": "mcq", "text": "Rank the torques: X: 20 N at 0.50 m, perpendicular. Y: 40 N at 0.50 m, at 30° to r. Z: 10 N at 1.0 m, perpendicular.", "opts": ["X = Y = Z", "Z > X > Y", "Y > X = Z", "Y > X > Z"], "correct": 0, "tag": "", "sol": "X = 10 N·m; Y = 0.50 × 40 × 0.5 = 10 N·m; Z = 10 N·m."}, {"kind": "blank", "p": "A mechanic pulls with 80 N on the end of a 0.25 m spanner, at 60° to the handle.", "tag": "", "marks": "", "flat": [{"t": "Torque = __B1__ N·m", "a": {"B1": "17.3"}, "expr": "approx"}, {"t": "Greatest torque possible with 80 N on this spanner = __B1__ N·m", "a": {"B1": "20"}}, {"t": "Force at 90° that gives the same torque as in the first blank = __B1__ N", "a": {"B1": "69.3"}, "expr": "approx"}], "sol": "0.25 × 80 × sin 60° ≈ 17.3 N·m.\nAt 90°: 0.25 × 80 = 20 N·m.\n17.3 ÷ 0.25 ≈ 69.3 N.", "tools": ["calc"]}, {"kind": "blank", "p": "A uniform 2.0 m plank of weight 30 N is pivoted at its left end and held horizontal. A 50 N box sits 1.5 m from the pivot.", "tag": "", "marks": "", "flat": [{"t": "Torque of the plank's weight = __B1__ N·m", "a": {"B1": "30"}}, {"t": "Torque of the box = __B1__ N·m", "a": {"B1": "75"}}, {"t": "Total torque of these two weights = __B1__ N·m", "a": {"B1": "105"}}, {"t": "Their torque is __B1__ (clockwise / counterclockwise).", "a": {"B1": "clockwise"}, "expr": "words", "accept": ["cw"]}], "sol": "Weight acts at the centre, 1.0 m: 30 × 1.0 = 30 N·m.\n50 × 1.5 = 75 N·m.\n30 + 75 = 105 N·m.\nBoth pull down to the right of the pivot: clockwise.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 140\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect class=\"sh\" x=\"32.0\" y=\"43.0\" width=\"256.0\" height=\"10\"/><path class=\"sh3\" d=\"M32.0,53.0 L21.0,71.0 L43.0,71.0 Z\"/><line class=\"ln\" x1=\"224.0\" y1=\"53.0\" x2=\"224.0\" y2=\"78.0\"/><rect class=\"sh2\" x=\"204.0\" y=\"78.0\" width=\"40\" height=\"20\"/><text class=\"lb\" x=\"224.0\" y=\"88.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">50 N</text><line class=\"arm\" x1=\"160.0\" y1=\"53.0\" x2=\"160.0\" y2=\"83.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"160.0\" y=\"95.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">30 N</text><line class=\"ln\" x1=\"32.0\" y1=\"22.0\" x2=\"224.0\" y2=\"22.0\" marker-end=\"url(#ah)\" marker-start=\"url(#ahs)\"/><text class=\"lb\" x=\"128.0\" y=\"14.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1.5 m</text></svg>", "tools": ["calc"]}, {"kind": "blank", "p": "A disc of radius 0.20 m turns about its centre (ccw positive). Forces on it: 15 N tangent at the rim, counterclockwise; 25 N tangent at the hub (r = 0.10 m), clockwise; 40 N at the rim pointing straight out along a radius.", "tag": "", "marks": "", "flat": [{"t": "Torque of the 15 N force = __B1__ N·m", "a": {"B1": "3"}}, {"t": "Torque of the 25 N force = __B1__ N·m", "a": {"B1": "-2.5"}}, {"t": "Torque of the 40 N force = __B1__ N·m", "a": {"B1": "0"}}, {"t": "Net torque = __B1__ N·m", "a": {"B1": "0.5"}}], "sol": "+0.20 × 15 = +3.0 N·m.\n−0.10 × 25 = −2.5 N·m.\nIts line of action passes through the axis: 0.\n3.0 − 2.5 + 0 = +0.5 N·m (ccw).", "tools": ["calc"]}]}, {"id": "s6", "label": "5.4.A", "sub": "Rotational inertia — LO 5.4.A: describe the rotational inertia of a rigid system relative to a given axis of rotation.", "slides": [{"kind": "mcq", "text": "The rotational inertia of a rigid system depends on", "opts": ["the net torque on it", "its mass and how that mass is distributed about the axis", "its mass only", "how fast it is spinning"], "correct": 1, "tag": "", "sol": "I = Σmr²: both mass and distance from the axis matter."}, {"kind": "mcq", "text": "What is the rotational inertia of a 2.0 kg point mass 0.50 m from an axis?", "opts": ["0.25 kg·m²", "0.50 kg·m²", "1.0 kg·m²", "2.0 kg·m²"], "correct": 1, "tag": "", "sol": "I = mr² = 2.0 × 0.25 = 0.50 kg·m²."}, {"kind": "mcq", "text": "Two 1.0 kg masses sit 0.20 m on either side of the axis on a light rod. What is I?", "opts": ["0.16 kg·m²", "0.40 kg·m²", "0.040 kg·m²", "0.080 kg·m²"], "correct": 3, "tag": "", "sol": "2 × 1.0 × 0.20² = 0.080 kg·m²."}, {"kind": "mcq", "text": "A hoop and a solid disc have the same mass and radius. About their central axes, which has the greater rotational inertia?", "opts": ["it depends on how fast they spin", "the hoop, because all its mass is at the rim", "they are equal", "the disc"], "correct": 1, "tag": "", "sol": "The disc has mass close to the axis, so I_disc = ½MR² < I_hoop = MR²."}, {"kind": "mcq", "text": "A small mass is moved to twice its distance from the axis. Its rotational inertia becomes", "opts": ["half as large", "4 times as large", "the same", "2 times as large"], "correct": 1, "tag": "", "sol": "I ∝ r²."}, {"kind": "mcq", "text": "A uniform rod is turned about an axis through one end instead of through its centre. Its rotational inertia is", "opts": ["smaller", "zero", "larger", "the same"], "correct": 2, "tag": "", "sol": "I is smallest about an axis through the centre of mass; more of the mass is far from an end axis."}, {"kind": "mcq", "text": "Why is it much easier to spin a pencil about its long axis than end over end?", "opts": ["Its mass is much closer to the long axis, so I is smaller", "Air resistance is smaller", "Gravity helps it spin", "It has less mass about that axis"], "correct": 0, "tag": "", "sol": "Same mass, much smaller distances from the axis."}, {"kind": "mcq", "text": "Three point masses lie on the same side of an axis: 1.0 kg at 0.10 m, 2.0 kg at 0.20 m and 3.0 kg at 0.30 m. What is their total rotational inertia?", "opts": ["0.14 kg·m²", "0.36 kg·m²", "0.60 kg·m²", "1.4 kg·m²"], "correct": 1, "tag": "", "sol": "0.01 + 0.08 + 0.27 = 0.36 kg·m². (1.4 is Σmr, not Σmr².)", "tools": ["calc"]}, {"kind": "blank", "p": "A light rod 0.80 m long has 0.50 kg masses at each end and a 1.0 kg mass at its centre.", "tag": "", "marks": "", "flat": [{"t": "I about an axis through the centre = __B1__ kg·m²", "a": {"B1": "0.16"}}, {"t": "I about an axis through one end = __B1__ kg·m²", "a": {"B1": "0.48"}}, {"t": "I(end) ÷ I(centre) = __B1__", "a": {"B1": "3"}}, {"t": "The smaller I is about the __B1__ axis (centre / end).", "a": {"B1": "centre"}, "expr": "words", "accept": ["center", "middle"]}], "sol": "2 × 0.50 × 0.40² + 0 = 0.16 kg·m².\n0 + 1.0 × 0.40² + 0.50 × 0.80² = 0.48 kg·m².\n0.48 ÷ 0.16 = 3.\nMinimum about the centre of mass.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 120\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line x1=\"32.0\" y1=\"64\" x2=\"288.0\" y2=\"64\" style=\"stroke:var(--ink);stroke-width:3\"/><circle cx=\"32.0\" cy=\"64\" r=\"8\" style=\"fill:var(--accent-text);stroke:var(--ink);stroke-width:1\"/><text class=\"lb\" x=\"32.0\" y=\"46.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">0.5 kg</text><circle cx=\"160.0\" cy=\"64\" r=\"8\" style=\"fill:var(--accent-text);stroke:var(--ink);stroke-width:1\"/><text class=\"lb\" x=\"160.0\" y=\"46.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1.0 kg</text><circle cx=\"288.0\" cy=\"64\" r=\"8\" style=\"fill:var(--accent-text);stroke:var(--ink);stroke-width:1\"/><text class=\"lb\" x=\"288.0\" y=\"46.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">0.5 kg</text><line class=\"ln\" x1=\"32.0\" y1=\"100.0\" x2=\"288.0\" y2=\"100.0\" marker-end=\"url(#ah)\" marker-start=\"url(#ahs)\"/><text class=\"lb\" x=\"160.0\" y=\"110.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">0.80 m</text></svg>", "tools": ["calc"]}, {"kind": "blank", "p": "Four 0.20 kg balls sit at the corners of a square of side 0.60 m, joined by light rods. The axis is perpendicular to the square.", "tag": "", "marks": "", "flat": [{"t": "Square of the distance from the centre to a corner = __B1__ m²", "a": {"B1": "0.18"}}, {"t": "I about an axis through the centre = __B1__ kg·m²", "a": {"B1": "0.144"}}, {"t": "I about an axis through one corner ball = __B1__ kg·m²", "a": {"B1": "0.288"}}], "sol": "(0.60 ÷ √2)² = 0.18 m².\n4 × 0.20 × 0.18 = 0.144 kg·m².\n0 + 2 × 0.20 × 0.36 + 0.20 × 0.72 = 0.288 kg·m².", "tools": ["calc"]}, {"kind": "blank", "p": "A hoop and a solid disc each have M = 2.0 kg and R = 0.30 m. Use I_hoop = MR² and I_disc = ½MR².", "tag": "", "marks": "", "flat": [{"t": "I_disc = __B1__ kg·m²", "a": {"B1": "0.09"}}, {"t": "I_hoop = __B1__ kg·m²", "a": {"B1": "0.18"}}, {"t": "Which is harder to spin up with the same torque? __B1__ (hoop / disc)", "a": {"B1": "hoop"}, "expr": "words"}], "sol": "½ × 2.0 × 0.09 = 0.090 kg·m².\n2.0 × 0.09 = 0.18 kg·m².\nLarger I: the hoop.", "tools": ["calc"]}]}, {"id": "s7", "label": "5.4.B", "sub": "Parallel-axis theorem — LO 5.4.B: describe the rotational inertia of a rigid system rotating about an axis that does not pass through the system's center of mass.", "slides": [{"kind": "mcq", "text": "The parallel-axis theorem states that", "opts": ["I = I_cm + Md²", "I = I_cm + Md", "I = Md²", "I = I_cm − Md²"], "correct": 0, "tag": "", "sol": "Moving the axis a distance d from the centre of mass adds Md²."}, {"kind": "mcq", "text": "A uniform rod has M = 0.90 kg, L = 2.0 m and I_cm = ML²/12 = 0.30 kg·m². What is I about one end?", "opts": ["0.90 kg·m²", "0.30 kg·m²", "2.1 kg·m²", "1.2 kg·m²"], "correct": 3, "tag": "", "sol": "d = 1.0 m: 0.30 + 0.90 × 1.0² = 1.2 kg·m².", "tools": ["calc"]}, {"kind": "mcq", "text": "For a set of parallel axes, about which one is the rotational inertia smallest?", "opts": ["the one at the edge", "they are all equal", "the one through the centre of mass", "the one farthest from the centre of mass"], "correct": 2, "tag": "", "sol": "I = I_cm + Md² is least when d = 0."}, {"kind": "mcq", "text": "A solid disc (M = 2.0 kg, R = 0.20 m, I_cm = 0.040 kg·m²) turns about an axis through its rim, parallel to its central axis. What is I?", "opts": ["0.16 kg·m²", "0.040 kg·m²", "0.080 kg·m²", "0.12 kg·m²"], "correct": 3, "tag": "", "sol": "0.040 + 2.0 × 0.20² = 0.12 kg·m².", "tools": ["calc"]}, {"kind": "mcq", "text": "In I = I_cm + Md², d is", "opts": ["the length of the object", "the distance from the axis to the edge", "the distance between the two parallel axes", "the radius of the object"], "correct": 2, "tag": "", "sol": "d separates the axis through the centre of mass from the new parallel axis."}, {"kind": "mcq", "text": "An object of mass 2.0 kg has I = 0.030 kg·m² about an axis 5.0 cm from its centre of mass. What is I_cm?", "opts": ["0.025 kg·m²", "0.035 kg·m²", "0.030 kg·m²", "0.020 kg·m²"], "correct": 0, "tag": "", "sol": "I_cm = 0.030 − 2.0 × 0.050² = 0.025 kg·m².", "tools": ["calc"]}, {"kind": "mcq", "text": "The parallel-axis theorem can be used when", "opts": ["the new axis is parallel to an axis through the centre of mass", "both axes pass through the centre of mass", "any two axes are chosen", "the two axes are perpendicular"], "correct": 0, "tag": "", "sol": "It links I about the centre-of-mass axis to I about any parallel axis."}, {"kind": "mcq", "text": "A metre stick swings about a nail through its 20 cm mark, and then about a nail through its 50 cm mark. Its rotational inertia is", "opts": ["greater about the 50 cm mark, because more stick is on each side", "the same about both", "greater about the 50 cm mark", "greater about the 20 cm mark, which is 0.30 m from the centre of mass"], "correct": 3, "tag": "", "sol": "About 20 cm: I = I_cm + M(0.30)² > I_cm."}, {"kind": "blank", "p": "A uniform rod has M = 0.60 kg and L = 1.2 m. I_cm = ML²/12.", "tag": "", "marks": "", "flat": [{"t": "I_cm = __B1__ kg·m²", "a": {"B1": "0.072"}}, {"t": "I about one end = __B1__ kg·m²", "a": {"B1": "0.288"}}, {"t": "I about an axis 0.20 m from one end = __B1__ kg·m²", "a": {"B1": "0.168"}}, {"t": "I(end) ÷ I_cm = __B1__", "a": {"B1": "4"}}], "sol": "0.60 × 1.44 ÷ 12 = 0.072 kg·m².\nd = 0.60 m: 0.072 + 0.60 × 0.36 = 0.288 kg·m².\nd = 0.40 m: 0.072 + 0.60 × 0.16 = 0.168 kg·m².\n0.288 ÷ 0.072 = 4.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 120\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line x1=\"32.0\" y1=\"64\" x2=\"288.0\" y2=\"64\" style=\"stroke:var(--ink);stroke-width:3\"/><line x1=\"160.0\" y1=\"24\" x2=\"160.0\" y2=\"92\" style=\"stroke:var(--danger);stroke-width:1.6;stroke-dasharray:5 4\"/><text class=\"al\" x=\"160.0\" y=\"16.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">cm axis</text><line x1=\"74.7\" y1=\"24\" x2=\"74.7\" y2=\"92\" style=\"stroke:var(--danger);stroke-width:1.6;stroke-dasharray:5 4\"/><text class=\"al\" x=\"74.7\" y=\"16.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">new axis</text><line class=\"ln\" x1=\"74.7\" y1=\"100.0\" x2=\"160.0\" y2=\"100.0\" marker-end=\"url(#ah)\" marker-start=\"url(#ahs)\"/><text class=\"lb\" x=\"117.3\" y=\"110.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">d</text></svg>", "tools": ["calc"]}, {"kind": "blank", "p": "A solid disc has M = 4.0 kg and R = 0.50 m. I_cm = ½MR².", "tag": "", "marks": "", "flat": [{"t": "I_cm = __B1__ kg·m²", "a": {"B1": "0.5"}}, {"t": "I about a parallel axis through the rim = __B1__ kg·m²", "a": {"B1": "1.5"}}, {"t": "Distance d from the centre at which I = 2 × I_cm: d = __B1__ m", "a": {"B1": "0.354"}, "expr": "approx"}], "sol": "½ × 4.0 × 0.25 = 0.50 kg·m².\n0.50 + 4.0 × 0.25 = 1.5 kg·m².\nMd² = 0.50: d² = 0.125, d ≈ 0.354 m.", "tools": ["calc"]}, {"kind": "blank", "p": "A uniform rod has mass M and length L, with I_cm = ML²/12. Use the parallel-axis theorem for an axis through one end.", "tag": "", "marks": "", "flat": [{"t": "d = __B1__ (in terms of L)", "a": {"B1": "L/2"}, "expr": true}, {"t": "I_end = __B1__ (in terms of M and L)", "a": {"B1": "ML^2/3"}, "expr": true}], "sol": "The end is half a length from the centre: d = L/2.\nML²/12 + M(L/2)² = ML²/12 + 3ML²/12 = ML²/3."}]}, {"id": "s8", "label": "5.5.A (i)", "sub": "Balanced torques — LO 5.5.A: describe the conditions under which a system's angular velocity remains constant.", "slides": [{"kind": "mcq", "text": "A rigid system is in rotational equilibrium. What must be true?", "opts": ["The net torque on it is zero", "No forces act on it", "Its angular velocity is zero", "The net force on it is zero"], "correct": 0, "tag": "", "sol": "Rotational equilibrium means Στ = 0; it may still be spinning at constant ω."}, {"kind": "mcq", "text": "A wheel spins at a constant 10 rad/s. What is the net torque on it?", "opts": ["10 N·m", "it cannot be found without I", "clockwise, to keep it turning", "zero"], "correct": 3, "tag": "", "sol": "Constant ω ⇒ α = 0 ⇒ Στ = 0 (rotational form of Newton's first law)."}, {"kind": "mcq", "text": "A 30 kg child sits 1.5 m from the pivot of a see-saw. How far from the pivot must a 45 kg child sit to balance it?", "opts": ["1.5 m", "2.25 m", "0.67 m", "1.0 m"], "correct": 3, "tag": "", "sol": "300 × 1.5 = 450 × d, d = 1.0 m."}, {"kind": "mcq", "text": "Can a system be in rotational equilibrium without being in translational equilibrium?", "opts": ["No: zero net torque means zero net force", "Yes: a non-spinning ball in free fall has zero net torque but a net force", "Yes, but only if it is at rest", "No: the two always happen together"], "correct": 1, "tag": "", "sol": "The two conditions are independent."}, {"kind": "mcq", "text": "Can the net force on a rigid body be zero while the net torque is not?", "opts": ["Only if the body is at rest", "Only if the body is moving", "No, never", "Yes: two equal and opposite forces along different lines (a couple)"], "correct": 3, "tag": "", "sol": "A couple has ΣF = 0 but Στ ≠ 0 (e.g. turning a steering wheel with both hands)."}, {"kind": "mcq", "text": "A metre rule is balanced at its 50 cm mark. A 0.20 N weight hangs at the 20 cm mark. Where must a 0.30 N weight hang to balance it?", "opts": ["at the 80 cm mark", "at the 30 cm mark", "at the 60 cm mark", "at the 70 cm mark"], "correct": 3, "tag": "", "sol": "0.20 × 30 cm = 0.30 × d, d = 20 cm to the right: the 70 cm mark."}, {"kind": "mcq", "text": "The net torque on a spinning top is zero (ignore friction). The top will", "opts": ["speed up", "stop at once", "slow down", "keep spinning at a constant angular velocity"], "correct": 3, "tag": "", "sol": "Στ = 0 ⇒ ω constant."}, {"kind": "mcq", "text": "A light rod is pivoted at its centre. A 20 N weight hangs 0.30 m to the left of the pivot and a 10 N weight 0.50 m to the right. The rod will", "opts": ["rise without rotating", "start to rotate clockwise", "stay balanced", "start to rotate counterclockwise (left end down)"], "correct": 3, "tag": "", "sol": "Left torque 6.0 N·m (ccw) > right torque 5.0 N·m (cw)."}, {"kind": "blank", "p": "A see-saw is pivoted at its middle. Rahul (45 kg) sits 1.6 m to the left of the pivot and Priya (36 kg) sits on the right.", "tag": "", "marks": "", "flat": [{"t": "Torque of Rahul's weight = __B1__ N·m", "a": {"B1": "720"}}, {"t": "Priya must sit __B1__ m from the pivot.", "a": {"B1": "2"}}, {"t": "Rahul's little brother (18 kg) now sits 0.80 m to the left. Priya must move to __B1__ m.", "a": {"B1": "2.4"}}], "sol": "450 × 1.6 = 720 N·m.\n720 ÷ 360 = 2.0 m.\n(720 + 180 × 0.80) ÷ 360 = 2.4 m.", "tools": ["calc"]}, {"kind": "blank", "p": "A uniform metre rule of weight 1.0 N is pivoted at its 40 cm mark. A weight W hung at the 10 cm mark keeps it balanced.", "tag": "", "marks": "", "flat": [{"t": "Torque of the rule's weight about the pivot = __B1__ N·m", "a": {"B1": "0.1"}}, {"t": "Lever arm of W = __B1__ m", "a": {"B1": "0.3"}}, {"t": "W = __B1__ N", "a": {"B1": "0.333"}, "expr": "approx"}], "sol": "Its weight acts at 50 cm, 0.10 m from the pivot: 1.0 × 0.10 = 0.10 N·m.\n40 − 10 = 30 cm = 0.30 m.\nW × 0.30 = 0.10, W ≈ 0.333 N.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 110\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect class=\"sh\" x=\"32.0\" y=\"31.0\" width=\"256.0\" height=\"10\"/><path class=\"sh3\" d=\"M134.4,41.0 L123.4,59.0 L145.4,59.0 Z\"/><line class=\"ln\" x1=\"57.6\" y1=\"41.0\" x2=\"57.6\" y2=\"66.0\"/><rect class=\"sh2\" x=\"37.6\" y=\"66.0\" width=\"40\" height=\"20\"/><text class=\"lb\" x=\"57.6\" y=\"76.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">W</text><text class=\"lb\" x=\"160.0\" y=\"20.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">50 cm</text></svg>", "tools": ["calc"]}, {"kind": "blank", "p": "A ceiling fan turns at a steady 12 rad/s while its motor exerts a torque of 0.90 N·m.", "tag": "", "marks": "", "flat": [{"t": "Magnitude of the air-resistance torque = __B1__ N·m", "a": {"B1": "0.9"}}, {"t": "Net torque = __B1__ N·m", "a": {"B1": "0"}}, {"t": "The fan's angular velocity __B1__ (increases / decreases / stays the same).", "a": {"B1": "stays the same"}, "expr": "words", "accept": ["constant", "same"]}, {"t": "When the motor is switched off, the fan __B1__ (speeds up / slows down / keeps turning steadily).", "a": {"B1": "slows down"}, "expr": "words", "accept": ["slows", "decelerates"]}], "sol": "Steady ω: the torques balance, 0.90 N·m.\nΣτ = 0.\nZero net torque: ω is constant.\nOnly the air torque is left, opposite to ω."}]}, {"id": "s9", "label": "5.5.A (ii)", "sub": "Static equilibrium — LO 5.5.A: describe the conditions under which a system's angular velocity remains constant (forces and torques together).", "slides": [{"kind": "mcq", "text": "For a rigid body to be in static equilibrium,", "opts": ["the net torque about the centre of mass only must be zero", "the net force and the net torque about any axis must both be zero", "only the net torque must be zero", "only the net force must be zero"], "correct": 1, "tag": "", "sol": "ΣF = 0 (no linear acceleration) and Στ = 0 (no angular acceleration)."}, {"kind": "mcq", "text": "A uniform 200 N plank rests on supports at its two ends. A 400 N person stands at the middle. How hard does each support push up?", "opts": ["200 N", "600 N", "400 N", "300 N"], "correct": 3, "tag": "", "sol": "By symmetry the 600 N total is shared equally."}, {"kind": "mcq", "text": "The same plank (length L, 200 N) has the 400 N person standing L/4 from the left end. What is the force from the left support?", "opts": ["200 N", "300 N", "500 N", "400 N"], "correct": 3, "tag": "", "sol": "Torques about the right end: N_L·L = 200(L/2) + 400(3L/4) = 400L, so N_L = 400 N (and N_R = 200 N)."}, {"kind": "mcq", "text": "When solving an equilibrium problem, a good choice of axis for torques is", "opts": ["always the centre of the object", "the point where an unknown force acts, so its torque is zero", "it does not matter, so pick one at random — the answer changes with the axis", "always one end of the object"], "correct": 1, "tag": "", "sol": "Any axis works, but choosing one on an unknown force removes it from the torque equation."}, {"kind": "mcq", "text": "A uniform beam is hinged to a wall at one end and held horizontal by a vertical rope at its other end. The rope tension equals", "opts": ["zero", "twice the beam's weight", "the beam's weight", "half the beam's weight"], "correct": 3, "tag": "", "sol": "Torques about the hinge: T·L = W·L/2."}, {"kind": "mcq", "text": "A painter climbs a ladder that leans on a smooth wall. As she climbs higher, the friction force needed at the floor", "opts": ["becomes zero", "stays the same", "increases", "decreases"], "correct": 2, "tag": "", "sol": "Her weight's lever arm about the foot grows, so the wall force (and the friction that balances it) must grow."}, {"kind": "mcq", "text": "A diving board is bolted at its left end and rests on a support 1.0 m from that end. A diver stands at the far right end. The bolt at the left end pushes on the board", "opts": ["upward", "downward", "with zero force", "sideways"], "correct": 1, "tag": "", "sol": "About the support, the diver's torque must be balanced by a downward pull at the left end."}, {"kind": "mcq", "text": "A mobile rod 0.60 m long has 2.0 N at its left end and 4.0 N at its right end. Where must the string be tied to keep it level (ignore the rod's weight)?", "opts": ["0.30 m from the left end", "0.20 m from the left end", "0.40 m from the left end", "0.45 m from the left end"], "correct": 2, "tag": "", "sol": "2.0x = 4.0(0.60 − x), x = 0.40 m."}, {"kind": "blank", "p": "A uniform 4.0 m bridge plank of weight 600 N rests on supports at its ends A (left) and B (right). A 900 N cart stands 1.0 m from A.", "tag": "", "marks": "", "flat": [{"t": "Torque of the plank's weight about A = __B1__ N·m", "a": {"B1": "1200"}}, {"t": "N_B = __B1__ N", "a": {"B1": "525"}}, {"t": "N_A = __B1__ N", "a": {"B1": "975"}}], "sol": "600 × 2.0 = 1200 N·m.\nAbout A: 4.0 N_B = 1200 + 900 × 1.0, N_B = 525 N.\nΣF = 0: 1500 − 525 = 975 N.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 130\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect class=\"sh\" x=\"32.0\" y=\"51.0\" width=\"256.0\" height=\"10\"/><line class=\"ln\" x1=\"96.0\" y1=\"61.0\" x2=\"96.0\" y2=\"86.0\"/><rect class=\"sh2\" x=\"76.0\" y=\"86.0\" width=\"40\" height=\"20\"/><text class=\"lb\" x=\"96.0\" y=\"96.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">900 N</text><line class=\"arm\" x1=\"32.0\" y1=\"51.0\" x2=\"32.0\" y2=\"17.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"32.0\" y=\"5.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">N<tspan dy=\"3\" font-size=\"10\">A</tspan></text><line class=\"arm\" x1=\"288.0\" y1=\"51.0\" x2=\"288.0\" y2=\"17.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"288.0\" y=\"5.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">N<tspan dy=\"3\" font-size=\"10\">B</tspan></text><text class=\"lb\" x=\"160.0\" y=\"40.0\" text-anchor=\"middle\" dominant-baseline=\"middle\"></text></svg>", "tools": ["calc"]}, {"kind": "blank", "p": "A uniform 1.2 m arm of weight 40 N is hinged to a wall at one end and held horizontal by a vertical cable at its far end. A 60 N lamp hangs from the far end.", "tag": "", "marks": "", "flat": [{"t": "Cable tension = __B1__ N", "a": {"B1": "80"}}, {"t": "Size of the hinge force = __B1__ N", "a": {"B1": "20"}}, {"t": "The hinge force points __B1__ (up / down).", "a": {"B1": "up"}, "expr": "words", "accept": ["upward", "upwards"]}], "sol": "About the hinge: 1.2T = 40 × 0.60 + 60 × 1.2 = 96, T = 80 N.\n40 + 60 − 80 = 20 N.\nThe cable supplies less than the total weight, so the hinge pushes up.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 130\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect x=\"20.0\" y=\"12\" width=\"12\" height=\"92\" style=\"fill:var(--paper-2);stroke:var(--ink);stroke-width:1.2\"/><rect class=\"sh\" x=\"32.0\" y=\"53.0\" width=\"256.0\" height=\"10\"/><line class=\"ln\" x1=\"288.0\" y1=\"63.0\" x2=\"288.0\" y2=\"88.0\"/><rect class=\"sh2\" x=\"268.0\" y=\"88.0\" width=\"40\" height=\"20\"/><text class=\"lb\" x=\"288.0\" y=\"98.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">60 N</text><line class=\"arm\" x1=\"288.0\" y1=\"53.0\" x2=\"288.0\" y2=\"15.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"288.0\" y=\"3.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">T</text></svg>", "tools": ["calc"]}, {"kind": "blank", "p": "A uniform 2.5 m ladder of weight 240 N leans against a smooth wall with its foot 1.5 m from the wall (top 2.0 m up).", "tag": "", "marks": "", "flat": [{"t": "Torque of the weight about the foot = __B1__ N·m", "a": {"B1": "180"}}, {"t": "Wall force N₂ = __B1__ N", "a": {"B1": "90"}}, {"t": "Friction at the floor = __B1__ N", "a": {"B1": "90"}}, {"t": "Floor normal force N₁ = __B1__ N", "a": {"B1": "240"}}], "sol": "Lever arm of W = 1.5 ÷ 2 = 0.75 m: 240 × 0.75 = 180 N·m.\nN₂ × 2.0 = 180, N₂ = 90 N.\nHorizontal forces balance: f = 90 N.\nVertical forces balance: N₁ = 240 N.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 260 210\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line x1=\"10\" y1=\"190\" x2=\"220\" y2=\"190\" style=\"stroke:var(--ink);stroke-width:2\"/><line x1=\"220\" y1=\"190\" x2=\"220\" y2=\"14\" style=\"stroke:var(--ink);stroke-width:2\"/><line x1=\"100\" y1=\"190\" x2=\"220\" y2=\"30\" style=\"stroke:var(--gold);stroke-width:7;stroke-linecap:round\"/><line class=\"arm\" x1=\"160.0\" y1=\"110.0\" x2=\"160.0\" y2=\"154.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"172.0\" y=\"150.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">W</text><line class=\"arm\" x1=\"220.0\" y1=\"30.0\" x2=\"176.0\" y2=\"30.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"170.0\" y=\"18.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">N₂</text><line class=\"arm\" x1=\"100.0\" y1=\"190.0\" x2=\"100.0\" y2=\"144.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"88.0\" y=\"140.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">N₁</text><line class=\"arm\" x1=\"100.0\" y1=\"190.0\" x2=\"140.0\" y2=\"190.0\" marker-end=\"url(#ah)\"/><text class=\"al\" x=\"144.0\" y=\"202.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">f</text></svg>", "tools": ["calc"]}]}, {"id": "s10", "label": "5.6.A (i)", "sub": "Net torque and angular acceleration — LO 5.6.A: describe the conditions under which a system's angular velocity changes.", "slides": [{"kind": "mcq", "text": "The net torque on a wheel is doubled; its rotational inertia is unchanged. Its angular acceleration", "opts": ["halves", "is unchanged", "becomes four times as large", "doubles"], "correct": 3, "tag": "", "sol": "α = Στ ÷ I ∝ Στ."}, {"kind": "mcq", "text": "The same torque acts on a hoop and on a solid disc of equal mass and radius. Which has the greater angular acceleration?", "opts": ["the hoop", "they are equal", "the disc, because its rotational inertia is smaller", "it depends on how fast they spin"], "correct": 2, "tag": "", "sol": "α = τ ÷ I and I_disc < I_hoop."}, {"kind": "mcq", "text": "A wheel with I = 0.50 kg·m² has a net torque of 2.0 N·m. What is its angular acceleration?", "opts": ["4.0 rad/s²", "1.0 rad/s²", "2.5 rad/s²", "0.25 rad/s²"], "correct": 0, "tag": "", "sol": "α = 2.0 ÷ 0.50 = 4.0 rad/s²."}, {"kind": "mcq", "text": "A net torque acts opposite to a wheel's angular velocity. The wheel", "opts": ["spins at constant ω", "slows down", "reverses instantly", "speeds up"], "correct": 1, "tag": "", "sol": "α is opposite to ω, so the angular speed decreases."}, {"kind": "mcq", "text": "A grindstone (I = 2.0 kg·m²) is slowed by friction from 30 rad/s to rest in 10 s. What is the friction torque?", "opts": ["3.0 N·m", "60 N·m", "0.67 N·m", "6.0 N·m"], "correct": 3, "tag": "", "sol": "α = 30 ÷ 10 = 3.0 rad/s²; τ = Iα = 6.0 N·m.", "tools": ["calc"]}, {"kind": "mcq", "text": "A torque of 12 N·m gives a flywheel an angular acceleration of 3.0 rad/s². What is its rotational inertia?", "opts": ["0.25 kg·m²", "15 kg·m²", "4.0 kg·m²", "36 kg·m²"], "correct": 2, "tag": "", "sol": "I = τ ÷ α = 12 ÷ 3.0 = 4.0 kg·m²."}, {"kind": "mcq", "text": "Arjun says: “A bigger force always gives a bigger angular acceleration.” Is he right?", "opts": ["No: α depends on the torque (force and lever arm) and on I", "Yes: α is proportional to F", "Yes, as long as the force is perpendicular", "No: α depends only on the mass"], "correct": 0, "tag": "", "sol": "A large force through the axis gives no torque; the same torque gives less α for a larger I."}, {"kind": "mcq", "text": "A wheel initially at rest has a constant net torque on it. Which graph describes its motion?", "opts": ["ω–t is a horizontal line", "ω–t is a straight line through the origin", "θ–t is a straight line through the origin", "α–t is a straight line through the origin"], "correct": 1, "tag": "", "sol": "Constant torque ⇒ constant α ⇒ ω rises linearly from zero."}, {"kind": "blank", "p": "A potter's wheel (I = 2.5 kg·m²) starts from rest. A constant net torque of 5.0 N·m acts for 4.0 s.", "tag": "", "marks": "", "flat": [{"t": "α = __B1__ rad/s²", "a": {"B1": "2"}}, {"t": "ω after 4.0 s = __B1__ rad/s", "a": {"B1": "8"}}, {"t": "Angle turned = __B1__ rad", "a": {"B1": "16"}}], "sol": "α = 5.0 ÷ 2.5 = 2.0 rad/s².\nω = 2.0 × 4.0 = 8.0 rad/s.\nθ = ½ × 2.0 × 16 = 16 rad.", "tools": ["calc"]}, {"kind": "blank", "p": "A bicycle wheel (I = 0.12 kg·m²) spinning at 20 rad/s is stopped by a brake pad that rubs at radius 0.30 m with a friction force of 2.0 N.", "tag": "", "marks": "", "flat": [{"t": "Friction torque = __B1__ N·m", "a": {"B1": "0.6"}}, {"t": "Magnitude of α = __B1__ rad/s²", "a": {"B1": "5"}}, {"t": "Time to stop = __B1__ s", "a": {"B1": "4"}}], "sol": "0.30 × 2.0 = 0.60 N·m.\n0.60 ÷ 0.12 = 5.0 rad/s².\n20 ÷ 5.0 = 4.0 s.", "tools": ["calc"]}, {"kind": "blank", "p": "Different net torques are applied to the same flywheel.\nτ (N·m): 2, 4, 6, 8   →   α (rad/s²): 0.5, 1.0, 1.5, 2.0", "tag": "", "marks": "", "flat": [{"t": "α ÷ τ for each row = __B1__", "a": {"B1": "0.25"}}, {"t": "Rotational inertia of the flywheel = __B1__ kg·m²", "a": {"B1": "4"}}, {"t": "α is __B1__ to τ (directly proportional / inversely proportional).", "a": {"B1": "directly proportional"}, "expr": "words", "accept": ["proportional"]}], "sol": "0.5 ÷ 2 = … = 2.0 ÷ 8 = 0.25.\nI = τ ÷ α = 1 ÷ 0.25 = 4 kg·m².\nStraight line through the origin.", "tools": ["calc", "desmos"], "desmos": [{"type": "table", "columns": [{"latex": "x_1", "values": ["2", "4", "6", "8"]}, {"latex": "y_1", "values": ["0.5", "1", "1.5", "2"]}]}, "y_1\\sim ax_1"]}]}, {"id": "s11", "label": "5.6.A (ii)", "sub": "Linked rotation and translation — LO 5.6.A: describe the conditions under which a system's angular velocity changes, for systems that both rotate and translate.", "slides": [{"kind": "mcq", "text": "A block hangs from a string wound round a pulley that has mass. Compared with free fall, the block's acceleration is", "opts": ["less than g, because the string tension must also spin the pulley", "equal to g", "greater than g", "zero"], "correct": 0, "tag": "", "sol": "The tension slows the block and gives the pulley its torque."}, {"kind": "mcq", "text": "A string runs over a pulley of radius R without slipping. The block's acceleration a and the pulley's angular acceleration α are related by", "opts": ["a = α", "a = R²α", "a = Rα", "a = α ÷ R"], "correct": 2, "tag": "", "sol": "a_T = rα at the rim equals the string's acceleration."}, {"kind": "mcq", "text": "A block falls while pulling a string off a heavy pulley. The string tension is", "opts": ["less than the block's weight", "zero", "equal to the block's weight", "greater than the block's weight"], "correct": 0, "tag": "", "sol": "The block accelerates downward, so mg − T = ma > 0."}, {"kind": "mcq", "text": "An Atwood machine has a heavy pulley. While the masses accelerate, the tensions in the string on the two sides are", "opts": ["different, because a net torque is needed to give the pulley α", "each equal to the weight hanging on that side", "equal", "both zero"], "correct": 0, "tag": "", "sol": "(T₁ − T₂)R = Iα ≠ 0."}, {"kind": "mcq", "text": "A solid-disc pulley (M = 2.0 kg, R = 0.10 m, I = ½MR² = 0.010 kg·m²) has a string tension of 5.0 N at its rim. Its angular acceleration is", "opts": ["5.0 rad/s²", "0.50 rad/s²", "500 rad/s²", "50 rad/s²"], "correct": 3, "tag": "", "sol": "τ = 5.0 × 0.10 = 0.50 N·m; α = 0.50 ÷ 0.010 = 50 rad/s².", "tools": ["calc"]}, {"kind": "mcq", "text": "To fully describe a yo-yo falling as its string unwinds, you need", "opts": ["only the kinematic equations", "ΣF = ma for its centre and Στ = Iα for its rotation", "only ΣF = ma", "only Στ = Iα"], "correct": 1, "tag": "", "sol": "Linear and rotational analyses are done separately and then linked."}, {"kind": "mcq", "text": "A block falls 1 m from rest, first with a massless pulley and then with a heavy pulley. With the heavy pulley, its speed after 1 m is", "opts": ["larger", "zero", "smaller", "the same"], "correct": 2, "tag": "", "sol": "Its acceleration is smaller, so it reaches a smaller speed over the same distance."}, {"kind": "mcq", "text": "A rope wrapped round a drum (radius 0.20 m, I = 0.40 kg·m², frictionless axle) is pulled with a steady 10 N. What is the acceleration of the rope?", "opts": ["0.20 m/s²", "1.0 m/s²", "25 m/s²", "5.0 m/s²"], "correct": 1, "tag": "", "sol": "τ = 2.0 N·m, α = 5.0 rad/s², a = Rα = 1.0 m/s².", "tools": ["calc"]}, {"kind": "blank", "p": "A 3.0 kg bucket hangs from a rope wound round a well's drum (solid cylinder, M = 2.0 kg, R = 0.20 m, I = ½MR²) and is released from rest.", "tag": "", "marks": "", "flat": [{"t": "I of the drum = __B1__ kg·m²", "a": {"B1": "0.04"}}, {"t": "Acceleration of the bucket = __B1__ m/s²", "a": {"B1": "7.5"}}, {"t": "Rope tension = __B1__ N", "a": {"B1": "7.5"}}, {"t": "Angular acceleration of the drum = __B1__ rad/s²", "a": {"B1": "37.5"}}], "sol": "½ × 2.0 × 0.04 = 0.040 kg·m².\nBucket: 30 − T = 3a; drum: T = (I/R²)a = 1.0a ⇒ 30 = 4a, a = 7.5 m/s².\nT = 1.0 × 7.5 = 7.5 N.\nα = a ÷ R = 7.5 ÷ 0.20 = 37.5 rad/s².", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 260 200\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line x1=\"70.0\" y1=\"10\" x2=\"190.0\" y2=\"10\" style=\"stroke:var(--ink);stroke-width:2.4\"/><line class=\"ln\" x1=\"130.0\" y1=\"10.0\" x2=\"130.0\" y2=\"52.0\"/><circle class=\"sh3\" cx=\"130.0\" cy=\"52\" r=\"26\"/><circle class=\"pt\" cx=\"130.0\" cy=\"52.0\" r=\"2.8\"/><line class=\"hid\" x1=\"130.0\" y1=\"52.0\" x2=\"111.5\" y2=\"70.5\"/><text class=\"lb\" x=\"86.0\" y=\"56.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">R</text><line class=\"ln\" x1=\"156.0\" y1=\"52.0\" x2=\"156.0\" y2=\"138.0\"/><rect class=\"sh2\" x=\"134.0\" y=\"138.0\" width=\"44\" height=\"28\"/><text class=\"lb\" x=\"156.0\" y=\"152.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">3.0 kg</text></svg>", "tools": ["calc"]}, {"kind": "blank", "p": "A block of mass m hangs from a string wound on a pulley of rotational inertia I and radius R.", "tag": "", "marks": "", "flat": [{"t": "Block: mg − T = ma. Pulley: TR = I(a/R). So T = __B1__ (in terms of I, R and a)", "a": {"B1": "Ia/R^2"}, "expr": true}, {"t": "a = __B1__ (in terms of m, g, I and R)", "a": {"B1": "mg/(m+I/R^2)"}, "expr": true, "accept": ["mgR^2/(mR^2+I)"]}, {"t": "If the pulley's I is increased, a __B1__ (increases / decreases / stays the same).", "a": {"B1": "decreases"}, "expr": "words", "accept": ["decrease"]}], "sol": "T = Iα ÷ R = Ia ÷ R².\nmg − Ia/R² = ma ⇒ a = mg ÷ (m + I/R²).\nA larger I makes the denominator larger."}, {"kind": "blank", "p": "An Atwood machine has m₁ = 3.0 kg and m₂ = 1.0 kg on a pulley with R = 0.10 m and I = 0.020 kg·m² (so I/R² = 2.0 kg).", "tag": "", "marks": "", "flat": [{"t": "Acceleration = __B1__ m/s²", "a": {"B1": "3.33"}, "expr": "approx"}, {"t": "Tension on the 3.0 kg side = __B1__ N", "a": {"B1": "20"}}, {"t": "Tension on the 1.0 kg side = __B1__ N", "a": {"B1": "13.3"}, "expr": "approx"}, {"t": "Net torque on the pulley = __B1__ N·m", "a": {"B1": "0.667"}, "expr": "approx"}], "sol": "a = (3.0 − 1.0)g ÷ (3.0 + 1.0 + 2.0) = 20 ÷ 6 ≈ 3.33 m/s².\nT₁ = 3.0(10 − 3.33) = 20 N.\nT₂ = 1.0(10 + 3.33) ≈ 13.3 N.\n(20 − 13.3) × 0.10 ≈ 0.667 N·m (= Iα = 0.020 × 33.3).", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 260 200\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line x1=\"70.0\" y1=\"10\" x2=\"190.0\" y2=\"10\" style=\"stroke:var(--ink);stroke-width:2.4\"/><line class=\"ln\" x1=\"130.0\" y1=\"10.0\" x2=\"130.0\" y2=\"52.0\"/><circle class=\"sh3\" cx=\"130.0\" cy=\"52\" r=\"26\"/><circle class=\"pt\" cx=\"130.0\" cy=\"52.0\" r=\"2.8\"/><line class=\"ln\" x1=\"156.0\" y1=\"52.0\" x2=\"156.0\" y2=\"148.0\"/><rect class=\"sh2\" x=\"134.0\" y=\"148.0\" width=\"44\" height=\"28\"/><text class=\"lb\" x=\"156.0\" y=\"162.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">3.0 kg</text><line class=\"ln\" x1=\"104.0\" y1=\"52.0\" x2=\"104.0\" y2=\"112.0\"/><rect class=\"sh2\" x=\"82.0\" y=\"112.0\" width=\"44\" height=\"28\"/><text class=\"lb\" x=\"104.0\" y=\"126.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1.0 kg</text></svg>", "tools": ["calc"]}]}, {"id": "s12", "label": "Test A", "sub": "Test A — Knowing and understanding", "slides": [{"kind": "mcq", "text": "What is the SI unit of angular acceleration?", "opts": ["rad/s", "m/s²", "rad/s²", "rev/min"], "correct": 2, "tag": "", "sol": "α = Δω ÷ Δt: (rad/s) ÷ s. [5.1.A]"}, {"kind": "mcq", "text": "A point on the rim of a 0.50 m radius wheel turning at 8.0 rad/s moves at", "opts": ["8.0 m/s", "16 m/s", "4.0 m/s", "0.063 m/s"], "correct": 2, "tag": "", "sol": "v = rω = 0.50 × 8.0 = 4.0 m/s. [5.2.A]"}, {"kind": "mcq", "text": "A 40 N force acts perpendicular to a 0.25 m lever at its end. The torque is", "opts": ["10 N·m", "40 N·m", "0.0063 N·m", "160 N·m"], "correct": 0, "tag": "", "sol": "τ = rF = 0.25 × 40 = 10 N·m. [5.3.B]"}, {"kind": "mcq", "text": "Which change increases a spinning ice-skater's rotational inertia?", "opts": ["spinning faster", "spinning slower", "pulling her arms in", "stretching her arms out"], "correct": 3, "tag": "", "sol": "Moving mass farther from the axis increases I. [5.4.A]"}, {"kind": "mcq", "text": "A rod has I_cm = 0.20 kg·m² and mass 2.0 kg. What is I about a parallel axis 0.50 m from its centre?", "opts": ["1.2 kg·m²", "0.20 kg·m²", "0.70 kg·m²", "0.50 kg·m²"], "correct": 2, "tag": "", "sol": "0.20 + 2.0 × 0.25 = 0.70 kg·m². [5.4.B]", "tools": ["calc"]}, {"kind": "mcq", "text": "A rigid system keeps a constant angular velocity when", "opts": ["the net torque on it is zero", "a constant torque acts on it", "the net force on it is zero", "it has no rotational inertia"], "correct": 0, "tag": "", "sol": "Rotational form of Newton's first law. [5.5.A]"}, {"kind": "mcq", "text": "A net torque of 9.0 N·m acts on a wheel with I = 3.0 kg·m². Its angular acceleration is", "opts": ["0.33 rad/s²", "3.0 rad/s²", "12 rad/s²", "27 rad/s²"], "correct": 1, "tag": "", "sol": "α = 9.0 ÷ 3.0 = 3.0 rad/s². [5.6.A]"}, {"kind": "mcq", "text": "Which force exerts NO torque about a door's hinges?", "opts": ["a push near the hinges, perpendicular to the door", "a push along the door directly towards the hinges", "a push at the handle, perpendicular to the door", "a push at the handle at 45° to the door"], "correct": 1, "tag": "", "sol": "Its line of action passes through the axis. [5.3.A]"}, {"kind": "blank", "p": "A turntable (I = 0.050 kg·m²) speeds up uniformly from rest to 4.0 rad/s in 2.0 s.", "tag": "", "marks": "", "flat": [{"t": "α = __B1__ rad/s²", "a": {"B1": "2"}}, {"t": "Net torque = __B1__ N·m", "a": {"B1": "0.1"}}, {"t": "Angle turned = __B1__ rad", "a": {"B1": "4"}}], "sol": "4.0 ÷ 2.0 = 2.0 rad/s². [5.1.A]\nτ = Iα = 0.050 × 2.0 = 0.10 N·m. [5.6.A]\nθ = ½ × 2.0 × 4.0 = 4.0 rad. [5.1.A]", "tools": ["calc"]}, {"kind": "blank", "p": "A 50 kg adult sits 1.2 m from the pivot of a see-saw.", "tag": "", "marks": "", "flat": [{"t": "A 20 kg child balances it by sitting __B1__ m from the pivot.", "a": {"B1": "3"}}, {"t": "The child sits on the __B1__ side of the pivot (same / opposite).", "a": {"B1": "opposite"}, "expr": "words", "accept": ["other"]}], "sol": "500 × 1.2 = 200 × d, d = 3.0 m. [5.5.A]\nHer torque must be in the other sense. [5.5.A]", "tools": ["calc"]}]}, {"id": "s13", "label": "Test B", "sub": "Test B — Investigating patterns", "slides": [{"kind": "blank", "p": "A constant torque spins a wheel up from rest.\nt (s): 0, 1, 2, 3, 4   →   ω (rad/s): 0, 1.5, 3.0, 4.5, 6.0", "tag": "", "marks": "", "flat": [{"t": "α = __B1__ rad/s²", "a": {"B1": "1.5"}}, {"t": "ω = __B1__ (in terms of t)", "a": {"B1": "1.5t"}, "expr": true}, {"t": "Angle turned in the 4 s = __B1__ rad", "a": {"B1": "12"}}], "sol": "Slope = 1.5 rad/s² (constant). [5.1.A]\nω = 1.5t.\nArea = ½ × 4 × 6.0 = 12 rad.", "tools": ["calc", "desmos"], "desmos": [{"type": "table", "columns": [{"latex": "x_1", "values": ["0", "1", "2", "3", "4"]}, {"latex": "y_1", "values": ["0", "1.5", "3", "4.5", "6"]}]}, "y_1\\sim ax_1"]}, {"kind": "blank", "p": "The rotational inertia of a 0.50 kg ball is measured at different distances from an axis.\nr (m): 0.1, 0.2, 0.3, 0.4   →   I (kg·m²): 0.005, 0.020, 0.045, 0.080", "tag": "", "marks": "", "flat": [{"t": "I ÷ r² for each row = __B1__ kg", "a": {"B1": "0.5"}}, {"t": "When r doubles, I is multiplied by __B1__", "a": {"B1": "4"}}, {"t": "Predicted I at r = 0.60 m = __B1__ kg·m²", "a": {"B1": "0.18"}}], "sol": "0.005 ÷ 0.01 = … = 0.080 ÷ 0.16 = 0.50 kg (the mass). [5.4.A]\nI ∝ r².\n0.50 × 0.36 = 0.18 kg·m².", "tools": ["calc", "desmos"], "desmos": [{"type": "table", "columns": [{"latex": "x_1", "values": ["0.1", "0.2", "0.3", "0.4"]}, {"latex": "y_1", "values": ["0.005", "0.02", "0.045", "0.08"]}]}, "y_1\\sim ax_1^2"]}, {"kind": "mcq", "text": "A 20 N force acts at 0.50 m from an axis at different angles θ to the position vector: θ = 0°, 30°, 60°, 90° → τ (N·m): 0, 5.0, 8.7, 10. Which relationship fits?", "opts": ["τ ∝ sin θ", "τ ∝ cos θ", "τ ∝ θ", "τ is constant"], "correct": 0, "tag": "", "sol": "10 sin θ gives 0, 5.0, 8.7, 10. [5.3.B]", "tools": ["desmos"], "desmos": ["y=10\\sin(x)"]}, {"kind": "blank", "p": "Different weights W hung at distance d on the right of a pivot each balance the same fixed torque on the left.\nd (m): 0.1, 0.2, 0.3, 0.4   →   W (N): 12, 6, 4, 3", "tag": "", "marks": "", "flat": [{"t": "W × d for each row = __B1__ N·m", "a": {"B1": "1.2"}}, {"t": "W is __B1__ to d (directly proportional / inversely proportional).", "a": {"B1": "inversely proportional"}, "expr": "words", "accept": ["inversely"]}, {"t": "Predicted W at d = 0.60 m = __B1__ N", "a": {"B1": "2"}}], "sol": "12 × 0.1 = 6 × 0.2 = … = 1.2 N·m. [5.5.A]\nConstant product: W ∝ 1/d.\n1.2 ÷ 0.60 = 2.0 N.", "tools": ["calc", "desmos"], "desmos": [{"type": "table", "columns": [{"latex": "x_1", "values": ["0.1", "0.2", "0.3", "0.4"]}, {"latex": "y_1", "values": ["12", "6", "4", "3"]}]}, "y_1\\sim a/x_1"]}, {"kind": "mcq", "text": "The same torque is applied to solid discs of equal mass but different radius: R (m) = 0.1, 0.2, 0.4 → α (rad/s²): 80, 20, 5. Which pattern fits?", "opts": ["α ∝ 1/R", "α ∝ 1/R²", "α ∝ R", "α ∝ R²"], "correct": 1, "tag": "", "sol": "Doubling R divides α by 4: I = ½MR² ∝ R². [5.6.A]"}, {"kind": "blank", "p": "A flywheel slows down under a constant friction torque.\nt (s): 0, 2, 4, 6   →   ω (rad/s): 24, 18, 12, 6", "tag": "", "marks": "", "flat": [{"t": "α = __B1__ rad/s²", "a": {"B1": "-3"}}, {"t": "It stops at t = __B1__ s", "a": {"B1": "8"}}, {"t": "Total angle turned before stopping = __B1__ rad", "a": {"B1": "96"}}], "sol": "Slope = −6 ÷ 2 = −3 rad/s². [5.1.A]\n24 ÷ 3 = 8 s.\n½ × 24 × 8 = 96 rad.", "tools": ["calc", "desmos"], "desmos": [{"type": "table", "columns": [{"latex": "x_1", "values": ["0", "2", "4", "6"]}, {"latex": "y_1", "values": ["24", "18", "12", "6"]}]}, "y_1\\sim mx_1+b"]}, {"kind": "mcq", "text": "Points on a wheel turning at a steady rate have these speeds: r (m) 0.1, 0.2, 0.3 → v (m/s) 0.4, 0.8, 1.2. A graph of v against r is", "opts": ["a horizontal line", "a straight line through the origin with slope 4 rad/s", "a curve getting steeper", "a straight line through the origin with slope 0.25 rad/s"], "correct": 1, "tag": "", "sol": "v = rω with ω = 4 rad/s. [5.2.A]"}, {"kind": "mcq", "text": "The rotational inertia of an object is measured about parallel axes a distance d from its centre of mass: d (m) = 0, 0.1, 0.2, 0.3 → I (kg·m²): 0.10, 0.12, 0.18, 0.28. What is its mass?", "opts": ["2.0 kg", "0.10 kg", "0.20 kg", "20 kg"], "correct": 0, "tag": "", "sol": "I − I_cm = Md²: 0.02 ÷ 0.01 = 0.08 ÷ 0.04 = 0.18 ÷ 0.09 = 2.0 kg. [5.4.B]", "tools": ["calc"]}]}, {"id": "s14", "label": "Test C", "sub": "Test C — Communicating", "slides": [{"kind": "mcq", "text": "Which is a correct unit for rotational inertia?", "opts": ["kg/m²", "N·m", "kg·m²", "kg·m"], "correct": 2, "tag": "", "sol": "I = mr²: kg × m². [5.4.A]"}, {"kind": "mcq", "text": "A student calculates τ = rF with r = 30 cm and F = 20 N and writes τ = 600 N·m. What went wrong?", "opts": ["Nothing — it is correct", "The answer should be in N/m", "F should be divided by r", "r must be in metres: τ = 0.30 × 20 = 6.0 N·m"], "correct": 3, "tag": "", "sol": "Convert cm to m before multiplying. [5.3.B]"}, {"kind": "mcq", "text": "Counterclockwise is positive. A wheel turning clockwise and slowing down has", "opts": ["ω > 0 and α > 0", "ω < 0 and α < 0", "ω > 0 and α < 0", "ω < 0 and α > 0"], "correct": 3, "tag": "", "sol": "Clockwise ⇒ ω < 0; slowing ⇒ α opposite to ω. [5.1.A]"}, {"kind": "mcq", "text": "Which statement about a rigid wheel turning about its axle is correct?", "opts": ["The angular acceleration depends on the distance from the axle", "All points on it have the same angular velocity", "Points on the axle have the largest linear speed", "Points farther out have a larger angular velocity"], "correct": 1, "tag": "", "sol": "ω and α are shared; only linear quantities scale with r. [5.2.A]"}, {"kind": "mcq", "text": "F = 12.5 N acts perpendicular to a lever at r = 0.40 m. How should the torque be reported?", "opts": ["5 N·m", "5.000 N·m", "5.00 N·m", "5.0 N·m"], "correct": 3, "tag": "", "sol": "0.40 has 2 significant figures, so give 2: 5.0 N·m. [5.3.B]"}, {"kind": "mcq", "text": "In a force diagram of a uniform horizontal beam on a pivot, the arrow for the beam's weight must start", "opts": ["at the pivot", "anywhere on the beam", "at one end of the beam", "at the beam's centre (its centre of mass)"], "correct": 3, "tag": "", "sol": "Gravity acts at the centre of mass; its position sets the lever arm. [5.3.A]"}, {"kind": "mcq", "text": "A student converts 120 rpm to rad/s as 120 × 2π ≈ 754 rad/s. What is the correct value?", "opts": ["720 rad/s", "4π ≈ 12.6 rad/s", "754 rad/s", "2.0 rad/s"], "correct": 1, "tag": "", "sol": "120 rev/min = 2 rev/s = 4π rad/s. [5.1.A]"}, {"kind": "blank", "p": "A student writes: “A 4.0 N·m torque acts on a disc with I = 0.50 kg·m², so α = τ × I = 2.0 rad/s².”", "tag": "", "marks": "", "flat": [{"t": "Is the student's answer correct? __B1__ (yes / no)", "a": {"B1": "no"}, "expr": "words", "accept": ["n"]}, {"t": "Correct α = __B1__ rad/s²", "a": {"B1": "8"}}, {"t": "The student should __B1__ (multiply / divide).", "a": {"B1": "divide"}, "expr": "words"}], "sol": "τ × I has the wrong units (kg·m²·N·m).\nα = τ ÷ I = 4.0 ÷ 0.50 = 8.0 rad/s². [5.6.A]\nα = Στ ÷ I."}, {"kind": "blank", "p": "Counterclockwise is positive. The ω–t graph of a wheel is shown.", "tag": "", "marks": "", "flat": [{"t": "α = __B1__ rad/s²", "a": {"B1": "-3"}}, {"t": "The wheel reverses direction at t = __B1__ s", "a": {"B1": "2"}}, {"t": "Net angular displacement from 0 to 4 s = __B1__ rad", "a": {"B1": "0"}}, {"t": "At t = 3 s the wheel turns __B1__ (clockwise / counterclockwise).", "a": {"B1": "clockwise"}, "expr": "words", "accept": ["cw"]}], "sol": "Slope = −12 ÷ 4 = −3 rad/s². [5.1.A]\nω = 0 at t = 2 s.\nAreas +6 and −6 cancel.\nω < 0: clockwise.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 320 230\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><line style=\"stroke:var(--rule)\" x1=\"46.0\" y1=\"196\" x2=\"46.0\" y2=\"18\"/><text class=\"po\" x=\"46.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">0</text><line style=\"stroke:var(--rule)\" x1=\"96.8\" y1=\"196\" x2=\"96.8\" y2=\"18\"/><text class=\"po\" x=\"96.8\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><line style=\"stroke:var(--rule)\" x1=\"147.6\" y1=\"196\" x2=\"147.6\" y2=\"18\"/><text class=\"po\" x=\"147.6\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><line style=\"stroke:var(--rule)\" x1=\"198.4\" y1=\"196\" x2=\"198.4\" y2=\"18\"/><text class=\"po\" x=\"198.4\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><line style=\"stroke:var(--rule)\" x1=\"249.2\" y1=\"196\" x2=\"249.2\" y2=\"18\"/><text class=\"po\" x=\"249.2\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">4</text><line style=\"stroke:var(--rule)\" x1=\"300.0\" y1=\"196\" x2=\"300.0\" y2=\"18\"/><text class=\"po\" x=\"300.0\" y=\"209.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">5</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"196.0\" x2=\"300\" y2=\"196.0\"/><text class=\"po\" x=\"40.0\" y=\"196.0\" text-anchor=\"end\" dominant-baseline=\"middle\">-8</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"173.8\" x2=\"300\" y2=\"173.8\"/><text class=\"po\" x=\"40.0\" y=\"173.8\" text-anchor=\"end\" dominant-baseline=\"middle\">-6</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"151.5\" x2=\"300\" y2=\"151.5\"/><text class=\"po\" x=\"40.0\" y=\"151.5\" text-anchor=\"end\" dominant-baseline=\"middle\">-4</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"129.2\" x2=\"300\" y2=\"129.2\"/><text class=\"po\" x=\"40.0\" y=\"129.2\" text-anchor=\"end\" dominant-baseline=\"middle\">-2</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"107.0\" x2=\"300\" y2=\"107.0\"/><text class=\"po\" x=\"40.0\" y=\"107.0\" text-anchor=\"end\" dominant-baseline=\"middle\">0</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"84.8\" x2=\"300\" y2=\"84.8\"/><text class=\"po\" x=\"40.0\" y=\"84.8\" text-anchor=\"end\" dominant-baseline=\"middle\">2</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"62.5\" x2=\"300\" y2=\"62.5\"/><text class=\"po\" x=\"40.0\" y=\"62.5\" text-anchor=\"end\" dominant-baseline=\"middle\">4</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"40.2\" x2=\"300\" y2=\"40.2\"/><text class=\"po\" x=\"40.0\" y=\"40.2\" text-anchor=\"end\" dominant-baseline=\"middle\">6</text><line style=\"stroke:var(--rule)\" x1=\"46\" y1=\"18.0\" x2=\"300\" y2=\"18.0\"/><text class=\"po\" x=\"40.0\" y=\"18.0\" text-anchor=\"end\" dominant-baseline=\"middle\">8</text><line class=\"ln\" x1=\"46.0\" y1=\"196.0\" x2=\"308.0\" y2=\"196.0\" marker-end=\"url(#ah)\"/><line class=\"ln\" x1=\"46.0\" y1=\"196.0\" x2=\"46.0\" y2=\"10.0\" marker-end=\"url(#ah)\"/><text class=\"lb\" x=\"300.0\" y=\"188.0\" text-anchor=\"end\" dominant-baseline=\"middle\">t (s)</text><text class=\"lb\" x=\"52.0\" y=\"12.0\" text-anchor=\"start\" dominant-baseline=\"middle\">ω (rad/s)</text><polyline fill=\"none\" style=\"stroke:var(--danger);stroke-width:2.2\" points=\"46.0,40.2 249.2,173.8\"/><circle cx=\"46.0\" cy=\"40.2\" r=\"3\" style=\"fill:var(--danger)\"/><circle cx=\"249.2\" cy=\"173.8\" r=\"3\" style=\"fill:var(--danger)\"/></svg>"}]}, {"id": "s15", "label": "Test D", "sub": "Test D — Applying physics in real-life contexts", "slides": [{"kind": "mcq", "text": "Why are door handles fitted far from the hinges?", "opts": ["It makes the door lighter", "It reduces friction in the hinges", "A larger lever arm gives more torque for the same push", "It reduces the door's rotational inertia"], "correct": 2, "tag": "", "sol": "τ = r⊥F. [5.3.B]"}, {"kind": "mcq", "text": "A car's wheel nuts must be tightened to 120 N·m. The mechanic's spanner is 0.30 m long. What is the smallest force she can use?", "opts": ["400 N", "0.0025 N", "36 N", "40 N"], "correct": 0, "tag": "", "sol": "F = τ ÷ r = 120 ÷ 0.30 = 400 N (pushing perpendicular at the end). [5.3.B]"}, {"kind": "mcq", "text": "Why does a tightrope walker carry a long, heavy pole?", "opts": ["It increases her rotational inertia, so a torque gives her a smaller angular acceleration and time to recover", "It pulls her centre of mass above the rope", "It makes her lighter", "It reduces friction with the rope"], "correct": 0, "tag": "", "sol": "α = τ ÷ I: larger I, slower tipping. [5.4.A, 5.6.A]"}, {"kind": "mcq", "text": "A student finds that the tip of a ceiling-fan blade (radius 0.60 m) turning at 300 rpm moves at 180 m/s. Is this reasonable?", "opts": ["Yes: fan blades move at the speed of sound", "Yes: v = rω = 0.60 × 300", "No: 300 rpm ≈ 31.4 rad/s, so v ≈ 19 m/s", "No: it should be about 500 m/s"], "correct": 2, "tag": "", "sol": "180 m/s is about 650 km/h — far too fast. The rpm were not converted to rad/s. [5.2.A]", "tools": ["calc"]}, {"kind": "mcq", "text": "A 60 kg cyclist stands with all her weight on one pedal when the 0.17 m crank is horizontal. What torque does she exert about the crank axle?", "opts": ["about 3500 N·m", "60 N·m", "10.2 N·m", "102 N·m"], "correct": 3, "tag": "", "sol": "τ = 600 × 0.17 = 102 N·m. [5.3.B]"}, {"kind": "blank", "p": "A tower crane's jib holds a 2000 kg load 15 m from the tower. The counterweight sits 5.0 m from the tower on the other side. Ignore the jib's own weight.", "tag": "", "marks": "", "flat": [{"t": "Counterweight mass for zero net torque = __B1__ kg", "a": {"B1": "6000"}}, {"t": "The trolley moves the load in to 10 m. Size of the net torque now = __B1__ N·m", "a": {"B1": "100000"}}, {"t": "The crane now tends to tip towards the __B1__ (load / counterweight).", "a": {"B1": "counterweight"}, "expr": "words"}], "sol": "2000 × 15 = m × 5.0, m = 6000 kg. [5.5.A]\n60000 × 5.0 − 20000 × 10 = 300000 − 200000 = 100000 N·m.\nThe counterweight's torque is now larger.", "tools": ["calc"]}, {"kind": "blank", "p": "A ceiling fan (I = 0.30 kg·m²) is switched on. The motor exerts 1.2 N·m and air resistance exerts 0.30 N·m against it (take this as constant).", "tag": "", "marks": "", "flat": [{"t": "Net torque = __B1__ N·m", "a": {"B1": "0.9"}}, {"t": "α = __B1__ rad/s²", "a": {"B1": "3"}}, {"t": "Time to reach 15 rad/s from rest = __B1__ s", "a": {"B1": "5"}}], "sol": "1.2 − 0.30 = 0.90 N·m. [5.6.A]\n0.90 ÷ 0.30 = 3.0 rad/s².\n15 ÷ 3.0 = 5.0 s.", "tools": ["calc"]}, {"kind": "blank", "p": "A 10 kg bucket of water is held still in a village well. Its rope is wound on a drum of radius 0.10 m, turned by a crank handle 0.30 m from the axle.", "tag": "", "marks": "", "flat": [{"t": "Torque of the bucket's weight about the axle = __B1__ N·m", "a": {"B1": "10"}}, {"t": "Force needed at the handle (perpendicular) = __B1__ N", "a": {"B1": "33.3"}, "expr": "approx"}, {"t": "With a longer crank handle, the force needed __B1__ (increases / decreases / stays the same).", "a": {"B1": "decreases"}, "expr": "words", "accept": ["decrease"]}], "sol": "100 × 0.10 = 10 N·m. [5.5.A]\nF × 0.30 = 10, F ≈ 33.3 N.\nLarger lever arm, smaller force.", "tools": ["calc"]}]}];
/* ================= build slide lists ================= */
var SLIDES = {}; SECTIONS.forEach(function(sec){ SLIDES[sec.id]=sec.slides; });
var TAB_DEFS = SECTIONS.map(function(sec){ return {id:sec.id, label:sec.label, sub:sec.sub}; });

/* ================= state ================= */
function blankItem(slide){
  if(slide.kind==='mcq'||slide.kind==='ar'){
    return {status:'unanswered', attempts:0, choice:undefined};
  }
  return {status:'unanswered', curStep:0, stepStates: slide.flat.map(function(){ return {status:'unanswered', attempts:0, inputs:{}}; })};
}
function defaultState(){
  var s={tabs:{}};
  TAB_DEFS.forEach(function(td){
    s.tabs[td.id] = { idx:0, maxReached:0, items: SLIDES[td.id].map(function(sl){ return blankItem(sl); }) };
  });
  return s;
}
var appState = {mode:null, learning:null, quiz:null};
var state = null;      // alias for appState[appState.mode]
var MODE = null;
var activeTab=TAB_DEFS[0].id;

function setMode(m){
  appState.mode = m;
  if(!appState[m]) appState[m] = defaultState();
  state = appState[m];
  MODE = m;
}
/* ================= student accounts (saved on this device) ================= */
var SHEET_KEY='sopaan-ap1-ch5';
var ACC_KEY='sopaan-students-v1';
var storageOK=true;
var MEM={};
function lsGet(k){ var r=null; try{ r=localStorage.getItem(k); }catch(e){ storageOK=false; }
  if(r===null||r===undefined) r=MEM[k]||null; try{ return r?JSON.parse(r):null; }catch(e){ return null; } }
function lsSet(k,v){ var j=JSON.stringify(v); MEM[k]=j; try{ localStorage.setItem(k,j); return true; }catch(e){ storageOK=false; return false; } }
try{ localStorage.setItem('sopaan-probe','1'); localStorage.removeItem('sopaan-probe'); }catch(e){ storageOK=false; }
function hashPin(pin,salt){ var h=2166136261, str=salt+'|'+pin; for(var i=0;i<str.length;i++){ h^=str.charCodeAt(i); h=Math.imul(h,16777619)>>>0; } return h.toString(36); }
function accKey(name,roll){ return (name.trim().toLowerCase().replace(/\s+/g,' ')+'#'+roll.trim().toLowerCase()); }
function accounts(){ return lsGet(ACC_KEY)||{students:{}}; }
var student=null; // {key,name,roll}
var records=[];   // finished-tab history for this student on this sheet
function progKey(){ return SHEET_KEY+'::'+student.key; }

function loadState(){
  appState = {mode:null, learning:null, quiz:null}; records=[]; activeTab=TAB_DEFS[0].id;
  var saved=lsGet(progKey());
  if(!saved) return;
  try{
    ['learning','quiz'].forEach(function(m){
      if(saved[m] && saved[m].tabs){
        var fresh = defaultState();
        TAB_DEFS.forEach(function(td){
          if(saved[m].tabs[td.id] && saved[m].tabs[td.id].items && saved[m].tabs[td.id].items.length===SLIDES[td.id].length){
            fresh.tabs[td.id] = saved[m].tabs[td.id];
          }
        });
        appState[m] = fresh;
      }
    });
    if(saved.mode==='learning' || saved.mode==='quiz') appState.mode = saved.mode;
    if(saved.activeTab && TAB_DEFS.some(function(t){ return t.id===saved.activeTab; })) activeTab = saved.activeTab;
    if(Array.isArray(saved.records)) records = saved.records;
  }catch(e){}
}
function saveState(){
  if(!student) return;
  lsSet(progKey(), {mode:appState.mode, learning:appState.learning, quiz:appState.quiz, activeTab:activeTab, records:records, updated:Date.now()});
  var acc=accounts(); if(acc.students[student.key]){ acc.students[student.key].last=Date.now(); lsSet(ACC_KEY,acc); }
}
function addRecord(mode, tabId, correct, revealed, skipped, total){
  records.unshift({at:Date.now(), mode:mode, tab:tabId, correct:correct, revealed:revealed, skipped:skipped, total:total});
  if(records.length>60) records.length=60;
  saveState();
}
function fmtDate(ms){ try{ return new Date(ms).toLocaleString(undefined,{day:'numeric',month:'short',hour:'2-digit',minute:'2-digit'}); }catch(e){ return ''; } }

function renderWho(){
  var el=document.getElementById('whoBar');
  if(!student){ el.hidden=true; el.innerHTML=''; return; }
  el.hidden=false;
  el.innerHTML='Signed in as <b>'+esc(student.name)+'</b> · Roll '+esc(student.roll)+
    ' <button id="btnRecord">My record</button> <button id="btnSignOut">Sign out</button>';
  document.getElementById('btnRecord').addEventListener('click', renderRecord);
  document.getElementById('btnSignOut').addEventListener('click', signOut);
}
function signOut(){
  saveState(); student=null; state=null; MODE=null;
  var acc=accounts(); delete acc.current; lsSet(ACC_KEY,acc);
  document.getElementById('modePill').hidden=true;
  renderWho(); renderLogin();
}
function signIn(key){
  var acc=accounts(); var st=acc.students[key]; if(!st) return;
  student={key:key, name:st.name, roll:st.roll};
  acc.current=key; st.last=Date.now(); lsSet(ACC_KEY,acc);
  loadState(); renderWho();
  if(appState.mode==='learning' || appState.mode==='quiz'){
    setMode(appState.mode); document.getElementById('modePill').hidden=false; updateModePill();
    showChrome(true); buildTabbar(); renderSlide();
    showToast('Welcome back, '+st.name.split(' ')[0]+'. Picking up where you left off.');
  } else {
    renderModePicker();
  }
}
function renderLogin(msg){
  showChrome(false);
  var acc=accounts(); var keys=Object.keys(acc.students).sort(function(a,b){ return (acc.students[b].last||0)-(acc.students[a].last||0); });
  var h='<div class="login-card"><h2>Student sign-in</h2>'+
    '<p class="lead">Sign in to save your answers, see your record and continue where you left off next time.</p>';
  if(!storageOK) h+='<div class="login-err">This browser is blocking saved data (for example, a private window). You can still practise, but progress will not be kept after you close the page.</div>';
  if(msg) h+='<div class="login-err">'+esc(msg)+'</div>';
  if(keys.length){
    h+='<div class="known"><span class="known-label">Continue as</span>';
    keys.slice(0,6).forEach(function(k){ var st=acc.students[k];
      h+='<button class="known-btn" data-k="'+esc(k)+'"><span>'+esc(st.name)+' <small>· Roll '+esc(st.roll)+'</small></span><small>'+(st.last?'Last active '+fmtDate(st.last):'')+'</small></button>'; });
    h+='</div>';
  }
  h+='<form id="loginForm" novalidate>'+
    '<div class="fld"><label for="lgName">Full name</label><input id="lgName" autocomplete="name" required></div>'+
    '<div class="fld-row"><div class="fld"><label for="lgRoll">Roll no.</label><input id="lgRoll" required></div>'+
    '<div class="fld"><label for="lgPin">4-digit PIN</label><input id="lgPin" type="password" inputmode="numeric" maxlength="4" autocomplete="off" required></div></div>'+
    '<button class="btn btn-primary" type="submit" style="width:100%;">Sign in</button>'+
    '<p class="login-note">New here? Enter your name, roll no. and a PIN you will remember, and your profile is created. Progress is saved on this device, so use the same device and browser to continue.</p>'+
    '</form></div>';
  var wrap=document.getElementById('wrap'); wrap.innerHTML=h;
  wrap.querySelectorAll('.known-btn').forEach(function(b){
    b.addEventListener('click', function(){
      var st=accounts().students[b.dataset.k];
      document.getElementById('lgName').value=st.name; document.getElementById('lgRoll').value=st.roll;
      document.getElementById('lgPin').focus();
    });
  });
  document.getElementById('loginForm').addEventListener('submit', function(e){
    e.preventDefault();
    var name=document.getElementById('lgName').value.trim(), roll=document.getElementById('lgRoll').value.trim(), pin=document.getElementById('lgPin').value.trim();
    if(!name||!roll){ renderLoginErr('Enter your name and roll no.'); return; }
    if(!/^\d{4}$/.test(pin)){ renderLoginErr('Your PIN must be exactly 4 digits.'); return; }
    var k=accKey(name,roll); var acc=accounts();
    if(acc.students[k]){
      if(acc.students[k].pin!==hashPin(pin,k)){ renderLoginErr('That PIN does not match this name and roll no. Try again.'); return; }
    } else {
      acc.students[k]={name:name.replace(/\s+/g,' '), roll:roll, pin:hashPin(pin,k), created:Date.now()};
      lsSet(ACC_KEY,acc);
    }
    signIn(k);
  });
}
function renderLoginErr(m){
  var n=document.getElementById('lgName').value, r=document.getElementById('lgRoll').value;
  renderLogin(m); document.getElementById('lgName').value=n; document.getElementById('lgRoll').value=r; document.getElementById('lgPin').focus();
}
function tabSummary(m){
  var st=appState[m]; if(!st) return null;
  return TAB_DEFS.map(function(td){
    var items=st.tabs[td.id].items, n=items.length;
    var c=items.filter(function(i){return i.status==='correct';}).length;
    var done=items.filter(function(i){return i.status!=='unanswered';}).length;
    return {label:td.label, n:n, done:done, correct:c};
  });
}
function renderRecord(){
  showChrome(false);
  var tabLabel=function(id){ var t=TAB_DEFS.find(function(x){return x.id===id;}); return t?t.label:id; };
  var h='<div class="rec-card"><h2>'+esc(student.name)+'’s record</h2><p class="rec-empty" style="margin:0 0 12px;">Roll '+esc(student.roll)+' · Torque and Rotational Dynamics</p>';
  ['learning','quiz'].forEach(function(m){
    var sum=tabSummary(m);
    h+='<h3>'+(m==='learning'?'📘 Learning Sheet':'📝 Quiz Mode')+'</h3>';
    if(!sum){ h+='<p class="rec-empty">Not started yet.</p>'; return; }
    h+='<div class="rec-table-wrap"><table class="rec-table"><thead><tr><th>Section</th><th>Progress</th><th>Correct first time</th></tr></thead><tbody>';
    sum.forEach(function(r){ var pct=Math.round(r.done/r.n*100);
      h+='<tr><td>'+r.label+'</td><td><span class="rec-bar"><i style="width:'+pct+'%"></i></span>'+r.done+' / '+r.n+'</td><td>'+(m==='quiz'?'—':r.correct+' / '+r.n)+'</td></tr>'; });
    h+='</tbody></table></div>';
  });
  h+='</div><div class="rec-card"><h3>Finished sections</h3>';
  if(!records.length){ h+='<p class="rec-empty">No sections finished yet. Each time you finish a tab, your score is recorded here.</p>'; }
  else {
    h+='<div class="rec-table-wrap"><table class="rec-table"><thead><tr><th>When</th><th>Mode</th><th>Section</th><th>Score</th></tr></thead><tbody>';
    records.forEach(function(r){ h+='<tr><td>'+fmtDate(r.at)+'</td><td>'+(r.mode==='quiz'?'Quiz':'Learning')+'</td><td>'+tabLabel(r.tab)+'</td><td>'+r.correct+' / '+r.total+(r.mode==='learning'?' <span class="rec-empty">('+r.revealed+' revealed, '+r.skipped+' skipped)</span>':'')+'</td></tr>'; });
    h+='</tbody></table></div>';
  }
  h+='</div><button class="btn btn-primary" id="btnBack">'+(MODE?'Back to practice':'Choose a mode')+'</button>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('btnBack').addEventListener('click', function(){
    if(MODE){ showChrome(true); buildTabbar(); renderSlide(); } else renderModePicker();
  });
  window.scrollTo({top:0});
}

/* ================= tab bar ================= */
function buildTabbar(){
  var el=document.getElementById('tabbar');
  el.innerHTML = '<button class="tab-btn'+(activeTab==='theory'?' active':'')+'" data-tab="theory">Theory<span class="tab-count">notes</span></button>' + TAB_DEFS.map(function(t){
    return '<button class="tab-btn'+(t.id===activeTab?' active':'')+'" data-tab="'+t.id+'">'+t.label+'<span class="tab-count">'+SLIDES[t.id].length+'</span></button>';
  }).join('') + '<button class="tab-btn'+(activeTab==='report'?' active':'')+'" data-tab="report">📊 Report<span class="tab-count">pie</span></button>';
  el.querySelectorAll('.tab-btn').forEach(function(btn){
    btn.addEventListener('click', function(){ activeTab=btn.dataset.tab; buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); });
  });
}

/* ================= rendering ================= */
function canProceed(item){ return item.status==='correct'||item.status==='revealed'||item.status==='skipped'; }

function renderSlide(){
  if(!state.tabs[activeTab]){ activeTab=TAB_DEFS[0].id; }
  var ts = state.tabs[activeTab];
  var slides = SLIDES[activeTab];
  if(ts.idx>=slides.length||ts.idx<0) ts.idx=0;
  var idx = ts.idx;
  var slide = slides[idx];
  var item = ts.items[idx];

  var tdef=TAB_DEFS.find(function(t){return t.id===activeTab;});
  document.getElementById('spLabel').textContent = tdef.label+' · Question '+(idx+1)+' of '+slides.length;
  document.getElementById('secSub').textContent = tdef.label+' — '+tdef.sub;
  document.getElementById('spFill').style.width = Math.round(((idx)/(Math.max(slides.length-1,1)))*100)+'%';

  var h='<div class="qcard">';
  if(slide.kind==='mcq' || slide.kind==='ar'){
    h += renderChoiceBody(slide, item, idx);
  } else {
    h += renderBlankBody(slide, item, idx);
  }
  h += '<div class="feedback" id="feedbackBox"></div>';
  if(MODE==='learning' && (item.status==='correct'||item.status==='revealed')) h += solutionHTML(slide);
  h += '</div>';

  document.getElementById('wrap').innerHTML = h;
  wireSlideEvents(slide, item, idx);
  restoreFeedback(item);
  renderNavbar(item);
  updateFab();
  saveState();
}

function solutionHTML(slide){
  if(!slide.sol) return '';
  return '<div class="solution"><div class="sol-h">Solution</div>'+slide.sol.split('\n').map(function(l){ return '<div class="sol-line">'+fr(esc(l))+'</div>'; }).join('')+'</div>';
}
var LETTERS=['a','b','c','d','e','f'];
function chosenHTML(slide,item){
  var opts = slide.kind==='mcq' ? slide.opts : AR_OPTIONS;
  if(item.choice===undefined) return '';
  var ok=item.choice===slide.correct;
  var h='<div class="chosen '+(ok?'ok':'bad')+'"><b>Your answer:</b> ('+LETTERS[item.choice]+') '+fr(esc(opts[item.choice]))+(ok?' ✓':' ✗')+'</div>';
  if(!ok) h+='<div class="chosen ok"><b>Correct answer:</b> ('+LETTERS[slide.correct]+') '+fr(esc(opts[slide.correct]))+'</div>';
  return h;
}
function renderChoiceBody(slide, item, idx){
  var h='<div class="qhead"><div class="qnum">'+(idx+1)+'</div>';
  if(slide.kind==='mcq'){
    h += '<div class="qtext">'+fr(esc(slide.text))+(slide.tag?'<span class="qtag">'+esc(slide.tag)+'</span>':'')+'</div>';
  } else {
    h += '<div class="qtext"><span class="qtag" style="margin-left:0;margin-right:6px;">A / R</span>Assertion (A): '+fr(esc(slide.a))+'<br>Reason (R): '+fr(esc(slide.r))+(slide.tag?'<span class="qtag">'+esc(slide.tag)+'</span>':'')+'</div>';
  }
  h += '</div>';
  if(slide.fig) h += '<div class="fig">'+slide.fig+'</div>';
  var opts = slide.kind==='mcq' ? slide.opts : AR_OPTIONS;
  if(MODE==='quiz') h += '<div class="quiz-hint">Quiz mode — pick an option. The correct answer is revealed once you finish this tab.</div>';
  var locked = MODE==='learning' && (item.status==='correct' || item.status==='revealed');
  h += '<div class="options">';
  opts.forEach(function(opt,i){
    var cls='opt';
    if(locked){
      if(i===slide.correct) cls+=' is-correct';
      else if(item.choice===i) cls+=' is-wrong';
      cls+=' locked';
    }
    if(item.choice===i) cls+=' is-chosen';
    h += '<label class="'+cls+'"><input type="radio" name="choice" value="'+i+'" '+(item.choice===i?'checked':'')+' '+(locked?'disabled':'')+'> <span class="opt-l">('+LETTERS[i]+')</span> <span class="opt-t">'+fr(esc(opt))+'</span>'+(item.choice===i?'<span class="opt-tag">'+(locked?(i===slide.correct?'Your answer ✓':'Your answer ✗'):'Selected')+'</span>':'')+'</label>';
  });
  h += '</div>';
  if(locked) h += chosenHTML(slide,item);
  return h;
}

function renderBlankBody(slide, item, idx){
  var h='<div class="qhead"><div class="qnum">'+(idx+1)+'</div><div class="qtext">';
  if(slide.kind==='case'){ h += '<b>'+esc(slide.title)+'.</b> '+esc(slide.body); }
  else { var pp=fr(esc(slide.p)).split('\n'); h += pp[0]+pp.slice(1).map(function(x){return '<span class="datline">'+x+'</span>';}).join(''); }
  h += (slide.tag?'<span class="qtag">'+esc(slide.tag)+'</span>':'')+'</div>';
  if(slide.marks) h += '<div class="marks-pill">'+slide.marks+'</div>';
  h += '</div>';
  if(slide.fig) h += '<div class="fig">'+slide.fig+'</div>';

  var total = slide.flat.length;
  var hasExpr = slide.flat.some(function(s){ return s.expr===true; });
  var hasFrac = slide.flat.some(function(s){ return ['fv','fe','fl','fm','fi','flist'].indexOf(s.expr)>=0; });
  var blankCount=0; slide.flat.forEach(function(s){ blankCount+=Object.keys(s.a).length; });

  if(MODE==='quiz'){
    h += '<div class="step-badge">'+blankCount+' blank'+(blankCount===1?'':'s')+' in this question</div>';
    h += '<div class="quiz-hint">Quiz mode — fill in what you can. Correct answers are revealed once you finish this tab.</div>';
    if(hasFrac) h += '<div class="quiz-hint">Type fractions like <b>3/4</b> and mixed numbers like <b>2 3/4</b> (whole number, space, fraction).</div>';
    if(hasExpr) h += '<div class="quiz-hint">Type answers like <b>2x-5</b>, <b>x^2-4</b> or <b>3+2i</b>. Use ^ for powers, | | for absolute value and pi for π. Any equivalent form is accepted.</div>';
    h += '<div class="steps">';
    for(var qi=0; qi<total; qi++){
      var qstep = slide.flat[qi];
      var qss = item.stepStates[qi];
      if(qstep.partLabel) h += '<div class="subpart-label">'+esc(qstep.partLabel)+'</div>';
      if(qstep.orDivider) h += '<div class="or-divider">OR</div>';
      var qline = fr(qstep.t);
      Object.keys(qstep.a).forEach(function(bkey){
        var val = qss.inputs[bkey]||'';
        var inp='<input class="blank-input'+(['fv','fe','fl','fm','fi','dec'].indexOf(qstep.expr)>=0?' fr':(qstep.expr==='flist'||qstep.expr==='dlist'||qstep.expr==='glist')?' wide':qstep.expr===true?' expr':(qstep.expr==='words'||qstep.expr==='list'||qstep.expr==='expanded'||qstep.expr==='set'||qstep.expr==='primes'||qstep.expr==='trans'||qstep.expr==='rot'?' wide':''))+'" data-step="'+qi+'" data-bkey="'+bkey+'" value="'+esc(val)+'">';
        qline = qline.replace('__'+bkey+'__', inp);
      });
      if(qstep.fig) h += '<div class="fig">'+qstep.fig+'</div>';
      h += '<div class="step-line">'+qline+'</div>';
    }
    h += '</div>';
    return h;
  }

  if(hasFrac) h += '<div class="quiz-hint">Type fractions like <b>3/4</b> and mixed numbers like <b>2 3/4</b> (whole number, space, fraction).</div>';
  if(hasExpr) h += '<div class="quiz-hint">Type answers like <b>2x-5</b>, <b>x^2-4</b> or <b>3+2i</b>. Use ^ for powers, | | for absolute value and pi for π. Any equivalent form is accepted.</div>';
  var locked = item.status==='correct' || item.status==='revealed';
  var upto = locked ? total-1 : item.curStep;
  h += '<div class="step-badge">Step '+(Math.min(item.curStep,total-1)+1)+' of '+total+'</div>';
  h += '<div class="steps">';
  for(var i=0;i<=upto;i++){
    var step = slide.flat[i];
    var ss = item.stepStates[i];
    if(step.partLabel) h += '<div class="subpart-label">'+esc(step.partLabel)+'</div>';
    if(step.orDivider) h += '<div class="or-divider">OR</div>';
    var resolved = ss.status==='correct' || ss.status==='revealed';
    var line = fr(step.t);
    Object.keys(step.a).forEach(function(bkey){
      var fs = ss.inputs['fs_'+bkey];
      var cls = fs==='correct'?'is-correct':(fs==='wrong'?'is-wrong':'');
      var val = ss.inputs[bkey]||'';
      var dis = resolved ? 'disabled':'';
      var inp='<input class="blank-input '+cls+(['fv','fe','fl','fm','fi','dec'].indexOf(step.expr)>=0?' fr':(step.expr==='flist'||step.expr==='dlist'||step.expr==='glist')?' wide':step.expr===true?' expr':(step.expr==='words'||step.expr==='list'||step.expr==='expanded'||step.expr==='set'||step.expr==='primes'||step.expr==='trans'||step.expr==='rot'?' wide':''))+'" data-step="'+i+'" data-bkey="'+bkey+'" value="'+esc(val)+'" '+dis+'>';
      line = line.replace('__'+bkey+'__', inp);
    });
    if(step.fig) h += '<div class="fig">'+step.fig+'</div>';
    h += '<div class="step-line'+(resolved?' resolved':'')+'">'+line+'</div>';
    if(ss.status==='revealed'){
      var reveals=[];
      Object.keys(step.a).forEach(function(bkey){ reveals.push(bkey+' = '+esc(step.a[bkey])); });
      h += '<div class="reveal-note">correct: '+reveals.join(', ')+'</div>';
    }
  }
  h += '</div>';
  return h;
}

var __flash=null;
function restoreFeedback(item){
  var fb=document.getElementById('feedbackBox'); if(!fb) return;
  if(__flash){ fb.className='feedback show '+__flash.c; fb.innerHTML=__flash.h; __flash=null; return; }
  if(MODE==='quiz'){ fb.className='feedback'; fb.innerHTML=''; return; }
  if(item.status==='correct'){ fb.className='feedback show ok'; fb.innerHTML='<b>All done — correct!</b>'; }
  else if(item.status==='revealed'){ fb.className='feedback show reveal'; fb.innerHTML='<b>Answer revealed above.</b> Move on whenever you’re ready.'; }
  else if(item.status==='skipped'){ fb.className='feedback show retry'; fb.innerHTML='Skipped — you can answer it now and press <b>Check</b>, or come back later from the Question Palette.'; }
  else { fb.className='feedback'; fb.innerHTML=''; }
}

function slideIsAnswered(slide,item){
  if(slide.kind==='mcq'||slide.kind==='ar') return item.choice!==undefined;
  return item.stepStates.some(function(ss){ return Object.keys(ss.inputs).some(function(k){ return k.indexOf('fs_')!==0 && ss.inputs[k] && String(ss.inputs[k]).trim()!==''; }); });
}
function renderNavbar(item){
  var ts = state.tabs[activeTab];
  var slides = SLIDES[activeTab];
  var slide = slides[ts.idx];
  var isLast = ts.idx === slides.length-1;
  var h='';
  h += '<button class="btn" id="btnPrev" '+(ts.idx===0?'disabled':'')+'>&larr; Previous</button>';

  if(MODE==='quiz'){
    h += '<button class="btn" id="btnSkip">Skip</button>';
    h += '<span class="spacer"></span>';
    h += '<button class="btn btn-primary" id="btnNext">'+(isLast?'Finish':'Next →')+'</button>';
  } else {
    h += '<button class="btn" id="btnSkip" '+(item.status!=='unanswered'?'disabled':'')+'>Skip</button>';
    h += '<span class="spacer"></span>';
    h += '<button class="btn btn-primary" id="btnCheck" '+((item.status!=='unanswered'&&item.status!=='skipped')?'disabled':'')+'>Check</button>';
    h += '<button class="btn btn-primary" id="btnNext" '+(canProceed(item)?'':'disabled')+'>'+(isLast?'Finish':'Next →')+'</button>';
  }
  document.getElementById('navbarInner').innerHTML = h;

  document.getElementById('btnPrev').addEventListener('click', function(){ if(ts.idx>0){ ts.idx--; renderSlide(); } });

  if(MODE==='quiz'){
    document.getElementById('btnSkip').addEventListener('click', function(){
      item.status='skipped';
      advanceQuiz(ts, slides);
    });
    document.getElementById('btnNext').addEventListener('click', function(){
      item.status = slideIsAnswered(slide,item) ? 'answered' : 'unanswered';
      advanceQuiz(ts, slides);
    });
  } else {
    document.getElementById('btnSkip').addEventListener('click', function(){
      if(item.status==='unanswered'){ item.status='skipped'; renderSlide(); }
    });
    document.getElementById('btnNext').addEventListener('click', function(){
      if(!canProceed(item)) return;
      if(ts.idx < slides.length-1){ ts.idx++; ts.maxReached=Math.max(ts.maxReached, ts.idx); renderSlide(); }
      else { renderFinished(); }
    });
    document.getElementById('btnCheck').addEventListener('click', function(){ handleCheck(); });
  }
}
function advanceQuiz(ts, slides){
  if(ts.idx < slides.length-1){ ts.idx++; ts.maxReached=Math.max(ts.maxReached, ts.idx); renderSlide(); }
  else { renderFinished(); }
}

function wireSlideEvents(slide, item, idx){
  document.querySelectorAll('input[type="radio"][name="choice"]').forEach(function(r){
    r.addEventListener('change', function(){ item.choice=parseInt(r.value,10); saveState();
      document.querySelectorAll('.opt').forEach(function(l){ l.classList.remove('is-chosen'); var t=l.querySelector('.opt-tag'); if(t) t.remove(); });
      var lab=r.closest('.opt'); lab.classList.add('is-chosen'); var tg=document.createElement('span'); tg.className='opt-tag'; tg.textContent='Selected'; lab.appendChild(tg); });
  });
  document.querySelectorAll('.blank-input').forEach(function(inp){
    inp.addEventListener('input', function(){
      var si=parseInt(inp.dataset.step,10), bk=inp.dataset.bkey;
      item.stepStates[si].inputs[bk]=inp.value;
      saveState();
    });
  });
}

function handleCheck(){
  var ts = state.tabs[activeTab];
  var slide = SLIDES[activeTab][ts.idx];
  var item = ts.items[ts.idx];
  if(item.status==='skipped') item.status='unanswered';
  var fb = document.getElementById('feedbackBox');

  if(slide.kind==='mcq' || slide.kind==='ar'){
    if(item.choice===undefined){ fb.className='feedback show err'; fb.innerHTML='Please choose an option first.'; return; }
    var ok = item.choice===slide.correct;
    if(ok){
      item.status='correct'; playSuccess();
      fb.className='feedback show ok'; fb.innerHTML='<b>Correct!</b>';
    } else {
      item.attempts=(item.attempts||0)+1;
      if(item.attempts>=2){ item.status='revealed'; playReveal(); }
      else { playWrong(); fb.className='feedback show retry'; fb.innerHTML='<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'; __flash={c:'retry',h:'<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'}; }
    }
    renderSlide();
    return;
  }

  // blank / case
  var step = slide.flat[item.curStep];
  var ss = item.stepStates[item.curStep];
  var blanks = Object.keys(step.a);
  var missing = blanks.some(function(k){ return !ss.inputs[k] || String(ss.inputs[k]).trim()===''; });
  if(missing){ fb.className='feedback show err'; fb.innerHTML='Fill in every blank in this step before checking.'; return; }

  var allGood=true;
  blanks.forEach(function(k){
    var good=answerMatches(ss.inputs[k], step.a[k], step.accept, step.expr);
    ss.inputs['fs_'+k]=good?'correct':'wrong';
    if(!good) allGood=false;
  });

  if(allGood){
    ss.status='correct'; playSuccess();
    advanceStepOrFinish(item, slide);
  } else {
    ss.attempts=(ss.attempts||0)+1;
    if(ss.attempts>=2){
      ss.status='revealed'; playReveal();
      advanceStepOrFinish(item, slide);
    } else {
      playWrong();
      fb.className='feedback show retry';
      fb.innerHTML='<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'; __flash={c:'retry',h:'<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'};
      renderSlide();
      return;
    }
  }
  renderSlide();
}

function advanceStepOrFinish(item, slide){
  var total = slide.flat.length;
  var hasExpr = slide.flat.some(function(s){ return s.expr===true; });
  var hasFrac = slide.flat.some(function(s){ return ['fv','fe','fl','fm','fi','flist'].indexOf(s.expr)>=0; });
  if(item.curStep < total-1){
    item.curStep++;
  } else {
    var anyRevealed = item.stepStates.some(function(ss){ return ss.status==='revealed'; });
    item.status = anyRevealed ? 'revealed' : 'correct';
  }
}

/* ================= palette ================= */
var overlay=document.getElementById('overlay');
function statusOf(tabId,i){
  var ts=state.tabs[tabId];
  if(i>ts.maxReached) return 'locked';
  if(i===ts.idx) return 'current';
  return ts.items[i].status==='unanswered' ? 'locked' : ts.items[i].status;
}
function buildPalette(){
  var slides=SLIDES[activeTab];
  document.getElementById('paletteTitle').textContent = TAB_DEFS.find(function(t){return t.id===activeTab;}).label+' · Question palette';
  var body=document.getElementById('paletteBody');
  var html='';
  for(var i=0;i<slides.length;i++){
    html += '<div class="chip" data-status="'+statusOf(activeTab,i)+'" data-idx="'+i+'">'+(i+1)+'</div>';
  }
  body.innerHTML = html;
  body.querySelectorAll('.chip').forEach(function(chip){
    chip.addEventListener('click', function(){
      var i=parseInt(chip.dataset.idx,10);
      var ts=state.tabs[activeTab];
      if(i>ts.maxReached){ showToast('Finish the earlier questions to unlock this one.'); return; }
      ts.idx=i; closePalette(); renderSlide();
    });
  });
}
function openPalette(){ buildPalette(); overlay.classList.add('show'); }
function closePalette(){ overlay.classList.remove('show'); }
var toastTimer;
function showToast(msg){
  var t=document.getElementById('toast'); t.textContent=msg; t.classList.add('show');
  clearTimeout(toastTimer); toastTimer=setTimeout(function(){ t.classList.remove('show'); },2200);
}
document.getElementById('paletteFab').addEventListener('click', openPalette);
document.getElementById('paletteClose').addEventListener('click', closePalette);
overlay.addEventListener('click', function(e){ if(e.target===overlay) closePalette(); });

function updateFab(){
  var slides=SLIDES[activeTab];
  var done=state.tabs[activeTab].items.filter(function(it){ return it.status==='correct'||it.status==='revealed'||it.status==='skipped'; }).length;
  document.getElementById('fabBadge').textContent = done+'/'+slides.length;
}

/* ================= overall progress + finish ================= */
function updateChapterProgress(){
  var total=0, done=0;
  TAB_DEFS.forEach(function(td){
    state.tabs[td.id].items.forEach(function(it){
      total++;
      if(it.status==='correct'||it.status==='revealed'||it.status==='skipped') done++;
    });
  });
  var pct = total>0 ? Math.round((done/total)*100) : 0;
  document.getElementById('cpFill').style.width=pct+'%';
  document.getElementById('cpLabel').textContent=pct+'% complete';
}
function isSlideCorrect(tabId,i){
  var slide=SLIDES[tabId][i], item=state.tabs[tabId].items[i];
  if(slide.kind==='mcq'||slide.kind==='ar') return item.choice===slide.correct;
  return slide.flat.every(function(step,si){
    var ss=item.stepStates[si];
    return Object.keys(step.a).every(function(k){ return answerMatches(ss.inputs[k], step.a[k], step.accept, step.expr); });
  });
}
function isSlideAttempted(tabId,i){
  var slide=SLIDES[tabId][i], item=state.tabs[tabId].items[i];
  if(slide.kind==='mcq'||slide.kind==='ar') return item.choice!==undefined;
  return slide.flat.some(function(step,si){
    var ss=item.stepStates[si];
    return Object.keys(step.a).some(function(k){ return ss.inputs[k] && String(ss.inputs[k]).trim()!==''; });
  });
}
function slideLabel(slide){
  var t = slide.kind==='case' ? slide.title : (slide.kind==='ar' ? slide.a : (slide.p||slide.text||''));
  t = String(t);
  return t.length>90 ? t.slice(0,90)+'…' : t;
}
function slideAnswerText(slide, item, correctSide){
  if(slide.kind==='mcq'||slide.kind==='ar'){
    var opts = slide.kind==='mcq' ? slide.opts : AR_OPTIONS;
    if(correctSide) return '('+LETTERS[slide.correct]+') '+opts[slide.correct];
    return item.choice!==undefined ? '('+LETTERS[item.choice]+') '+opts[item.choice] : '— not attempted —';
  }
  return slide.flat.map(function(step,si){
    var ss=item.stepStates[si];
    return Object.keys(step.a).map(function(k){
      return correctSide ? step.a[k] : (ss.inputs[k] || '—');
    }).join(', ');
  }).join('  |  ');
}

function renderFinished(){
  if(MODE==='quiz'){ renderQuizResults(); return; }
  var ts=state.tabs[activeTab];
  var correct=ts.items.filter(function(i){return i.status==='correct';}).length;
  var revealed=ts.items.filter(function(i){return i.status==='revealed';}).length;
  var skipped=ts.items.filter(function(i){return i.status==='skipped';}).length;
  var h='<div class="qcard done-card"><div class="big">✨</div><h2>Tab complete</h2>';
  h+='<p style="color:var(--ink-soft);font-size:14px;">You worked through all '+ts.items.length+' questions in this tab.</p>';
  if(!ts.recorded){ ts.recorded=true; addRecord('learning', activeTab, correct, revealed, skipped, ts.items.length); }
  h+='<div class="score-pills">'+
     '<span class="score-pill" style="background:var(--success-soft);color:var(--success);">'+correct+' correct</span>'+
     '<span class="score-pill" style="background:var(--danger-soft);color:var(--danger);">'+revealed+' revealed</span>'+
     '<span class="score-pill" style="background:var(--gold-soft);color:var(--retry-text);">'+skipped+' skipped</span></div>'+
     '<button class="btn btn-primary" id="btnReview">Review from question 1</button></div>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('navbarInner').innerHTML='';
  document.getElementById('btnReview').addEventListener('click', function(){ ts.idx=0; ts.recorded=false; renderSlide(); });
  updateChapterProgress(); saveState();
}

function renderQuizResults(){
  var ts=state.tabs[activeTab];
  var slides=SLIDES[activeTab];
  var correctCount=0, attemptedCount=0;
  var rowsHtml = slides.map(function(slide,i){
    var item=ts.items[i];
    var ok=isSlideCorrect(activeTab,i);
    var att=isSlideAttempted(activeTab,i);
    if(ok) correctCount++;
    if(att) attemptedCount++;
    var tagCls = ok?'ok':(att?'bad':'na');
    var tagText = ok?'Correct':(att?'Incorrect':'Not attempted');
    var row='<div class="review-row">';
    row+='<span class="review-tag '+tagCls+'">Q'+(i+1)+' · '+tagText+'</span>';
    row+='<div class="review-q">'+fr(esc(slideLabel(slide)))+'</div>';
    row+='<div class="review-ans"><b>Your answer:</b> '+fr(esc(slideAnswerText(slide,item,false)))+'</div>';
    if(!ok) row+='<div class="review-ans" style="color:var(--success);"><b>Correct answer:</b> '+fr(esc(slideAnswerText(slide,item,true)))+'</div>';
    if(slide.sol) row+='<details class="rev-sol"><summary>Show solution</summary>'+solutionHTML(slide)+'</details>';
    row+='</div>';
    return row;
  }).join('');

  addRecord('quiz', activeTab, correctCount, 0, 0, slides.length);
  var h='<div class="qcard done-card" style="text-align:left;">';
  h+='<div style="text-align:center;"><div class="big">🏁</div><h2>Quiz complete</h2>';
  h+='<p style="color:var(--ink-soft);font-size:14px;">'+correctCount+' / '+slides.length+' correct · '+attemptedCount+' attempted</p></div>';
  h+='<div style="margin-top:16px;">'+rowsHtml+'</div>';
  h+='<div style="text-align:center;margin-top:6px;"><button class="btn btn-primary" id="btnReview">Review from question 1</button></div>';
  h+='</div>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('navbarInner').innerHTML='';
  document.getElementById('btnReview').addEventListener('click', function(){ ts.idx=0; renderSlide(); });
  playSuccess();
  updateChapterProgress(); saveState();
}

/* ================= mode picker ================= */
function updateModePill(){
  var btn=document.getElementById('modePill');
  if(!btn) return;
  btn.textContent = MODE==='quiz' ? '📝 Quiz Mode · switch' : '📘 Learning Sheet · switch';
}
function showChrome(show){
  document.querySelector('.tabbar').style.display = show?'':'none';
  document.querySelector('.slide-progress').style.display = show?'':'none';
  document.querySelector('.navbar').style.display = show?'':'none';
  document.getElementById('paletteFab').style.display = show?'':'none';
}
function renderModePicker(){
  showChrome(false);
  var wrap=document.getElementById('wrap');
  wrap.innerHTML =
    '<div class="mode-pick">'+
      '<div class="mode-card" data-pick="learning"><div class="mode-icon">📘</div><h3>Learning Sheet</h3>'+
      '<p>Work through each question step by step with instant feedback — two tries, then the correct value is revealed right there so you can keep moving.</p></div>'+
      '<div class="mode-card" data-pick="quiz"><div class="mode-icon">📝</div><h3>Quiz Mode</h3>'+
      '<p>Attempt every question with no hints along the way. Your score and the full answer key are shown together once you finish the tab — just like the real exam.</p></div>'+
    '</div>';
  wrap.querySelectorAll('[data-pick]').forEach(function(card){
    card.addEventListener('click', function(){
      setMode(card.dataset.pick); activeTab='theory';
      document.getElementById('modePill').hidden=false;
      updateModePill();
      showChrome(true);
      buildTabbar();
      renderSlide();
    });
  });
}
document.getElementById('modePill').addEventListener('click', function(){
  if(!appState.mode){ return; }
  setMode(appState.mode==='quiz' ? 'learning' : 'quiz');
  updateModePill();
  showChrome(true);
  buildTabbar();
  renderSlide();
});

/* ================= boot ================= */
var _origRenderSlide = renderSlide;
renderSlide = function(){ _origRenderSlide(); updateChapterProgress(); updateModePill(); };


/* ================= theory tab ================= */
function renderTheory(){
  var wrap=document.getElementById('wrap');
  wrap.innerHTML='<div class="theory">'+fr(THEORY)+'</div>';
  document.querySelector('.slide-progress').style.display='none';
  document.querySelector('.navbar').style.display='none';
  document.getElementById('paletteFab').style.display='none';
  document.getElementById('secSub').textContent='Theory — notes, key ideas and worked examples';
  wrap.querySelectorAll('[data-go]').forEach(function(b){ b.addEventListener('click',function(){ activeTab=b.dataset.go; buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); }); });
  wrap.querySelectorAll('[data-jump]').forEach(function(b){ b.addEventListener('click',function(){ var t=document.getElementById(b.dataset.jump); if(t) t.scrollIntoView({behavior:'smooth',block:'start'}); }); });
}
var _rsTheory = renderSlide;
renderSlide = function(){
  if(activeTab==='theory'){ renderTheory(); buildTabbar(); updateChapterProgress(); updateModePill(); saveState(); return; }
  document.querySelector('.slide-progress').style.display='';
  document.querySelector('.navbar').style.display='';
  document.getElementById('paletteFab').style.display='';
  _rsTheory();
};


/* ================= MYP criterion report ================= */
var CRIT_COL={A:'var(--accent-text)',B:'var(--gold)',C:'#8A6FC4',D:'#2F9AA0'};
function critStats(){
  return REPORT.map(function(r){
    var tabId=r[0], slides=SLIDES[tabId], ts=state&&state.tabs[tabId];
    var c=0,w=0,n=0;
    slides.forEach(function(sl,i){
      if(!ts||!ts.items[i]){ n++; return; }
      var it=ts.items[i];
      if(MODE==='learning'){
        if(it.status==='correct') c++; else if(it.status==='revealed') w++; else n++;
      } else {
        if(isSlideCorrect(tabId,i)) c++; else if(isSlideAttempted(tabId,i)) w++; else n++;
      }
    });
    var tot=slides.length, pct=tot?Math.round(c/tot*100):0;
    var lvl=Math.round(c/Math.max(tot,1)*8);
    return {tab:tabId,letter:r[1],name:r[2],c:c,w:w,n:n,tot:tot,pct:pct,lvl:lvl};
  });
}
function arcPath(cx,cy,r,a0,a1){
  if(a1-a0>=Math.PI*2-1e-6){ return 'M'+(cx-r)+','+cy+' a'+r+','+r+' 0 1,0 '+(2*r)+',0 a'+r+','+r+' 0 1,0 '+(-2*r)+',0 Z'; }
  var x0=cx+r*Math.sin(a0), y0=cy-r*Math.cos(a0), x1=cx+r*Math.sin(a1), y1=cy-r*Math.cos(a1);
  return 'M'+cx+','+cy+' L'+x0.toFixed(2)+','+y0.toFixed(2)+' A'+r+','+r+' 0 '+((a1-a0)>Math.PI?1:0)+',1 '+x1.toFixed(2)+','+y1.toFixed(2)+' Z';
}
function pieSVG(parts,size,hole,center,sub){
  var tot=parts.reduce(function(s,p){return s+p.v;},0), cx=size/2, cy=size/2, r=size/2-4, a=0, h='';
  if(!tot){ h+='<circle cx="'+cx+'" cy="'+cy+'" r="'+r+'" style="fill:var(--paper-2);stroke:var(--rule)"/>'; }
  parts.forEach(function(p){ if(!p.v) return; var b=a+p.v/tot*Math.PI*2; h+='<path d="'+arcPath(cx,cy,r,a,b)+'" style="fill:'+p.col+';stroke:var(--card);stroke-width:2"><title>'+esc(p.label)+': '+p.v+'</title></path>'; a=b; });
  if(hole) h+='<circle cx="'+cx+'" cy="'+cy+'" r="'+(r*hole)+'" style="fill:var(--card)"/>';
  if(center) h+='<text x="'+cx+'" y="'+(cy-(sub?4:0))+'" text-anchor="middle" dominant-baseline="middle" style="fill:var(--ink);font:700 '+(size>150?22:15)+'px Fraunces,serif">'+center+'</text>';
  if(sub) h+='<text x="'+cx+'" y="'+(cy+16)+'" text-anchor="middle" style="fill:var(--ink-soft);font:600 11px \'Source Sans 3\',sans-serif">'+sub+'</text>';
  return '<svg viewBox="0 0 '+size+' '+size+'" width="'+size+'" height="'+size+'" role="img">'+h+'</svg>';
}
function band(l){ return l===0?'0':(l<=2?'1–2':(l<=4?'3–4':(l<=6?'5–6':'7–8'))); }
function renderReport(){
  var st=critStats(), totC=0, totQ=0;
  st.forEach(function(s){ totC+=s.c; totQ+=s.tot; });
  var h='<div class="theory">';
  h+='<section class="note"><h2>Chapter test report</h2><p class="lt">'+(MODE==='quiz'?'<b>Quiz mode</b> results: answers are marked when you finish each test tab.':'<b>Learning mode</b> results: a question counts as correct only if you got it without the answer being revealed. Switch to Quiz mode for an exam-style score.')+'</p>';
  h+='<div class="rep-top"><div class="rep-pie">'+pieSVG(st.map(function(s){return {v:s.c,col:CRIT_COL[s.letter],label:'Criterion '+s.letter};}),200,0.55,totC+'/'+totQ,'correct')+'</div>';
  h+='<div class="rep-legend"><div class="rep-cap">Marks earned, by criterion</div>'+st.map(function(s){ return '<div class="rep-li"><span class="sw" style="background:'+CRIT_COL[s.letter]+'"></span><b>'+s.letter+'</b>&nbsp;'+esc(s.name)+'<span class="rep-n">'+s.c+' / '+s.tot+'</span></div>'; }).join('')+'</div></div></section>';
  h+='<div class="rep-grid">'+st.map(function(s){
    return '<div class="rep-card"><div class="rep-h"><span class="crit" style="background:'+CRIT_COL[s.letter]+'">'+s.letter+'</span>'+esc(s.name)+'</div>'+
      '<div class="rep-row">'+pieSVG([{v:s.c,col:'var(--success)',label:'Correct'},{v:s.w,col:'var(--danger)',label:'Incorrect'},{v:s.n,col:'var(--locked)',label:'Not attempted'}],120,0.5,s.pct+'%')+
      '<div class="rep-stats"><div><span class="sw" style="background:var(--success)"></span>Correct <b>'+s.c+'</b></div><div><span class="sw" style="background:var(--danger)"></span>Incorrect <b>'+s.w+'</b></div><div><span class="sw" style="background:var(--locked)"></span>Not attempted <b>'+s.n+'</b></div>'+
      '<div class="rep-lvl">Indicative level <b>'+s.lvl+'</b> / 8 <span>(band '+band(s.lvl)+')</span></div></div></div>'+
      '<button class="hub-btn" data-go="'+s.tab+'">Open Test '+s.letter+' →</button></div>';
  }).join('')+'</div>';
  h+='<p class="rep-note">Indicative level = fraction of questions correct × 8, rounded. It is a practice guide only; your teacher awards the real MYP criterion levels.</p></div>';
  var wrap=document.getElementById('wrap'); wrap.innerHTML=h;
  document.querySelector('.slide-progress').style.display='none';
  document.querySelector('.navbar').style.display='none';
  document.getElementById('paletteFab').style.display='none';
  document.getElementById('secSub').textContent='Report — chapter test results by MYP criterion';
  wrap.querySelectorAll('[data-go]').forEach(function(b){ b.addEventListener('click',function(){ activeTab=b.dataset.go; buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); }); });
}
var _rsReport = renderSlide;
renderSlide = function(){
  if(activeTab==='report'){ renderReport(); buildTabbar(); updateChapterProgress(); updateModePill(); saveState(); return; }
  _rsReport();
};


/* ================= skipped-question return ================= */
function firstSkippedIdx(ts){
  for(var i=0;i<ts.items.length;i++){
    var it=ts.items[i];
    if(MODE==='quiz'){ if(!isSlideAttempted(activeTab,i)) return i; }
    else if(it.status==='skipped'||it.status==='unanswered') return i;
  }
  return -1;
}
function skippedList(ts){
  var out=[]; for(var i=0;i<ts.items.length;i++){ var it=ts.items[i];
    if(MODE==='quiz' ? !isSlideAttempted(activeTab,i) : (it.status==='skipped'||it.status==='unanswered')) out.push(i); }
  return out;
}
function renderSkipGate(ts){
  var list=skippedList(ts);
  var h='<div class="qcard done-card"><div class="big">⏭️</div><h2>You skipped '+list.length+' question'+(list.length>1?'s':'')+'</h2>'+
    '<p style="color:var(--ink-soft);font-size:14px;">Answer them now, or submit the tab as it is. Answers are marked only when you submit.</p>'+
    '<div class="skip-chips">'+list.map(function(i){ return '<button class="chip skip-go" data-i="'+i+'">Q'+(i+1)+'</button>'; }).join('')+'</div>'+
    '<div class="skip-btns"><button class="btn btn-primary" id="btnGoSkipped">Answer skipped questions</button><button class="btn" id="btnSubmitAnyway">Submit anyway</button></div></div>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('navbarInner').innerHTML='';
  var go=function(i){ ts.idx=i; ts.maxReached=Math.max(ts.maxReached,i); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); };
  document.getElementById('btnGoSkipped').addEventListener('click',function(){ go(list[0]); });
  document.querySelectorAll('.skip-go').forEach(function(b){ b.addEventListener('click',function(){ go(parseInt(b.dataset.i,10)); }); });
  document.getElementById('btnSubmitAnyway').addEventListener('click',function(){ ts.submitOK=true; renderFinished(); ts.submitOK=false; });
  saveState();
}
var _rfSkip = renderFinished;
renderFinished = function(){
  var ts=state.tabs[activeTab];
  if(MODE==='quiz' && !ts.submitOK && skippedList(ts).length){ renderSkipGate(ts); return; }
  _rfSkip();
  if(MODE==='learning'){
    var list=skippedList(ts);
    if(list.length){
      var card=document.querySelector('.done-card');
      if(card){ var d=document.createElement('div'); d.className='skip-btns';
        d.innerHTML='<button class="btn btn-primary" id="btnGoSkipped">Attempt skipped questions ('+list.length+')</button>';
        card.appendChild(d);
        document.getElementById('btnGoSkipped').addEventListener('click',function(){ ts.idx=list[0]; ts.recorded=false; renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); }); }
    }
  }
};


/* ================= approx answers (physics: within 1%) ================= */
var _amTools = answerMatches;
answerMatches = function(input,answer,accept,expr){
  if(expr==='approx'){
    var n1=parseNum(input); if(n1===null) return false;
    return [answer].concat(accept||[]).some(function(a){ var n2=parseNum(a); if(n2===null) return false; return Math.abs(n1-n2) <= Math.max(0.011*Math.abs(n2), 1e-9); });
  }
  return _amTools(input,answer,accept,expr);
};

/* ================= palette: every question already reached can be opened ================= */
statusOf = function(tabId,i){
  var ts=state.tabs[tabId];
  if(i>ts.maxReached) return 'locked';
  if(i===ts.idx) return 'current';
  var s=ts.items[i].status;
  if(s==='unanswered') return 'skipped';
  return s;
};

/* ================= on-screen keyboard ================= */
var KB={on:true, page:'num', target:null};
try{ var kbp=localStorage.getItem('sopaan-kb-pref'); if(kbp==='off') KB.on=false; }catch(e){}
function kbSave(){ try{ localStorage.setItem('sopaan-kb-pref', KB.on?'on':'off'); }catch(e){} }
var KEYS_NUM=[['7','8','9','÷','(',')'],['4','5','6','×','^','²'],['1','2','3','−','x','π'],['0','.','/','+','t','°'],['abc','←','→','⌫','Clear','Done']];
var KEYS_ABC=[['q','w','e','r','t','y','u','i','o','p'],['a','s','d','f','g','h','j','k','l','-'],['z','x','c','v','b','n','m',',',':','/'],['123','space','←','→','⌫','Done']];
function kbBuild(){
  var el=document.getElementById('vkb'); if(!el){ el=document.createElement('div'); el.id='vkb'; el.className='vkb'; document.body.appendChild(el);
    el.addEventListener('pointerdown',function(e){ var b=e.target.closest('button[data-k]'); e.preventDefault(); if(b) kbPress(b.dataset.k); });
    el.addEventListener('mousedown',function(e){ e.preventDefault(); }); }
  var rows=KB.page==='num'?KEYS_NUM:KEYS_ABC;
  el.innerHTML='<div class="vkb-top"><span>⌨️ Keyboard</span><button data-k="device" class="vkb-link">Use device keyboard</button></div>'+
    rows.map(function(r){ return '<div class="vkb-row">'+r.map(function(k){
      var cls='vkb-k'+(/^(abc|123|Done|Clear|⌫|←|→|space)$/.test(k)?' fn':'')+(k==='Done'?' done':'');
      return '<button type="button" class="'+cls+'" data-k="'+k+'">'+(k==='space'?'space':k)+'</button>'; }).join('')+'</div>'; }).join('');
  return el;
}
function kbShow(inp){ KB.target=inp; if(!KB.on) return; var cp=document.getElementById('calcPanel'); if(cp&&cp.classList.contains('show')) return; var el=kbBuild(); var nb=document.querySelector('.navbar'); el.style.bottom=(nb?nb.getBoundingClientRect().height:0)+'px'; el.classList.add('show'); document.body.classList.add('kb-open'); setTimeout(function(){ var r=inp.getBoundingClientRect(), top=el.getBoundingClientRect().top; if(r.bottom>top-12) window.scrollBy({top:r.bottom-top+60,behavior:'smooth'}); else if(r.top<60) window.scrollBy({top:r.top-80,behavior:'smooth'}); },30); }
function kbHide(){ var el=document.getElementById('vkb'); if(el) el.classList.remove('show'); document.body.classList.remove('kb-open'); }
function kbInsert(s){
  var t=KB.target; if(!t||t.disabled) return;
  var a=t.selectionStart==null?t.value.length:t.selectionStart, b=t.selectionEnd==null?a:t.selectionEnd;
  t.value=t.value.slice(0,a)+s+t.value.slice(b); var p=a+s.length; try{ t.setSelectionRange(p,p); }catch(e){}
  t.dispatchEvent(new Event('input',{bubbles:true}));
}
function kbPress(k){
  var t=KB.target; if(k==='device'){ KB.on=false; kbSave(); kbHide(); document.querySelectorAll('.blank-input').forEach(function(i){ i.removeAttribute('inputmode'); }); if(t){ t.blur(); setTimeout(function(){ t.focus(); },50); } showToast('Device keyboard on. Tap ⌨️ on any blank to bring the on-screen keyboard back.'); return; }
  if(k==='Done'){ kbHide(); if(t) t.blur(); return; }
  if(k==='abc'||k==='123'){ KB.page=(k==='abc')?'abc':'num'; kbBuild(); return; }
  if(!t) return;
  if(k==='⌫'){ var a=t.selectionStart, b=t.selectionEnd; if(a===b&&a>0){ t.value=t.value.slice(0,a-1)+t.value.slice(b); try{t.setSelectionRange(a-1,a-1);}catch(e){} } else { t.value=t.value.slice(0,a)+t.value.slice(b); try{t.setSelectionRange(a,a);}catch(e){} } t.dispatchEvent(new Event('input',{bubbles:true})); return; }
  if(k==='Clear'){ t.value=''; t.dispatchEvent(new Event('input',{bubbles:true})); return; }
  if(k==='←'||k==='→'){ var p=(t.selectionStart||0)+(k==='←'?-1:1); p=Math.max(0,Math.min(t.value.length,p)); try{t.setSelectionRange(p,p);}catch(e){} return; }
  var map={'−':'-','space':' '}; kbInsert(map[k]!==undefined?map[k]:k);
}
function kbWire(){
  document.querySelectorAll('.blank-input').forEach(function(inp){
    if(inp.dataset.kb) return; inp.dataset.kb='1';
    if(KB.on) inp.setAttribute('inputmode','none');
    inp.addEventListener('focus',function(){ kbShow(inp); });
    var btn=document.createElement('button'); btn.type='button'; btn.className='kb-toggle'; btn.title='On-screen keyboard'; btn.textContent='⌨️'; btn.tabIndex=-1;
    btn.addEventListener('mousedown',function(e){ e.preventDefault(); });
    btn.addEventListener('click',function(){ KB.on=true; kbSave(); document.querySelectorAll('.blank-input').forEach(function(i){ i.setAttribute('inputmode','none'); }); inp.focus(); kbShow(inp); });
    if(!inp.disabled) inp.insertAdjacentElement('afterend',btn);
  });
}
document.addEventListener('focusout',function(e){ setTimeout(function(){ var a=document.activeElement; if(!a||!a.classList||!a.classList.contains('blank-input')){ if(!document.querySelector('#vkb:hover')) kbHide(); } },120); });

/* ================= calculator ================= */
var CALC={expr:'', ans:0};
function calcEval(src){
  var s=String(src).replace(/×/g,'*').replace(/÷/g,'/').replace(/−/g,'-').replace(/π/g,'(PI)').replace(/Ans/g,'('+CALC.ans+')').replace(/\^/g,'**').replace(/√\(/g,'sqrt(').replace(/²/g,'**2').replace(/E/g,'*10**');
  s=s.replace(/(^|[(*\/+\-,])-/g,'$1(-1)*');
  if(/[^0-9+\-*/().,a-z A-Z]/.test(s)) throw 0;
  var ok=s.replace(/sin|cos|tan|asin|acos|atan|sqrt|log|ln|PI|abs/g,''); if(/[a-zA-Z]/.test(ok)) throw 0;
  var f=new Function('sin','cos','tan','asin','acos','atan','sqrt','log','ln','PI','abs','return ('+s+');');
  var d=Math.PI/180;
  var v=f(function(x){return Math.sin(x*d);},function(x){return Math.cos(x*d);},function(x){return Math.tan(x*d);},
    function(x){return Math.asin(x)/d;},function(x){return Math.acos(x)/d;},function(x){return Math.atan(x)/d;},Math.sqrt,Math.log10,Math.log,Math.PI,Math.abs);
  if(typeof v!=='number'||!isFinite(v)) throw 0; return v;
}
function fmtNum(v){ if(Math.abs(v)>=1e10||(Math.abs(v)<1e-6&&v!==0)) return v.toExponential(6).replace(/\.?0+e/,'e'); return String(parseFloat(v.toPrecision(10))); }
var CKEYS=[['sin(','cos(','tan(','√(','^','²'],['asin(','acos(','atan(','(',')','π'],['7','8','9','÷','⌫','AC'],['4','5','6','×','E','Ans'],['1','2','3','−','(−)','Insert'],['0','.','=','+']];
function openCalc(){
  var p=document.getElementById('calcPanel');
  if(!p){ p=document.createElement('div'); p.id='calcPanel'; p.className='tool-panel calc';
    p.innerHTML='<div class="tp-head"><b>🧮 Calculator</b><span class="tp-note">degrees · tap a blank, then Insert</span><button class="tp-x" data-c="close">✕</button></div>'+
      '<div class="calc-disp"><div class="calc-expr" id="calcExpr"></div><div class="calc-res" id="calcRes">0</div></div>'+
      '<div class="calc-keys">'+CKEYS.map(function(r){ return r.map(function(k){ return '<button type="button" data-c="'+k+'" class="'+(k==='='?'eq':(/^(AC|⌫|Insert)$/.test(k)?'fn':''))+'">'+({'sin(':'sin','cos(':'cos','tan(':'tan','asin(':'sin⁻¹','acos(':'cos⁻¹','atan(':'tan⁻¹','√(':'√'}[k]||k)+'</button>'; }).join(''); }).join('')+'</div>';
    document.body.appendChild(p);
    p.addEventListener('mousedown',function(e){ if(e.target.closest('button')) e.preventDefault(); });
    p.addEventListener('click',function(e){ var b=e.target.closest('button[data-c]'); if(!b) return; calcKey(b.dataset.c); });
  }
  kbHide(); var nb=document.querySelector('.navbar'); p.style.bottom=((nb?nb.getBoundingClientRect().height:0)+8)+'px'; p.classList.add('show'); document.body.classList.add('calc-open'); calcShow();
}
function calcShow(res){ document.getElementById('calcExpr').textContent=CALC.expr||' '; if(res!==undefined) document.getElementById('calcRes').textContent=res; }
function calcKey(k){
  if(k==='close'){ document.getElementById('calcPanel').classList.remove('show'); document.body.classList.remove('calc-open'); return; }
  if(k==='AC'){ CALC.expr=''; calcShow('0'); return; }
  if(k==='⌫'){ CALC.expr=CALC.expr.replace(/(asin\(|acos\(|atan\(|sin\(|cos\(|tan\(|√\(|Ans|.)$/,''); calcShow(); return; }
  if(k==='='){ try{ var v=calcEval(CALC.expr); CALC.ans=v; calcShow(fmtNum(v)); }catch(e){ calcShow('Error'); } return; }
  if(k==='(−)'){ CALC.expr+='−'; calcShow(); return; }
  if(k==='Insert'){ var r=document.getElementById('calcRes').textContent; if(KB.target && !KB.target.disabled && r!=='Error'){ KB.target.value=r; KB.target.dispatchEvent(new Event('input',{bubbles:true})); showToast('Inserted '+r+' into the blank.'); } else showToast('Tap a blank first, then press Insert.'); return; }
  CALC.expr+=k; calcShow();
}

/* ================= Desmos ================= */
var DESMOS_KEY='dcb31709b452b1cf9dc26972add0fda6', desmosCalc=null, desmosLoading=false;
function openDesmos(exprs){
  var p=document.getElementById('desmosPanel');
  if(!p){ p=document.createElement('div'); p.id='desmosPanel'; p.className='tool-panel desmos';
    p.innerHTML='<div class="tp-head"><b>📈 Desmos graphing calculator</b><button class="tp-x" id="desmosReset">Reset graph</button><button class="tp-x" id="desmosClose">✕</button></div><div id="desmosBox"><div class="desmos-msg">Loading Desmos…</div></div>';
    document.body.appendChild(p);
    document.getElementById('desmosClose').addEventListener('click',function(){ p.classList.remove('show'); });
    document.getElementById('desmosReset').addEventListener('click',function(){ if(desmosCalc) desmosSet(p._exprs||[]); });
  }
  p._exprs=exprs||[]; p.classList.add('show');
  if(window.Desmos){ desmosInit(); desmosSet(p._exprs); return; }
  if(desmosLoading) return; desmosLoading=true;
  var sc=document.createElement('script'); sc.src='https://www.desmos.com/api/v1.9/calculator.js?apiKey='+DESMOS_KEY;
  sc.onload=function(){ desmosInit(); desmosSet(p._exprs); };
  sc.onerror=function(){ desmosLoading=false; document.getElementById('desmosBox').innerHTML='<div class="desmos-msg">Desmos needs an internet connection. <a href="https://www.desmos.com/calculator" target="_blank" rel="noopener">Open Desmos in a new tab</a>.</div>'; };
  document.head.appendChild(sc);
}
function desmosInit(){ if(desmosCalc) return; var box=document.getElementById('desmosBox'); box.innerHTML=''; desmosCalc=Desmos.GraphingCalculator(box,{expressionsCollapsed:false,settingsMenu:false,border:false,degreeMode:true}); }
function desmosSet(exprs){ if(!desmosCalc) return; desmosCalc.setBlank(); (exprs||[]).forEach(function(e,i){ if(typeof e==='string') desmosCalc.setExpression({id:'e'+i,latex:e}); else desmosCalc.setExpression(Object.assign({id:'e'+i},e)); }); }

/* ================= toolbar on each question ================= */
var _rsTools = renderSlide;
renderSlide = function(){
  _rsTools();
  kbHide();
  if(activeTab==='theory'||activeTab==='report') return;
  var ts=state.tabs[activeTab]; if(!ts) return;
  var slide=SLIDES[activeTab][ts.idx], card=document.querySelector('#wrap .qcard');
  if(card && slide && slide.tools && slide.tools.length && !card.querySelector('.tool-bar')){
    var bar=document.createElement('div'); bar.className='tool-bar';
    bar.innerHTML=(slide.tools.indexOf('calc')>=0?'<button type="button" class="tool-btn" data-t="calc">🧮 Calculator</button>':'')+
                  (slide.tools.indexOf('desmos')>=0?'<button type="button" class="tool-btn" data-t="desmos">📈 Desmos graph</button>':'');
    card.insertBefore(bar, card.firstChild);
    bar.addEventListener('click',function(e){ var b=e.target.closest('[data-t]'); if(!b) return; if(b.dataset.t==='calc') openCalc(); else openDesmos(slide.desmos||[]); });
  }
  kbWire();
};

renderLogin();
})();
</script>
</body>
</html>
