<!doctype html>

<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Fake Notifikasi Generator — Alwizz8.10</title>
  <!-- html2canvas CDN -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <style>
    :root{--bg:#0f1720;--card:#2b2f33;--muted:#9aa3ad;--accent:#0ea5a4}
    *{box-sizing:border-box}
    body{font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto,"Helvetica Neue",Arial;min-height:100vh;margin:0;padding:18px;background:linear-gradient(180deg,#0b0f13 0%, #101418 100%);color:#e6eef3}
    .wrap{max-width:960px;margin:0 auto;display:grid;grid-template-columns:1fr 420px;gap:20px}
    .panel{background:rgba(255,255,255,0.03);padding:14px;border-radius:12px;box-shadow:0 6px 18px rgba(2,6,23,0.6)}
    h1{font-size:18px;margin:0 0 8px}
    label{display:block;font-size:13px;color:var(--muted);margin-top:10px}
    input[type=text], input[type=number], select, textarea{width:100%;padding:8px 10px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:inherit;font-size:14px}
    textarea{min-height:70px;resize:vertical}
    .small{font-size:13px;color:var(--muted)}
    .row{display:flex;gap:8px;align-items:center}
    button.primary{background:var(--accent);border:none;color:#022;padding:8px 12px;border-radius:8px;cursor:pointer}
    button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:8px 10px;border-radius:8px;color:var(--muted);cursor:pointer}/* preview area */
.canvas-wrap{display:flex;justify-content:center;align-items:flex-start;padding:20px}
.phone{width:360px;border-radius:28px;padding:14px;background:linear-gradient(180deg,#0b0f13,#0e1317);position:relative}
.screen{width:332px;height:640px;background:linear-gradient(180deg,#111316,#0b0f13);border-radius:20px;padding:12px;overflow:auto;position:relative}

/* notification card */
.notif{width:92%;background:rgba(255,255,255,0.03);margin:14px auto;border-radius:14px;padding:12px;color:#e9eef2;backdrop-filter: blur(6px);position:relative;z-index:30}
.topbar{display:flex;justify-content:space-between;align-items:center;font-size:12px;color:var(--muted)}
.notif-body{display:flex;gap:10px;align-items:flex-start;margin-top:10px}
.avatar{width:46px;height:46px;border-radius:50%;flex:0 0 46px;background:#3b4750;overflow:hidden;border:2px solid rgba(255,255,255,0.04)}
.avatar img{width:100%;height:100%;object-fit:cover}
.meta{flex:1}
.meta b{display:inline-block}
.actions{display:flex;gap:8px;margin-top:10px}
.pill{background:rgba(255,255,255,0.03);padding:6px 10px;border-radius:999px;font-size:12px}

/* battery visual */
.battery-box{display:flex;align-items:center;gap:8px}
.battery-outer{width:46px;height:18px;border-radius:4px;border:2px solid rgba(255,255,255,0.12);display:inline-block;position:relative;padding:1px}
.battery-inner{height:100%;width:40%;background:#9ad6c9;border-radius:2px}
.battery-tip{width:4px;height:8px;background:rgba(255,255,255,0.12);position:absolute;right:-6px;top:4px;border-radius:1px}

/* blurred content area to appear as "future content" */
.content-mock{margin-top:6px;padding:12px;display:grid;gap:12px}
.content-card{height:80px;border-radius:12px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));display:flex;align-items:center;padding:12px;gap:12px;filter:blur(6px) saturate(90%);opacity:0.85;transition:filter 0.18s, opacity 0.18s}
.content-card .thumb{width:64px;height:64px;border-radius:10px;background:rgba(255,255,255,0.06)}
.content-card .text{flex:1}
.content-card h4{margin:0 0 6px;font-size:14px;color:rgba(255,255,255,0.9)}
.content-card p{margin:0;color:rgba(255,255,255,0.6);font-size:12px}

/* input area scroll */
.controls{max-height:84vh;overflow:auto;padding-right:6px}
.footnote{font-size:12px;color:var(--muted);margin-top:8px}

@media (max-width:880px){.wrap{grid-template-columns:1fr;}.canvas-wrap{order:-1}}

  </style>
</head>
<body>
  <div class="wrap">
    <div class="panel controls">
      <h1>Fake Notifikasi Generator</h1>
      <div class="small">Ganti teks, nama, foto profil, dan persen baterai iPhone. Klik <b>Download</b> untuk menyimpan sebagai gambar.</div><label>Nama Aplikasi</label>
  <input id="appName" type="text" value="GBWhatsApp" />

  <label>Nama Kontak</label>
  <input id="contactName" type="text" value="AlwizzStore" />

  <label>Pesan (preview)</label>
  <textarea id="messageText">p</textarea>

  <label>Waktu / status kecil</label>
  <input id="subText" type="text" value="Sekarang" />

  <label>Persen Baterai (0-100)</label>
  <input id="battery" type="number" value="87" min="0" max="100" />

  <label>Foto Profil (upload)</label>
  <input id="avatarFile" type="file" accept="image/*" />
  <div class="small">Atau klik avatar pada preview untuk mengganti.</div>

  <label>Teks tombol aksi (pisahkan koma)</label>
  <input id="actionText" type="text" value="Balas,Tandai dibaca,Bisukan" />

  <label>Intensity blur konten bawah (px)</label>
  <input id="blurAmount" type="number" value="6" min="0" max="24" />

  <label style="margin-top:8px"><input id="unblurOnDownload" type="checkbox" /> <span style="margin-left:8px">Unblur konten saat Download full-screen</span></label>

  <div style="margin-top:12px;display:flex;gap:8px">
    <button class="primary" id="applyBtn">Perbarui Preview</button>
    <button class="ghost" id="resetBtn">Reset</button>
  </div>

  <div style="margin-top:12px;display:flex;gap:8px">
    <button class="primary" id="downloadBtn">Download Gambar (Kartu)</button>
    <button class="ghost" id="downloadFullBtn">Download Latar Lengkap</button>
  </div>

  <div class="footnote">Catatan: Hasilnya di-download sebagai PNG. Jika ingin background transparan, set latar screen ke transparan di kode.</div>
</div>

<div class="panel canvas-wrap">
  <div class="phone" id="phone">
    <div class="screen" id="previewScreen">
      <!-- top status area -->
      <div style="display:flex;justify-content:space-between;align-items:center;font-size:12px;color:var(--muted);padding:4px 6px">
        <div id="statusLeft">GBWhatsApp · <span id="statusAppName">AlwizzStore</span></div>
        <div class="battery-box"><div id="batteryText">87%</div>
          <div class="battery-outer" aria-hidden>
            <div class="battery-inner" id="batteryFill" style="width:43%"></div>
            <div class="battery-tip"></div>
          </div>
        </div>
      </div>

      <!-- notification card (this is what will be downloaded) -->
      <div class="notif" id="notifCard">
        <div class="topbar">
          <div style="display:flex;align-items:center;gap:8px"><strong id="appTitle">GBWhatsApp</strong> · <span class="small" id="smallText">Sekarang</span></div>
          <div style="display:flex;align-items:center;gap:8px;color:var(--muted)"><span id="dotTime">Sekarang</span></div>
        </div>

        <div class="notif-body">
          <div class="avatar" id="avatarBox"><img id="avatarImg" src="https://i.ibb.co/2yX8nxX/avatar.png" alt="avatar" /></div>
          <div class="meta">
            <div><b id="contactBold">AlwizzStore</b> · <span style="color:var(--muted)" id="when">Sekarang</span></div>
            <div style="margin-top:6px;font-size:15px" id="msgText">p</div>
            <div class="actions" id="actionPills"></div>
          </div>
        </div>
      </div>

      <!-- blurred content below to appear like "future content" -->
      <div class="content-mock" id="contentMock">
        <div class="content-card">
          <div class="thumb"></div>
          <div class="text"><h4>Judul Konten 1</h4><p>Ringkasan singkat konten yang nanti terlihat di sini.</p></div>
        </div>
        <div class="content-card">
          <div class="thumb"></div>
          <div class="text"><h4>Judul Konten 2</h4><p>Contoh preview konten — blur agar terlihat background fokusnya di notifikasi.</p></div>
        </div>
        <div class="content-card">
          <div class="thumb"></div>
          <div class="text"><h4>Judul Konten 3</h4><p>Konten tambahan yang membuat layar terlihat seperti feed.</p></div>
        </div>
      </div>

      <!-- filler to make screen look natural -->
      <div style="height:40px"></div>
    </div>
  </div>
</div>

  </div>  <script>
    // helper to set initial action pills
    function setActionPills(text){
      const container = document.getElementById('actionPills');
      container.innerHTML='';
      const parts = text.split(',').map(s=>s.trim()).filter(Boolean);
      for(const p of parts){
        const sp = document.createElement('div'); sp.className='pill'; sp.innerText=p; container.appendChild(sp);
      }
    }

    function applyPreview(){
      const appName = document.getElementById('appName').value || 'App';
      const contact = document.getElementById('contactName').value || 'Contact';
      const msg = document.getElementById('messageText').value || '';
      const sub = document.getElementById('subText').value || 'Sekarang';
      const battery = Math.max(0,Math.min(100, parseInt(document.getElementById('battery').value || 0)));
      const actionText = document.getElementById('actionText').value || '';
      const blur = Math.max(0, Math.min(24, parseInt(document.getElementById('blurAmount').value || 6)));

      document.getElementById('appTitle').innerText = appName;
      document.getElementById('statusAppName').innerText = contact;
      document.getElementById('contactBold').innerText = contact;
      document.getElementById('msgText').innerText = msg;
      document.getElementById('when').innerText = sub;
      document.getElementById('smallText').innerText = sub;
      document.getElementById('batteryText').innerText = battery + '%';

      // battery fill width
      document.getElementById('batteryFill').style.width = Math.max(6, battery * 0.4) + '%';
      // change fill color depending on level
      const fill = document.getElementById('batteryFill');
      if(battery>50) fill.style.background='#9ad6c9';
      else if(battery>20) fill.style.background='#ffd86b';
      else fill.style.background='#ff6b6b';

      setActionPills(actionText);

      // adjust blur intensity for content mock
      const cards = document.querySelectorAll('.content-card');
      cards.forEach(c=> c.style.filter = `blur(${blur}px) saturate(90%)`);
    }

    // initial
    applyPreview();

    document.getElementById('applyBtn').addEventListener('click', applyPreview);
    document.getElementById('resetBtn').addEventListener('click', ()=>{
      document.getElementById('appName').value='GBWhatsApp';
      document.getElementById('contactName').value='AlwizzStore';
      document.getElementById('messageText').value='p';
      document.getElementById('subText').value='Sekarang';
      document.getElementById('battery').value=87;
      document.getElementById('actionText').value='Balas,Tandai dibaca,Bisukan';
      document.getElementById('avatarImg').src='https://i.ibb.co/2yX8nxX/avatar.png';
      document.getElementById('blurAmount').value = 6;
      document.getElementById('unblurOnDownload').checked = true;
      applyPreview();
    });

    // avatar upload
    const avatarFile = document.getElementById('avatarFile');
    avatarFile.addEventListener('change', (e)=>{
      const f = e.target.files?.[0];
      if(!f) return;
      const reader = new FileReader();
      reader.onload = ()=>{ document.getElementById('avatarImg').src = reader.result; };
      reader.readAsDataURL(f);
    });

    // click avatar in preview to trigger upload (nice UX)
    document.getElementById('avatarBox').addEventListener('click', ()=> avatarFile.click());

    // download the notification card only
    document.getElementById('downloadBtn').addEventListener('click', async ()=>{
      const card = document.getElementById('notifCard');
      html2canvas(card, {backgroundColor: null, scale:2}).then(canvas=>{
        const link = document.createElement('a');
        link.download = 'fake_notification.png';
        link.href = canvas.toDataURL('image/png');
        link.click();
      });
    });

    // download full phone screen (with phone frame)
    document.getElementById('downloadFullBtn').addEventListener('click', async ()=>{
      const screen = document.getElementById('previewScreen');
      const unblur = document.getElementById('unblurOnDownload').checked;
      const cards = document.querySelectorAll('.content-card');

      // if user wants unblur on download, temporarily remove blur & increase opacity
      const oldStyles = [];
      if(unblur){
        cards.forEach(c=>{
          oldStyles.push({el:c,filter:c.style.filter,opacity:c.style.opacity});
          c.style.filter = 'none';
          c.style.opacity = '1';
        });
      }

      // small delay to ensure styles applied
      setTimeout(()=>{
        html2canvas(screen, {backgroundColor:null, scale:2}).then(canvas=>{
          const link = document.createElement('a');
          link.download = unblur ? 'fake_notification_full_unblur.png' : 'fake_notification_full.png';
          link.href = canvas.toDataURL('image/png');
          link.click();

          // restore styles
          if(unblur){
            oldStyles.forEach(o=>{ o.el.style.filter = o.filter; o.el.style.opacity = o.opacity; });
          }
        }).catch(err=>{
          if(unblur){ oldStyles.forEach(o=>{ o.el.style.filter = o.filter; o.el.style.opacity = o.opacity; }); }
          alert('Gagal membuat gambar: '+err.message);
        });
      }, 120);
    });

    // allow Enter to update preview from inputs
    ['appName','contactName','messageText','battery','subText','actionText','blurAmount'].forEach(id=>{
      const el=document.getElementById(id);
      el.addEventListener('keydown', (e)=>{ if(e.key==='Enter'){ e.preventDefault(); applyPreview(); } });
    });
  </script></body>
</html>
