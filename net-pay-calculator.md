# 意大利到手工资计算器

> 2026年意大利税前到实发：员工社保、含工资收入抵免的个税（IRPEF）、大区和市级附加税，覆盖米兰、罗马、都灵、博洛尼亚和那不勒斯。


<section> <span class="eyebrow">到手工资计算器</span> <h1 id="what-your-employee-takes-home-in-italy">员工在意大利到手多少</h1> <p class="lede">输入税前年薪和城市。估算按2026年规则：员工社保、含工资收入抵免的个税和地方附加税。</p> </section> <section> <div class="grid"> <form class="panel" id="nf" style="display:grid;gap:16px"> <label for="g">税前年薪 RAL（欧元）<input id="g" type="number" min="10000" step="1000" value="50000"></label> <label for="city">城市<select id="city"><option value="Milan">米兰</option><option value="Rome">罗马</option><option value="Turin">都灵</option><option value="Bologna">博洛尼亚</option><option value="Naples">那不勒斯</option></select></label> <label for="inst">发放期数<select id="inst"><option value="13">13期</option><option value="14">14期</option><option value="12">12期（第13/14薪按月分摊）</option></select></label> </form> <div class="panel" style="min-width:0"><div class="tbl"><table id="nt"><thead><tr><th>项目</th><th class="num">每年</th></tr></thead><tbody></tbody></table></div> <p id="nm" style="margin-bottom:0;font-weight:600"></p></div> </div>  </section> <section class="prose"> <h2 id="how-the-calculation-works">计算方法</h2> <ol> <li>员工按税前工资缴9.19%社保，年收入超过€56,224的部分另加1%。</li> <li>剩余部分计征个税（IRPEF）：€28,000以内23%，€28,000至€50,000为33%，以上43%。</li> <li>工资收入税收抵免降低税额，低工资抵免最多，到€50,000降为零。€20,000至€40,000之间另有最多€1,000的额外抵免。</li> <li>加上大区和市级附加税；收入低于城市免征额的除外。</li> </ol> <p>以上都不影响雇主支出：见<a href="calculator.html">雇主成本计算器</a>。</p> </section> <script>(function(){const N = {"status":"secondary","source":"data/params/2026.yaml (irpef, inps, addizionali); TUIR art. 11 and 13; L. 207/2024; L. 199/2025","employee_inps":0.0919,"extra_1pct_above":56224,"irpef":[[28000,0.23],[50000,0.33],[null,0.43]],"cities":{"Milan":{"region":"伦巴第","regional":[[null,0.0123]],"municipal":0.008,"exempt":23000},"Rome":{"region":"拉齐奥","regional":[[28000,0.0173],[null,0.0333]],"municipal":0.009,"exempt":14000},"Turin":{"region":"皮埃蒙特","regional":[[null,0.0162]],"municipal":0.008,"municipal_note":"0.8% to 1.2% by band; 0.8% used","exempt":11790},"Bologna":{"region":"艾米利亚-罗马涅","regional":[[null,0.0133]],"municipal":0.008,"exempt":15000},"Naples":{"region":"坎帕尼亚","regional":[[null,0.0173]],"municipal":0.01,"exempt":12000}}};

const eur = n => (n < 0 ? '−€' : '€') + Math.round(Math.abs(n)).toLocaleString('en-US');
function bands(x, b){ let t = 0, lo = 0; for (const [hi, r] of b){ const top = hi ?? Infinity; if (x > lo) t += (Math.min(x, top) - lo) * r; lo = top; } return t; }
function credit(r){ if (r <= 15000) return 1955; if (r <= 28000) return 1910 + 1190 * (28000 - r) / 13000; if (r <= 50000) return 1910 * (50000 - r) / 22000 + (r > 25000 && r <= 35000 ? 65 : 0); return 0; }
function extra(r){ if (r > 20000 && r <= 32000) return 1000; if (r > 32000 && r < 40000) return 1000 * (40000 - r) / 8000; return 0; }
function calc(){
  const g = +document.getElementById('g').value || 0, c = N.cities[document.getElementById('city').value], n = +document.getElementById('inst').value;
  const inps = g * N.employee_inps + Math.max(0, g - N.extra_1pct_above) * 0.01;
  const tx = g - inps;
  const irpef = Math.max(0, bands(tx, N.irpef) - credit(tx) - extra(tx));
  const reg = bands(tx, c.regional), mun = tx > c.exempt ? tx * c.municipal : 0;
  const net = g - inps - irpef - reg - mun;
  const rows = [['税前工资', g], ['员工社保', -inps], ['抵免后个税（IRPEF）', -irpef], [`大区附加税（${c.region}）`, -reg], ['市级附加税', -mun]];
  document.querySelector('#nt tbody').innerHTML = rows.map(([k, v]) => `<tr><td>${k}</td><td class="num">${eur(v)}</td></tr>`).join('') + `<tr><td><b>到手工资</b></td><td class="num"><b>${eur(net)}</b></td></tr>`;
  document.getElementById('nm').textContent = `${n}期工资单中每期到手约${eur(net / n)}（税前的${(net / g * 100).toFixed(0)}%）。`;
}
document.getElementById('nf').addEventListener('input', calc); document.getElementById('nf').addEventListener('submit', e => e.preventDefault()); calc();
})();</script>


---
Source: https://yipinitaly.com/net-pay-calculator.html
