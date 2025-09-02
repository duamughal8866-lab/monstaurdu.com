<!doctype html>
<html lang="ur" dir="rtl">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>MonstaUrdu — Cartoon Demo (Local)</title>
<style>
  /* Simple cartoon-ish styles */
  @import url('https://fonts.googleapis.com/css2?family=Baloo+2:wght@400;700&display=swap');
  :root{
    --bg1:#fff7e6; --bg2:#e8f7ff; --accent:#ffb703; --accent2:#3a86ff;
    --card:#ffffff; --muted:#6b7280;
  }
  *{box-sizing:border-box}
  body{margin:0;font-family:'Baloo 2',system-ui,Arial; background:linear-gradient(120deg,var(--bg1),var(--bg2)); color:#111;}
  header{background:linear-gradient(90deg,var(--accent),var(--accent2)); color:#fff; padding:18px; text-align:center; font-size:26px; font-weight:800; box-shadow:0 6px 18px rgba(0,0,0,0.08)}
  .container{max-width:920px;margin:18px auto;padding:12px}
  .card{background:var(--card);border-radius:14px;padding:12px;box-shadow:0 6px 22px rgba(16,24,40,0.06); margin-bottom:14px}
  input,textarea,select,button{font-family:inherit}
  .row{display:flex;gap:8px;flex-wrap:wrap}
  .col{flex:1;min-width:160px}
  input[type=text],textarea{width:100%;padding:10px;border-radius:10px;border:2px dashed #f0f0f0}
  .btn{background:var(--accent2);color:#fff;border:0;padding:10px 12px;border-radius:12px;cursor:pointer;font-weight:700}
  .btn.alt{background:#fff;color:var(--accent2);border:2px solid var(--accent2)}
  .small{font-size:13px;color:var(--muted)}
  .video-card{display:grid;grid-template-columns:1fr 220px;gap:12px;align-items:center}
  iframe{width:100%;height:160px;border-radius:12px;border:0}
  .list-item{padding:10px;border-radius:10px;border:1px solid #f3f3f3;background:linear-gradient(180deg,#fff,#fffef8)}
  .flex{display:flex;gap:8px;align-items:center}
  .badge{background:var(--accent);color:#fff;padding:6px 8px;border-radius:999px;font-weight:800;font-size:13px}
  .muted{color:var(--muted);font-size:13px}
  footer{text-align:center;color:var(--muted);font-size:13px;margin:18px 0}
  .center{display:flex;justify-content:center;align-items:center}
  /* responsive */
  @media(max-width:720px){
    .video-card{grid-template-columns:1fr}
    iframe{height:200px}
  }
</style>
</head>
<body>
<header>MonstaUrdu (Local Cartoon Demo)</header>
<div class="container">

  <div class="card" id="authCard">
    <div class="flex" style="justify-content:space-between;align-items:center">
      <div>
        <div style="font-weight:800;font-size:18px">لاگ اِن / سائن اپ</div>
        <div class="small">اپنا email اور نام لکھیں — Admin: <strong>urdumonsta@gmail.com</strong></div>
      </div>
      <div class="flex">
        <button class="btn" id="btnShowLogin">Login</button>
        <button class="btn alt" id="btnShowSignup">Signup</button>
      </div>
    </div>

    <div id="loginBox" style="margin-top:12px;display:none">
      <input type="text" id="loginEmail" placeholder="Email (مثال: urdumonsta@gmail.com)" />
      <input type="text" id="loginName" placeholder="Name (صرف demo کے لیے)" style="margin-top:8px" />
      <div class="row" style="margin-top:8px">
        <button class="btn" id="btnLoginDo">Login as User</button>
        <button class="btn alt" id="btnGuest">Continue as Guest (no signup)</button>
      </div>
      <div class="small" style="margin-top:8px">نوٹ: یہ demo localStorage استعمال کرتا ہے — حقیقی auth کے لیے Supabase/Server درکار ہوگا</div>
    </div>

    <div id="signupBox" style="margin-top:12px;display:none">
      <input type="text" id="signupEmail" placeholder="Email" />
      <input type="text" id="signupName" placeholder="Full name" style="margin-top:8px" />
      <div class="row" style="margin-top:8px">
        <button class="btn" id="btnSignupDo">Create Account</button>
      </div>
      <div class="small" style="margin-top:8px">Signup کرنے کے بعد آپ public comments کر سکتے ہیں</div>
    </div>
  </div>

  <div class="card" id="adminPanel" style="display:none">
    <div class="flex" style="justify-content:space-between">
      <div><div class="badge">Admin</div><div class="muted" id="adminEmailShow" style="margin-top:6px"></div></div>
      <div class="flex">
        <button class="btn" id="btnAddVideoToggle">Add Video</button>
        <button class="btn alt" id="btnAddPostToggle">Add Post</button>
        <button class="btn alt" id="btnExport">Export JSON</button>
        <button class="btn" id="btnClearAll">Clear All</button>
      </div>
    </div>

    <div id="addVideoForm" style="margin-top:12px;display:none">
      <input type="text" id="vidTitle" placeholder="Video Title" />
      <input type="text" id="vidLink" placeholder="YouTube link (https://...)" style="margin-top:8px" />
      <div class="row" style="margin-top:8px">
        <button class="btn" id="btnSaveVideo">Save Video</button>
      </div>
    </div>

    <div id="addPostForm" style="margin-top:12px;display:none">
      <input type="text" id="postTitle" placeholder="Post Title" />
      <textarea id="postContent" placeholder="Post content..." style="margin-top:8px"></textarea>
      <div class="row" style="margin-top:8px">
        <button class="btn" id="btnSavePost">Save Post</button>
      </div>
    </div>

    <div id="adminMsg" class="small" style="margin-top:8px;color:green"></div>
  </div>

  <div class="card" id="publicArea" style="display:none">
    <div class="flex" style="justify-content:space-between;align-items:center">
      <div><div style="font-weight:900;font-size:20px">تازہ ویڈیوز</div><div class="small">دیکھنے کے لیے ویڈیو کھولیں</div></div>
      <div class="muted">Welcome: <span id="whoName">Guest</span></div>
    </div>

    <div id="videosList" style="margin-top:12px" class="grid"></div>

    <hr style="margin:14px 0" />

    <div style="font-weight:900;font-size:20px">تازہ پوسٹس</div>
    <div id="postsList" style="margin-top:12px"></div>
  </div>

</div>

<footer>MonstaUrdu — Local Demo • Save this HTML to use offline</footer>

<script>
/*
  Single-file MonstaUrdu demo.
  - Uses localStorage to store: users, currentUser, videos, posts, comments
  - Admin identified by email: urdumonsta@gmail.com
  - No server; everything runs locally in browser.
*/

// helpers
const $ = id => document.getElementById(id)
const LS = {
  users: 'mu_users_v1',
  current: 'mu_current_v1',
  videos: 'mu_videos_v1',
  posts: 'mu_posts_v1',
  comments: 'mu_comments_v1'
}
const adminEmail = 'urdumonsta@gmail.com'

// read/write
function read(key, def){ try{ const v = localStorage.getItem(key); return v ? JSON.parse(v) : def }catch(e){ return def } }
function write(key, val){ localStorage.setItem(key, JSON.stringify(val)) }

// initial load
if(!read(LS.users, null)){
  write(LS.users, []) // empty users
}
if(!read(LS.videos, null)){
  write(LS.videos, []) // empty videos
}
if(!read(LS.posts, null)){
  write(LS.posts, [])
}
if(!read(LS.comments, null)){
  write(LS.comments, [])
}

// UI state
function showLogin(){ $('loginBox').style.display='block'; $('signupBox').style.display='none' }
function showSignup(){ $('loginBox').style.display='none'; $('signupBox').style.display='block' }

// event bindings
$('btnShowLogin').onclick = showLogin
$('btnShowSignup').onclick = showSignup
$('btnGuest').onclick = ()=>{ loginGuest(); }
$('btnLoginDo').onclick = loginFromForm
$('btnSignupDo').onclick = signupFromForm

$('btnAddVideoToggle').onclick = ()=> toggleEl('addVideoForm')
$('btnAddPostToggle').onclick = ()=> toggleEl('addPostForm')
$('btnSaveVideo').onclick = saveVideo
$('btnSavePost').onclick = savePost
$('btnExport').onclick = exportAll
$('btnClearAll').onclick = clearAll

function toggleEl(id){ const e=$$(id); e.style.display = e.style.display==='none' ? 'block' : 'none' }
function $$(id){ return document.getElementById(id) }

// Auth functions (local demo)
function signupFromForm(){
  const email = $('signupEmail').value.trim()
  const name = $('signupName').value.trim() || email.split('@')[0]
  if(!email){ alert('Email likhiye'); return }
  const users = read(LS.users, [])
  if(users.find(u=>u.email===email)){ alert('Email already exists. Please login.'); return }
  const id = 'u'+Date.now()
  users.push({ id, email, name, role: (email===adminEmail)?'admin':'user' })
  write(LS.users, users)
  setCurrent({ id, email, name, role: (email===adminEmail)?'admin':'user' })
  refreshUI()
  alert('Signup successful. You are logged in.')
}

function loginFromForm(){
  const email = $('loginEmail').value.trim()
  const name = $('loginName').value.trim() || email.split('@')[0]
  if(!email){ alert('Email likhiye'); return }
  const users = read(LS.users, [])
  let user = users.find(u=>u.email===email)
  if(!user){
    // create simple user (demo)
    user = { id:'u'+Date.now(), email, name, role: (email===adminEmail)?'admin':'user'}
    users.push(user); write(LS.users, users)
  }
  setCurrent(user)
  refreshUI()
  alert('Logged in as '+user.email)
}

function loginGuest(){
  setCurrent({ id:'guest', email:'', name:'Guest', role:'guest' })
  refreshUI()
}

function setCurrent(user){ write(LS.current, user) }
function getCurrent(){ return read(LS.current, null) }

// Admin actions
function saveVideo(){
  const title = $('vidTitle').value.trim()
  const link = $('vidLink').value.trim()
  if(!title || !link){ alert('Title aur Link zaroori hain'); return }
  const yid = parseYouTube(link)
  const videos = read(LS.videos, [])
  videos.unshift({ id: 'v'+Date.now(), title, youtube_id: yid, original: link, createdAt: new Date().toLocaleString() })
  write(LS.videos, videos)
  $('vidTitle').value=''; $('vidLink').value=''
  adminMsg('Video added')
  renderPublic()
}

function savePost(){
  const title = $('postTitle').value.trim()
  const content = $('postContent').value.trim()
  if(!title || !content){ alert('Title aur content zaroori'); return }
  const posts = read(LS.posts, [])
  posts.unshift({ id:'p'+Date.now(), title, content, createdAt: new Date().toLocaleString() })
  write(LS.posts, posts)
  $('postTitle').value=''; $('postContent').value=''
  adminMsg('Post added')
  renderPublic()
}

function adminMsg(t){ $('adminMsg').textContent = t; setTimeout(()=> $('adminMsg').textContent='',3000) }
function exportAll(){
  const payload = { users: read(LS.users,[]), videos: read(LS.videos,[]), posts: read(LS.posts,[]), comments: read(LS.comments,[]) }
  const blob = new Blob([JSON.stringify(payload, null,2)], {type:'application/json'})
  const url = URL.createObjectURL(blob)
  const a = document.createElement('a'); a.href=url; a.download='monstaurdu-data.json'; a.click()
  URL.revokeObjectURL(url)
}

// Public rendering
function renderPublic(){
  const videos = read(LS.videos, [])
  const posts = read(LS.posts, [])
  const vbox = $('videosList'); vbox.innerHTML = ''
  if(videos.length==0) vbox.innerHTML = '<div class="muted">کوئی ویڈیو موجود نہیں</div>'
  videos.forEach(v=>{
    const div = document.createElement('div'); div.className='list-item'
    div.innerHTML = `<div class="video-card">
      <div>
        <div style="font-weight:800">${escapeHtml(v.title)}</div>
        <div class="muted">${escapeHtml(v.createdAt||'')}</div>
        <div style="margin-top:8px"><button class="btn" onclick="openVideo('${v.id}')">Play</button> <button class="btn alt" onclick="shareLink('${v.original||v.youtube_id}')">Share</button></div>
      </div>
      <div><iframe src="https://www.youtube.com/embed/${v.youtube_id}" frameborder="0" allowfullscreen></iframe></div>
    </div>`
    vbox.appendChild(div)
  })

  const pbox = $('postsList'); pbox.innerHTML = ''
  if(posts.length==0) pbox.innerHTML = '<div class="muted">کوئی پوسٹ نہیں</div>'
  posts.forEach(p=>{
    const div = document.createElement('div'); div.className='list-item'; div.style.marginBottom='8px'
    div.innerHTML = `<div style="font-weight:800">${escapeHtml(p.title)}</div><div class="muted">${escapeHtml(p.createdAt||'')}</div><div style="margin-top:8px">${escapeHtml(p.content)}</div>`
    pbox.appendChild(div)
  })
}

// open video page (simple modal)
function openVideo(id){
  const v = read(LS.videos,[]).find(x=>x.id===id); if(!v) return
  const modal = document.createElement('div')
  modal.style.position='fixed'; modal.style.inset='0'; modal.style.background='rgba(0,0,0,0.6)'; modal.style.display='flex'; modal.style.alignItems='center'; modal.style.justifyContent='center'; modal.style.zIndex=9999
  modal.innerHTML = `<div style="background:#fff;border-radius:12px;padding:12px;max-width:920px;width:95%;max-height:90%;overflow:auto">
    <div style="display:flex;justify-content:space-between;align-items:center">
      <div style="font-weight:800">${escapeHtml(v.title)}</div>
      <button class="btn alt" onclick="document.body.removeChild(this.parentNode.parentNode.parentNode)">بند کریں</button>
    </div>
    <div style="margin-top:10px"><iframe src="https://www.youtube.com/embed/${v.youtube_id}" width="100%" height="360" frameborder="0" allowfullscreen></iframe></div>
    <div style="margin-top:12px"><h3 style="margin:0 0 6px 0">Comments</h3><div id="cmtsArea"></div>
    <textarea id="cmtText" placeholder="اپنا کمنٹ لکھیں..." style="width:100%;margin-top:8px;padding:8px;border-radius:8px"></textarea>
    <div class="row" style="margin-top:8px"><button class="btn" onclick="submitComment('${v.id}')">Post Comment</button></div></div>
  </div>`
  document.body.appendChild(modal)
  renderCommentsFor(v.id)
}

function submitComment(videoId){
  const txt = document.getElementById('cmtText').value.trim()
  if(!txt){ alert('Comment likhiye'); return }
  const cur = getCurrent()
  if(!cur || cur.role==='guest'){ alert('Login kar ke comment karein'); return }
  const comments = read(LS.comments, [])
  comments.unshift({ id:'c'+Date.now(), video_id: videoId, author_id: cur.id, author: cur.name, content: txt, approved:true, createdAt: new Date().toLocaleString() })
  write(LS.comments, comments)
  renderCommentsFor(videoId)
  document.getElementById('cmtText').value=''
}

function renderCommentsFor(videoId){
  const area = document.getElementById('cmtsArea'); if(!area) return
  const comments = read(LS.comments, []).filter(c=>c.video_id===videoId)
  area.innerHTML = comments.map(c=>`<div style="padding:8px;border-radius:8px;border:1px solid #f3f3f3;margin-bottom:8px"><div style="font-weight:700">${escapeHtml(c.author||'User')}</div><div class="muted">${escapeHtml(c.createdAt||'')}</div><div style="margin-top:6px">${escapeHtml(c.content)}</div></div>`).join('')
}

// utilities
function parseYouTube(url){
  try{
    const u = new URL(url); if(u.hostname.includes('youtu.be')) return u.pathname.slice(1)
    if(u.searchParams.get('v')) return u.searchParams.get('v')
    const m = url.match(/(?:embed|shorts)\\/([\\w-]+)/)
    return m ? m[1] : url
  }catch(e){ return url }
}
function escapeHtml(s){ return String(s||'').replace(/[&<>"]/g, c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c])) }

// misc
function shareLink(s){ try{ navigator.share ? navigator.share({title:'MonstaUrdu',text:'Video',url:s}) : prompt('Copy link', s) }catch(e){ prompt('Copy link', s) } }
function clearAll(){ if(confirm('سارا local data مٹائیں؟')){ localStorage.removeItem(LS.videos); localStorage.removeItem(LS.posts); localStorage.removeItem(LS.comments); renderPublic(); alert('Deleted') } }

// init UI based on current user
function refreshUI(){
  const cur = getCurrent()
  if(!cur){ $('authCard').style.display='block'; $('publicArea').style.display='none'; $('adminPanel').style.display='none'; return }
  $('authCard').style.display='none'; $('publicArea').style.display='block'; $('whoName').textContent = cur.name || 'User'
  if(cur.role==='admin'){ $('adminPanel').style.display='block'; $('adminEmailShow').textContent = cur.email } else { $('adminPanel').style.display='none' }
  renderPublic()
}

// set current if saved
if(!getCurrent()) { /* nothing */ } else { /* keep */ }
refreshUI()

// small helper to create a default sample video if none exist
(function maybeSeed(){
  const v = read(LS.videos, [])
  if(v.length===0){
    v.push({ id:'v0', title:'Sample Cartoon Episode', youtube_id:'dQw4w9WgXcQ', original:'https://www.youtube.com/watch?v=dQw4w9WgXcQ', createdAt:new Date().toLocaleString() })
    write(LS.videos, v)
  }
  renderPublic()
})();

</script>
</body>
</html>
