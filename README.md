
מערכת לניהול לקוחות אונליין
<!DOCTYPE html>
<html lang="he">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>ניהול לקוחות זומבים — מקצועי</title>


<script>
// מניעת מקש ימני
document.addEventListener('contextmenu', function(e){
  e.preventDefault();
  toast("פעולה זו מנועה", "error"); // הודעה למשתמש
});

// מניעת Ctrl+C / Ctrl+X / Ctrl+U
document.addEventListener('keydown', function(e){
  if (e.ctrlKey && (e.key === 'c' || e.key === 'x' || e.key === 'u')) {
    e.preventDefault();
    toast("פעולה זו מנועה", "error");
  }
});
</script>



<script>
// זמן ניתוק בדקות
const AUTO_LOGOUT_MINUTES = 5;
let logoutTimer;

// הפעלת הטיימר מחדש בעת פעילות
function resetLogoutTimer() {
  clearTimeout(logoutTimer);
  logoutTimer = setTimeout(() => {
    toast("אין פעילות – התנתקת אוטומטית", "error");
    logout(); // הפונקציה שלך להתנתקות
  }, AUTO_LOGOUT_MINUTES * 60 * 1000);
}

// מאזינים לאירועי פעילות
['mousemove', 'keydown', 'scroll', 'click'].forEach(evt => {
  document.addEventListener(evt, resetLogoutTimer);
});

// התחלת הטיימר בעת טעינת הדף אם המשתמש מחובר
window.addEventListener('DOMContentLoaded', () => {
  if(localStorage.getItem('adminLogged') === '1'){
    resetLogoutTimer();
  }
});
</script>











