# 获取报价

> 申请在意大利雇佣员工的报价。三个简短步骤，一个工作日内答复。


<section> <span class="eyebrow">获取报价</span> <h1 id="tell-us-who-you-want-to-hire">告诉我们您想雇谁</h1> <p class="lede">三个简短步骤。我们在一个工作日内回复完整成本和合同要点。</p> </section> <section> <form id="lead" class="panel" style="display:grid;gap:18px;max-width:640px" novalidate> <p id="stepinfo" class="tag" style="justify-self:start">第1步，共3步</p> <fieldset data-step="1" style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="email">工作邮箱<input id="email" type="email" autocomplete="email" required></label> <label for="company">公司名称<input id="company" autocomplete="organization"></label> </fieldset> <fieldset data-step="2" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="service">您需要什么服务？<select id="service"><option>名义雇主（EOR）</option><option>工资代发（已有意大利公司）</option><option>工作签证 / 蓝卡</option><option>招聘</option></select></label> <label for="count">在意大利需要几个人？<input id="count" type="number" min="1" value="1"></label> <label for="city">城市或地区<input id="city" placeholder="米兰、罗马、远程……"></label> <label for="salary">计划税前年薪（欧元）<input id="salary" type="number" step="1000"></label> </fieldset> <fieldset data-step="3" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="name">您的姓名<input id="name" autocomplete="name"></label> <label for="start">预计入职日期<input id="start" type="date"></label> <label for="notes">其他说明（如是否需要为中国籍员工办签证）<textarea id="notes" rows="4"></textarea></label> <label style="display:flex;gap:10px;align-items:flex-start;font-weight:400"><input id="consent" type="checkbox" style="width:auto;margin-top:5px">我同意意聘为答复本次咨询处理我的数据（隐私政策）。</label> </fieldset> <p id="msg" role="status" style="margin:0;color:var(--accent)"></p> <div class="cta"><button type="button" class="btn ghost" id="back" hidden>上一步</button><button type="submit" class="btn" id="next">继续</button></div> </form> </section> <script>
// Il passo 1 salva subito l'email (lead abbandonato recuperabile dal nurturing). Endpoint del worker: /api/lead.
const f = document.getElementById('lead'); let step = 1;
const sets = [...f.querySelectorAll('fieldset')];
const data = () => Object.fromEntries([...f.querySelectorAll('input,select,textarea')].map(e => [e.id, e.type === 'checkbox' ? e.checked : e.value]));
async function save(final){ try { await fetch('/api/lead', {method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify({...data(), step, final, brand:'yidalieor'})}); } catch(e){} }
function show(){ sets.forEach(s => s.hidden = +s.dataset.step !== step);
  document.getElementById('stepinfo').textContent = `第${step}步，共3步`;
  document.getElementById('back').hidden = step === 1;
  document.getElementById('next').textContent = step === 3 ? '发送' : '继续'; }
// Ripresa dal link nelle email: ?resume=<token> precompila i campi e salta al passo successivo.
(async () => { const tok = new URLSearchParams(location.search).get('resume'); if (!tok) return;
  try { const r = await fetch('/api/lead/' + encodeURIComponent(tok)); if (!r.ok) return; const d = await r.json();
    for (const [k, v] of Object.entries(d)) { const el = document.getElementById(k); if (el && v != null) el.value = v; }
    step = d.step || 2; show(); } catch(e){} })();
document.getElementById('back').onclick = () => { step--; show(); };
f.addEventListener('submit', e => { e.preventDefault(); const m = document.getElementById('msg'); m.textContent = '';
  if (step === 1 && !document.getElementById('email').checkValidity()) { m.textContent = '请输入有效的工作邮箱。'; return; }
  if (step === 3 && !document.getElementById('consent').checked) { m.textContent = '请勾选同意，以便我们回复。'; return; }
  save(step === 3);
  if (step < 3) { step++; show(); } else { f.querySelectorAll('fieldset,.cta').forEach(x => x.hidden = true); m.textContent = '谢谢！我们将在一个工作日内回复。'; } });
</script>

<script>(function(){var sj=new URLSearchParams(location.search).get('subject');var n=document.getElementById('notes');if(sj&&n&&!n.value)n.value='['+sj.slice(0,40)+'] ';})();</script>


---
Source: https://yipinitaly.com/contact.html
