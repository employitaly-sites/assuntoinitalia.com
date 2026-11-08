# Calcolatore netto

> Calcola lo stipendio netto in Italia nel 2026 dalla RAL: contributi INPS, IRPEF con detrazioni e cuneo fiscale, addizionali regionali e comunali a Milano, Roma, Torino, Bologna e Napoli.


<section> <span class="eyebrow">Calcolatore netto</span> <h1 id="quanto-ti-resta-in-busta-paga">Quanto ti resta in busta paga</h1> <p class="lede">Inserisci la RAL dell'offerta e la città in cui vivi. La stima applica le regole 2026: contributi INPS, IRPEF con detrazioni per lavoro dipendente e addizionali locali.</p> </section> <section> <div class="grid"> <form class="panel" id="nf" style="display:grid;gap:16px"> <label for="g">Retribuzione annua lorda, RAL (EUR)<input id="g" type="number" min="10000" step="1000" value="50000"></label> <label for="city">Città<select id="city"><option value="Milan">Milano</option><option value="Rome">Roma</option><option value="Turin">Torino</option><option value="Bologna">Bologna</option><option value="Naples">Napoli</option></select></label> <label for="inst">Pagata in<select id="inst"><option value="13">13 mensilità</option><option value="14">14 mensilità</option><option value="12">12 (13a/14a pro-quota)</option></select></label> </form> <div class="panel" style="min-width:0"><div class="tbl"><table id="nt"><thead><tr><th>Voce</th><th class="num">All'anno</th></tr></thead><tbody></tbody></table></div> <p id="nm" style="margin-bottom:0;font-weight:600"></p></div> </div>  </section> <section class="prose"> <h2 id="come-funziona-il-calcolo">Come funziona il calcolo</h2> <ol> <li>Paghi il 9,19% di contributi INPS sulla retribuzione lorda, più l'1% sulla parte oltre €56.224 l'anno.</li> <li>Sul resto si applica l'IRPEF: 23% fino a €28.000, 33% da €28.000 a €50.000, 43% oltre.</li> <li>Le detrazioni per lavoro dipendente riducono l'imposta: sono più alte sui redditi bassi e si azzerano a €50.000. Tra €20.000 e €40.000 si aggiunge una detrazione ulteriore fino a €1.000.</li> <li>Si sommano le addizionali regionale e comunale, salvo che il reddito sia sotto la soglia di esenzione del comune.</li> </ol> <p>Il netto non cambia con l'EOR: è lo stesso di un dipendente di un'azienda italiana con la stessa RAL. Vuoi capire le voci? Leggi <a href="busta-paga-e-diritti.html">busta paga e diritti</a>.</p> </section> <script>(function(){const N = {"status":"secondary","source":"data/params/2026.yaml (irpef, inps, addizionali); TUIR art. 11 e 13; L. 207/2024; L. 199/2025","employee_inps":0.0919,"extra_1pct_above":56224,"irpef":[[28000,0.23],[50000,0.33],[null,0.43]],"cities":{"Milan":{"region":"Lombardia","regional":[[null,0.0123]],"municipal":0.008,"exempt":23000},"Rome":{"region":"Lazio","regional":[[28000,0.0173],[null,0.0333]],"municipal":0.009,"exempt":14000},"Turin":{"region":"Piemonte","regional":[[null,0.0162]],"municipal":0.008,"municipal_note":"da 0,8% a 1,2% per scaglioni; usato 0,8%","exempt":11790},"Bologna":{"region":"Emilia-Romagna","regional":[[null,0.0133]],"municipal":0.008,"exempt":15000},"Naples":{"region":"Campania","regional":[[null,0.0173]],"municipal":0.01,"exempt":12000}}};

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
  const rows = [['Retribuzione lorda (RAL)', g], ['Contributi INPS a tuo carico', -inps], ['IRPEF dopo le detrazioni', -irpef], [`Addizionale regionale (${c.region})`, -reg], ['Addizionale comunale', -mun]];
  document.querySelector('#nt tbody').innerHTML = rows.map(([k, v]) => `<tr><td>${k}</td><td class="num">${eur(v)}</td></tr>`).join('') + `<tr><td><b>Netto annuo</b></td><td class="num"><b>${eur(net)}</b></td></tr>`;
  document.getElementById('nm').textContent = `Circa ${eur(net / n)} netti in ciascuna delle ${n} buste paga (${(net / g * 100).toFixed(0)}% del lordo).`;
}
document.getElementById('nf').addEventListener('input', calc); document.getElementById('nf').addEventListener('submit', e => e.preventDefault()); calc();
})();</script>


---
Source: https://assuntoinitalia.com/calcolatore-netto.html