<style>
:root{
  --primary:#2E71B3; 
  --accent:#0F0880; 
  --dark:#1A1A1A; 
  --hover-row:#FFFFFF;
  --maxw:1650px;
}
body{
  font-family:'Segoe UI',Tahoma,sans-serif;
  margin:0; padding:0; direction:rtl;
  background:linear-gradient(F7892F,#f0f4f8,#e8f5e9);
  color:#111;
}
header{
  background:linear-gradient(90deg,var(--primary),var(--accent));
  color:#EBC26A;
  padding:16px 24px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  box-shadow:0 4px 6px rgba(0,0,0,.12);
  position:sticky;
  top:0;
  z-index:1000;
}
.logo{
  font-size:20px;
  font-weight:800;
  text-shadow:1px 1px 2px rgba(0,0,0,.15);
}
nav ul{
  list-style:none; margin:0; padding:0; display:flex; gap:2px; align-items:center;
}
nav a{
  color:white; text-decoration:none; padding:8px 12px; border-radius:6px; transition:background .18s; font-size:18px;
}
nav a:hover{ background:rgba(255,255,255,0.12); }
nav li{ position:relative; }
nav li ul{
  display:none;
  position:absolute;
  top:36px;
  right:0;
  background:var(--accent);
  padding:8px 0;
  border-radius:4px;
  min-width:180px;
}
nav li:hover ul{ display:block; }
nav li ul a{ display:block; padding:8px 14px; color:#FFFFFF; }




main.wrapper{
  max-width:var(--maxw);
  margin:20px auto;
  padding:20px;
  gap:18px;
  display:flex;
  flex-direction:column;
  min-height:calc(100vh - 220px);




}
.container{ display:flex; gap:18px; align-items:flex-start; }
.right-panel{ width:300px; display:flex; flex-direction:column; gap:14px; }
.tables-box{ flex:1; display:flex; flex-direction:column; gap:14px; }

.card{ background:#82D5ED; padding:15px; border-radius:10px; box-shadow:0 6px 16px rgba(0,0,0,.06); }

input, textarea, select {
  width: 100%;              /* תופס את רוחב הקונטיינר */
  padding: 6px 8px;         /* רווח פנימי נוח לקריאה */
  margin: 4px 0;            /* רווח בין השדות */
  font-size: 16px;          /* גודל טקסט אחיד */
  line-height: 1.4;         /* שורת טקסט נוחה לקריאה */
  border-radius: 6px;       /* פינות מעוגלות */
  border: 1px solid #d0d7de; /* גבול עדין */
  box-sizing: border-box;   /* כולל padding ברוחב */
}

/* אם רוצים ש-inlinedit בטבלה יקבל את אותו סגנון */
tr input, tr textarea, tr select {
  font-size: 16px;
  padding: 4px 6px;
}
button{
  padding:5px 5px; border:none; border-radius:8px; cursor:pointer;
  font-size:16px; transition:0.2s; white-space:nowrap;
}
button:hover{ opacity:0.95; transform:scale(1.03); }

.save{ background:#49C449; color:white; }
.archive{ background:#1FFFAE; color:#000000; }
.delete{ background:#F52116; color:white; }
.edit{ background:#5bc0de; color:white; }
.search-btn{ background:#337ab7; color:white; }
.reset-search{ background:#777; color:#fff; }
.logout-btn{ background:#f54291; color:white; margin-top:6px; }


table{
  width:100%; border-collapse:collapse; font-size:18px;
  border-radius:12px; overflow:hidden; box-shadow:0 6px 12px rgba(0,0,0,.04);
}
th,td{ padding:5px 5px; text-align:center; border-bottom:1px solid #000000; cursor:default; }
thead th{ background:linear-gradient(#FFFF1C,#FFFF1C,#FFFF1C); font-weight:700; cursor:pointer; }
tbody tr:hover{ background:var(--hover-row); }
tr.empty td{ text-align:center; color:#666; padding:18px; }

table.inactive thead th{ background:#d9534f; color:#fff; }
table.archive thead th{ background:#f0ad4e; color:#fff; }

.status-paid{ color:#147A00; font-weight:600; }
.status-unpaid{ color:#d9534f; font-weight:600; }
.status-trial{ color:#0275d8; font-weight:600; }
.status-blocked{ color:#555; font-weight:600; }




tr[data-status="שילם"]{ background:#EDEDED; }
tr[data-status="לא שילם"]{ background:#fde0e0; }
tr[data-status="מתנסה"]{ background:#e0f0fd; }
tr[data-status="חסום"]{ background:#fde0e0; color:#a00; }

.highlight{ background:yellow; }

#toast{
  position:fixed; top:30px; right:50%; transform:translateX(50%);
  background:rgba(0,0,0,.85); color:#fff; padding:12px 20px; border-radius:6px;
  opacity:0; pointer-events:none; transition:opacity .4s, transform .4s; z-index:2000;
  display:flex; align-items:center; gap:8px;
}
#toast.show{ opacity:1; transform:translateX(50%) translateY(0); }

.pagination-buttons{ display:flex; justify-content:center; padding:12px 0; gap:6px; flex-wrap:wrap; }
.pagination-buttons button{ min-width:40px; }

.actions{ display:flex; justify-content:center; gap:6px; flex-wrap:wrap; }
.actions button{ flex:1; min-width:80px; }


/* --- טבלה של לקוחות פעילים --- */
#customerTable, #customerTable td, #customerTable th {
  color: #222;
  font-size: 18px;
}
#customerTable tbody tr {
  background: #e0f7fa; /* רקע שורות פעילים */
}
#customerTable tbody tr:hover {
  background: #FFFFFF; /* hover שורות פעילים */
}

/* --- טבלה של לקוחות לא פעילים --- */
#inactiveTable, #inactiveTable td, #inactiveTable th {
  color: #444;
  font-size: 17px;
}
#inactiveTable tbody tr {
  background: #fff3e0; /* רקע שורות לא פעילים */
}
#inactiveTable tbody tr:hover {
  background: #FFFFFF; /* hover שורות לא פעילים */
}

/* --- טבלת ארכיון --- */
#archiveTable, #archiveTable td, #archiveTable th {
  color: #111;
  font-size: 17px;
}
#archiveTable tbody tr {
  background: #fff3e0; /* רקע שורות ארכיון */
}
#archiveTable tbody tr:hover {
  background: #FFFFFF; /* hover שורות ארכיון */
}

.footer{ background:var(--dark); color:#fff; padding:50px 20px; text-align:center; margin-top:30px; }
.footer a{ color:#fff; text-decoration:none; margin:0 8px; }

#loginScreen{
  position:fixed; top:0; left:0; width:100%; height:100%;
  background:#0b63a8; display:flex; justify-content:center; align-items:center; flex-direction:column; gap:12px;
  z-index:3000;
}
#loginScreen input{ width:200px; font-size:18px; }
#loginScreen button{ width:100px; }




@media(max-width:900px){ .container{ flex-direction:column; } .right-panel{ width:100%; } }
</style>
</head>
<body>




<div id="loginScreen">
  <h2 style="color:white;">כניסה למערכת🔑</h2>
  <input type="password" id="adminPassword" placeholder="סיסמת מנהל" autofocus onkeypress="if(event.key==='Enter'){login();}">
  <button onclick="login()">כניסה</button>
</div>





<header>
  <div class="logo">ניהול לקוחות CRM </div>
  <nav>
    <ul>
      <li><a href="file:///C:/Users/Admin/OneDrive/%D7%A9%D7%95%D7%9C%D7%97%D7%9F%20%D7%94%D7%A2%D7%91%D7%95%D7%93%D7%94/lekohot.html#">בית</a></li>


      <li><a href="#">לקוחות ▾</a>
        <ul>
    <li><a href="file:///C:/Users/Admin/OneDrive/%D7%A9%D7%95%D7%9C%D7%97%D7%9F%20%D7%94%D7%A2%D7%91%D7%95%D7%93%D7%94/%D7%90%D7%A0%D7%9C%D7%99%D7%98%D7%A7%D7%A1%20%D7%9C%D7%A7%D7%95%D7%97%D7%95%D7%AA.html">דוחות</a>
      </li>
          <li><a href="#">לקוחות פעילים</a></li>
          <li><a href="#">לקוחות לא פעילים</a></li>
          <li><a href="#">ארכיון</a></li>
        </ul>
      </li>
  
      <li><a href="file:///C:/Users/Admin/OneDrive/%D7%A9%D7%95%D7%9C%D7%97%D7%9F%20%D7%94%D7%A2%D7%91%D7%95%D7%93%D7%94/%D7%AA%D7%96%D7%9B%D7%95%D7%A8%D7%AA.html">תזכורת🔔</a></li>
      <li><a href="#">צור קשר</a></li>
    </ul>
  </nav>
</header>

<main class="wrapper">
  <h1 id="title" style="text-align:right; margin:0 6px 6px 0; color:var(--primary); font-size:22px;"></h1>
  <div class="container">
    <aside class="right-panel">









<div class="card search-card">
  <div class="search-wrapper">
    <input type="text" id="searchInput" placeholder="חיפוש לקוחות לפי שם, נייד, אימייל" onkeypress="if(event.key==='Enter'){manualSearch();}">
    <button class="icon-btn search-btn" onclick="manualSearch()">
      🔍 חיפוש
    </button>
    <button class="icon-btn reset-search" onclick="resetSearch()" title="איפוס חיפוש">
      🧹ניקוי חיפוש
    </button>
  </div>
</div>


      <div class="card">
        <h3>הוספת לקוח</h3>
        <input type="text" id="name" placeholder="שם מלא">
        <input type="email" id="email" placeholder="אימייל">
        <input type="text" id="mobile" placeholder="מספר נייד">
        <input type="number" step="0.01" id="amount" placeholder="סכום כסף">
        <textarea id="comment" placeholder="הערה" rows="3"></textarea>
        <label>סטטוס לקוח:</label>
        <select id="status">
          <option>שילם</option>
          <option>מתנסה</option>
          <option>לא שילם</option>
          <option>חסום</option>
        </select>
        <div class="actions" style="margin-top:8px;">
          <button class="save" onclick="addCustomer()">שמור💾</button>
          <button class="reset-search" onclick="resetAddCustomerForm()">איפוס 🧹</button>
          <button class="archive" onclick="archiveAllCustomers()">הכל לארכיון📦</button>
          <button class="delete" onclick="deleteSelectedCustomers()">מחק נבחרים🗑️</button>
          <button class="logout-btn" onclick="logout()">יציאה⏻</button>
        </div>
      </div>
    </aside>
    <section class="tables-box">
      <div class="card"><h2>לקוחות פעילים🟢</h2>
        <div style="overflow:auto; max-height:350px;">
          <table id="customerTable" class="active"></table>
          <div id="pagination-customers" class="pagination-buttons"></div>
        </div>
      </div>
      <div class="card"><h2>לקוחות לא פעילים⚪</h2>
        <div style="overflow:auto; max-height:350px;">
          <table id="inactiveTable" class="inactive"></table>
          <div id="pagination-inactive" class="pagination-buttons"></div>
        </div>
      </div>
      <div class="card"><h2>ארכיון📦</h2>
        <div style="overflow:auto; max-height:350px;">
          <table id="archiveTable" class="archive"></table>
          <div id="pagination-archive" class="pagination-buttons"></div>
        </div>
      </div>
    </section>
  </div>
</main>

<div id="toast"></div>

<footer class="footer">
  <div><a href="#">בית</a><a href="#">לקוחות</a><a href="#">צור קשר</a></div>
  <div style="font-size:13px; opacity:0.9;">© 2025 ניהול לקוחות זומבים. כל הזכויות שמורות.</div>
</footer>

<script>
const DELETE_PASSWORD="5135";
const ADMIN_PASSWORD="5135";

// ====== Login / Logout ======
function login(){
  const pass = document.getElementById('adminPassword').value;
  if(pass===ADMIN_PASSWORD){
    localStorage.setItem('adminLogged', '1');
    document.getElementById('loginScreen').style.display='none';
    renderAll();
  } else { alert("סיסמה שגויה"); }
}

function logout(){
  localStorage.removeItem('adminLogged');
  location.reload();
}

// ====== Check login on load ======
window.addEventListener('DOMContentLoaded', ()=>{
  if(localStorage.getItem('adminLogged')==='1'){
    document.getElementById('loginScreen').style.display='none';
    renderAll();
  } else {
    document.getElementById('loginScreen').style.display='flex';
  }
});

// ====== Toast ======
function toast(msg,type="info"){ 
  const t=document.getElementById('toast');
  const icon= type==="success"?"✔":type==="error"?"✖":"ℹ";
  t.innerHTML=`<span>${icon}</span>${msg}`;
  t.style.background= type==="error" ? "#d9534f" : type==="success" ? "#2e8b57" : "rgba(0,0,0,.8)";
  t.className="show";
  setTimeout(()=>t.className="",3000);
}

// ====== Data ======
let customers=JSON.parse(localStorage.getItem('customers'))||[];
let inactive=JSON.parse(localStorage.getItem('inactive'))||[];
let archive=JSON.parse(localStorage.getItem('archive'))||[];
let currentPage={customers:0,inactive:0,archive:0};
const PAGE_SIZE=5;
let searchTerm="";
let sortState={customers:{col:null,asc:true},inactive:{col:null,asc:true},archive:{col:null,asc:true}};

function generateUUID(){return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g,c=>{const r=Math.random()*16|0,v=c==='x'?r:(r&0x3|0x8);return v.toString(16);});}
function formatAmount(a){ const n=parseFloat(a); return isNaN(n)?"":n.toLocaleString('he-IL',{style:'currency',currency:'ILS',minimumFractionDigits:2,maximumFractionDigits:2}); }

// ====== Local Storage ======
function saveLocal(){
  localStorage.setItem('customers',JSON.stringify(customers));
  localStorage.setItem('inactive',JSON.stringify(inactive));
  localStorage.setItem('archive',JSON.stringify(archive));
  updateTitle();
}


// ====== Title ======
function updateTitle(){
  document.getElementById('title').textContent=`ניהול לקוחות זומבים — ${customers.length} לקוחות פעילים`;
}

// ====== Add Customer ======
function addCustomer(){
  const name=document.getElementById('name').value.trim();
  const email=document.getElementById('email').value.trim();
  const mobile=document.getElementById('mobile').value.trim();
  const amount=parseFloat(document.getElementById('amount').value);
  const comment=document.getElementById('comment').value.trim();
  const status=document.getElementById('status').value;
  if(!name||!email||!mobile||isNaN(amount)){ toast("מלא את כל השדות","error"); return; }
  if(customers.some(c=>c.name===name && c.mobile===mobile)){ toast("לקוח קיים","error"); return; }
  customers.push({id:generateUUID(),name,email,mobile,amount,comment,status});
  saveLocal(); renderAll(); toast("לקוח נוסף בהצלחה✔️","success"); resetAddCustomerForm();
}

function resetAddCustomerForm(){
  ["name","email","mobile","amount","comment"].forEach(id => document.getElementById(id).value="");
  document.getElementById('status').value="שילם";
  document.getElementById('name').focus();
  toast("טופס הוספת הלקוח אופס","success");
}

// ====== Highlight & Filter ======
function highlight(text, term){ if(!term) return text; return term.split(" ").filter(w=>w).reduce((acc,w)=>acc.replace(new RegExp(`(${w})`,'gi'), `<span class="highlight">$1</span>`), text); }
function filterData(arr){ 
  if(!searchTerm) return arr;
  const words=searchTerm.split(" ").filter(w=>w);
  return arr.filter(c=>words.every(w=>c.name.toLowerCase().includes(w)||c.email.toLowerCase().includes(w)||c.mobile.toString().toLowerCase().includes(w)||(c.comment||"").toLowerCase().includes(w)||c.status.toLowerCase().includes(w)));
}

// ====== Render Tables ======
function tableIdFromType(type){ return type==="customers"?"customerTable":type==="inactive"?"inactiveTable":"archiveTable"; }

function renderAll(){
  renderTable('customerTable',filterData(customers),'customers',currentPage.customers);
  renderTable('inactiveTable',filterData(inactive),'inactive',currentPage.inactive);
  renderTable('archiveTable',filterData(archive),'archive',currentPage.archive);
  updateTitle();
}

function renderTable(tableId,data,type,page){
  const table=document.getElementById(tableId);
  table.innerHTML="";
  if(!data.length){ table.innerHTML='<tr class="empty"><td colspan="9">אין נתונים</td></tr>'; return; }

  const state=sortState[type];
  if(state.col){ data.sort((a,b)=>{let valA=a[state.col], valB=b[state.col]; if(state.col==="amount"){ valA=parseFloat(valA); valB=parseFloat(valB);} else{ valA=valA.toString(); valB=valB.toString();} if(valA<valB)return state.asc?-1:1;if(valA>valB)return state.asc?1:-1;return 0;});}
  else{ data.sort((a,b)=>a.name.localeCompare(b.name,'he')); }

  const start=page*PAGE_SIZE;
  const pageData=data.slice(start,start+PAGE_SIZE);

  table.innerHTML=`<thead><tr>
    <th><input type="checkbox" onclick="toggleSelectAll(this,'${tableId}')" aria-label="בחר הכל"/></th>
    <th>#</th>
    <th onclick="sortColumn('${type}','name')">שם מלא</th>
    <th onclick="sortColumn('${type}','email')">אימייל</th>
    <th onclick="sortColumn('${type}','mobile')">נייד</th>
    <th onclick="sortColumn('${type}','amount')">סכום</th>
    <th onclick="sortColumn('${type}','comment')">הערה</th>
    <th onclick="sortColumn('${type}','status')">סטטוס</th>
    <th>פעולות</th>
  </tr></thead><tbody></tbody>`;

  const tbody=table.querySelector('tbody');
  pageData.forEach((c,i)=>{
    const abs=start+i;
    const tr=document.createElement('tr');
    tr.setAttribute("data-id", c.id);
    tr.setAttribute("data-status",c.status);
    const statusClass=c.status==="שילם"?"status-paid":c.status==="לא שילם"?"status-unpaid":c.status==="מתנסה"?"status-trial":"status-blocked";
    tr.innerHTML=`<td><input type="checkbox" class="row-checkbox" data-id="${c.id}" data-type="${type}"></td>
      <td>${abs+1}</td>
      <td>${highlight(c.name,searchTerm)}</td>
      <td>${highlight(c.email,searchTerm)}</td>
      <td>${highlight(c.mobile.toString(),searchTerm)}</td>
      <td>${formatAmount(c.amount)}</td>
      <td>${highlight(c.comment||'',searchTerm)}</td>
      <td class="${statusClass}">${c.status}</td>
      <td><div class="actions"></div></td>`;
    const actions=tr.querySelector('.actions');
    if(type==="customers"){ actions.innerHTML=`<button class="edit" onclick="enableInlineEdit('customers','${c.id}')">ערוך✏️</button><button class="archive" onclick="moveToArchive('${c.id}')">ארכיון📦</button>

<button class="delete" onclick="deleteCustomerConfirm('${c.id}')">מחק🗑️</button><button onclick="moveToInactive('${c.id}')">לא פעיל⛔</button>`; }
    if(type==="inactive"){ actions.innerHTML=`<button class="edit" onclick="enableInlineEdit('inactive','${c.id}')">ערוך✏️</button><button class="delete" onclick="deleteInactiveConfirm('${c.id}')">מחק🗑️</button><button onclick="moveToCustomersFromInactive('${c.id}')">חזור לפעילים🔄</button>`; }
    if(type==="archive"){ actions.innerHTML=`<button class="edit" onclick="enableInlineEdit('archive','${c.id}')">ערוך✏️</button><button class="delete" onclick="deleteArchiveConfirm('${c.id}')">מחק🗑️</button><button onclick="moveToCustomersFromArchive('${c.id}')">חזור לפעילים🔄</button>`; }
    tbody.appendChild(tr);
  });

  const paginationDiv=document.getElementById('pagination-'+type);
  paginationDiv.innerHTML="";
  if(data.length>PAGE_SIZE){
    const totalPages=Math.ceil(data.length/PAGE_SIZE);
    for(let i=0;i<totalPages;i++){
      const btn=document.createElement('button');
      btn.textContent=i+1;
      if(i===page){ btn.style.fontWeight="bold"; btn.style.background="#ccc"; }
      btn.onclick=()=>{ currentPage[type]=i; renderAll(); };
      paginationDiv.appendChild(btn);
    }
  }
}

// ====== Sorting ======
function sortColumn(type,col){
  if(sortState[type].col===col) sortState[type].asc=!sortState[type].asc;
  else{ sortState[type]={col:col,asc:true}; }
  renderAll();
}

// ====== Inline Edit ======
function enableInlineEdit(type,id){
  let arr = type==="customers"?customers:type==="inactive"?inactive:archive;
  const idx=arr.findIndex(c=>c.id===id);
  if(idx<0) return;
  const table=document.getElementById(tableIdFromType(type));
  const row=table.querySelector(`tr[data-id="${id}"]`);
  const absIdx = currentPage[type]*PAGE_SIZE + idx;
  const c = arr[idx];
  row.innerHTML = `<td></td><td>${absIdx+1}</td>
    <td><input type="text" value="${c.name}" /></td>
    <td><input type="email" value="${c.email}" /></td>
    <td><input type="text" value="${c.mobile}" /></td>
    <td><input type="number" step="0.01" value="${c.amount}" /></td>
    <td><textarea>${c.comment||""}</textarea></td>
    <td>
      <select>
        <option ${c.status==="שילם"?"selected":""}>שילם</option>
        <option ${c.status==="מתנסה"?"selected":""}>מתנסה</option>
        <option ${c.status==="לא שילם"?"selected":""}>לא שילם</option>
        <option ${c.status==="חסום"?"selected":""}>חסום</option>
      </select>
    </td>
    <td><button class="save" onclick="saveInlineEdit('${type}','${id}')">שמור</button></td>`;
}

function saveInlineEdit(type,id){
  let arr = type==="customers"?customers:type==="inactive"?inactive:archive;
  const idx=arr.findIndex(c=>c.id===id);
  if(idx<0) return;
  const table=document.getElementById(tableIdFromType(type));
  const row=table.querySelector(`tr[data-id="${id}"]`);
  const inputs=row.querySelectorAll('input,textarea,select');
  arr[idx].name=inputs[0].value.trim();
  arr[idx].email=inputs[1].value.trim();
  arr[idx].mobile=inputs[2].value.trim();
  arr[idx].amount=parseFloat(inputs[3].value)||0;
  arr[idx].comment=inputs[4].value.trim();
  arr[idx].status=inputs[5].value;
  saveLocal(); renderAll(); toast("הלקוח עודכן","success");
}

// ====== Move / Archive / Delete ======
function moveToArchive(id){ customers=customers.filter(c=>{if(c.id===id){archive.push(c);} return c.id!==id;}); saveLocal(); renderAll(); }
function moveToInactive(id){ customers=customers.filter(c=>{if(c.id===id){inactive.push(c);} return c.id!==id;}); saveLocal(); renderAll(); }
function moveToCustomersFromInactive(id){ inactive=inactive.filter(c=>{if(c.id===id){customers.push(c);} return c.id!==id;}); saveLocal(); renderAll(); }
function moveToCustomersFromArchive(id){ archive=archive.filter(c=>{if(c.id===id){customers.push(c);} return c.id!==id;}); saveLocal(); renderAll(); }

function deleteCustomerConfirm(id){ if(prompt("סיסמא למחיקה")===DELETE_PASSWORD){ deleteCustomer(id); toast("הלקוח נמחק","success"); } }
function deleteInactiveConfirm(id){ if(prompt("סיסמא למחיקה")===DELETE_PASSWORD){ deleteInactive(id); toast("הלקוח נמחק","success"); } }
function deleteArchiveConfirm(id){ if(prompt("סיסמא למחיקה")===DELETE_PASSWORD){ deleteArchive(id); toast("הלקוח נמחק","success"); } }

function deleteCustomer(id){ customers=customers.filter(c=>c.id!==id); saveLocal(); renderAll(); }
function deleteInactive(id){ inactive=inactive.filter(c=>c.id!==id); saveLocal(); renderAll(); }
function deleteArchive(id){ archive=archive.filter(c=>c.id!==id); saveLocal(); renderAll(); }

function archiveAllCustomers(){ if(confirm("לארכיון כל הלקוחות?")){ archive.push(...customers); customers=[]; saveLocal(); renderAll(); toast("כל הלקוחות הועברו לארכיון","success"); } }





// ====== Search ======
function manualSearch() {
  const input = document.getElementById('searchInput').value.trim().toLowerCase();

  if(!input){ 
    toast("אנא הכנס טקסט לחיפוש", "error"); 
    return; 
  }

  searchTerm = input;
  currentPage = {customers:0, inactive:0, archive:0};

  // סינון נתונים
  const filteredCustomers = filterData(customers);
  const filteredInactive = filterData(inactive);
  const filteredArchive = filterData(archive);

  if(filteredCustomers.length === 0 && filteredInactive.length === 0 && filteredArchive.length === 0) {
    toast("לא נמצא לקוח במערכת", "error");

    ['customerTable', 'inactiveTable', 'archiveTable'].forEach(id => {
      document.getElementById(id).innerHTML = '';
      document.getElementById(id).style.background="#fde0e0"; 
    });

    ['pagination-customers','pagination-inactive','pagination-archive'].forEach(id => {
      document.getElementById(id).innerHTML = '';
    });

    return;
  }

  ['customerTable', 'inactiveTable', 'archiveTable'].forEach(id => {
    document.getElementById(id).style.background="";
  });

  renderAll();
}




// ====== Reset Search ======
function resetSearch() {
  document.getElementById('searchInput').value = "";
  searchTerm = "";

  // החזר את כל הנתונים לטבלאות
  renderAll();
}
// ====== Select All ======
function toggleSelectAll(chk,tableId){
  const checked=chk.checked;
  const boxes=document.querySelectorAll(`#${tableId} .row-checkbox`);
  boxes.forEach(b=>b.checked=checked);
}




// ====== Delete Selected ======
function deleteSelectedCustomers(){
  const selected=document.querySelectorAll('.row-checkbox:checked');
  if(selected.length===0){ toast("בחר לפחות לקוח אחד","error"); return; }
  if(prompt("סיסמה למחיקה")!==DELETE_PASSWORD){ toast("סיסמה שגויה","error"); return; }
  selected.forEach(chk=>{
    const id=chk.dataset.id, type=chk.dataset.type;
    if(type==="customers") deleteCustomer(id);
    else if(type==="inactive") deleteInactive(id);
    else if(type==="archive") deleteArchive(id);
  });
  toast("הלקוחות שנבחרו נמחקו","success");
}

</script>
</body>
</html>
