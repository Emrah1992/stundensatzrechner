# Stundensatzrechner
Der Freelancer-Stundensatz-Rechner ist ein gesichtsloses, vollständig im Browser laufendes Web-Tool zur Kalkulation realistischer Honorare für Selbstständige. Nutzer geben gewünschtes Nettoeinkommen, Fixkosten, Arbeitszeit, Auslastung sowie Steuerquote ein und erhalten sofort Mindest- und Empfehlungspreise, Tagessatz und Umsatzziel. 
<!doctype html>
<html lang="de">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Freelancer Stundensatz Rechner</title>
  <style>
    :root { --bg:#0b1020; --card:#121a33; --muted:#9aa6c2; --text:#e8ecff; --acc:#7aa2ff; --bad:#ff6b6b; --ok:#2ee59d; }
    body{ margin:0; font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial;
      background: radial-gradient(1200px 600px at 30% 10%, #18214a 0%, var(--bg) 60%);
      color:var(--text); }
    .wrap{ max-width:1100px; margin:0 auto; padding:28px 16px 60px;}
    header{ display:flex; gap:16px; align-items:flex-start; justify-content:space-between; flex-wrap:wrap; }
    h1{ font-size:28px; margin:0; letter-spacing:.2px;}
    .sub{ color:var(--muted); margin:8px 0 0; line-height:1.4; max-width:720px;}
    .grid{ display:grid; grid-template-columns: 1.1fr .9fr; gap:16px; margin-top:18px; }
    @media (max-width: 900px){ .grid{ grid-template-columns:1fr; } }
    .card{ background: linear-gradient(180deg, rgba(255,255,255,.05), rgba(255,255,255,.02));
      border:1px solid rgba(255,255,255,.08); border-radius:16px; padding:16px; }
    .card h2{ font-size:14px; text-transform:uppercase; letter-spacing:.12em; color:var(--muted); margin:0 0 12px;}
    .row{ display:grid; grid-template-columns: 1fr 1fr; gap:10px; }
    @media (max-width: 520px){ .row{ grid-template-columns:1fr; } }
    label{ display:block; font-size:13px; color:var(--muted); margin:10px 0 6px;}
    input, select{ width:100%; padding:10px 11px; border-radius:12px; border:1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.18); color:var(--text); outline:none; }
    input:focus, select:focus{ border-color: rgba(122,162,255,.65); box-shadow: 0 0 0 3px rgba(122,162,255,.15); }
    .actions{ display:flex; gap:10px; flex-wrap:wrap; margin-top:12px; }
    button{ padding:10px 12px; border-radius:12px; border:1px solid rgba(255,255,255,.14);
      background: rgba(122,162,255,.22); color:var(--text); cursor:pointer; font-weight:600; }
    button.secondary{ background: rgba(255,255,255,.06); }
    button:active{ transform: translateY(1px); }
    .kpi{ display:grid; grid-template-columns: 1fr 1fr; gap:10px; margin-top:10px; }
    @media (max-width: 520px){ .kpi{ grid-template-columns:1fr; } }
    .box{ padding:12px; border-radius:14px; border:1px solid rgba(255,255,255,.08); background: rgba(0,0,0,.16); }
    .box .t{ color:var(--muted); font-size:12px; text-transform:uppercase; letter-spacing:.08em;}
    .box .v{ font-size:22px; margin-top:6px; font-weight:800;}
    .note{ margin-top:12px; color:var(--muted); font-size:13px; line-height:1.45;}
    .warn{ color: #ffd38b; }
    .pill{ display:inline-block; padding:6px 10px; border-radius:999px; border:1px solid rgba(255,255,255,.14);
      background: rgba(0,0,0,.12); color:var(--muted); font-size:12px; }
    .split{ display:flex; gap:10px; flex-wrap:wrap; align-items:center; justify-content:space-between;}
    .hr{ height:1px; background: rgba(255,255,255,.09); margin:12px 0; }
    .list{ margin:0; padding-left:18px; color:var(--muted); }
    a{ color:var(--acc); text-decoration:none;}
    a:hover{text-decoration:underline;}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div>
        <h1>Freelancer Stundensatz Rechner</h1>
        <p class="sub">Berechnet deinen <b>Mindest-</b> und <b>Empfehlungs-</b>Stundensatz basierend auf Wunsch-Netto, Fixkosten, Steuern, Arbeitszeit, Urlaub & Krankheit. Läuft komplett im Browser (kein Tracking, keine Accounts).</p>
      </div>
      <span class="pill" id="statusPill">Bereit</span>
    </header>

    <div class="grid">
      <section class="card">
        <h2>Eingaben</h2>

        <div class="row">
          <div>
            <label>Land / Kontext</label>
            <select id="country">
              <option value="AT">Österreich (AT)</option>
              <option value="DE">Deutschland (DE)</option>
              <option value="CH">Schweiz (CH)</option>
              <option value="INT">International</option>
            </select>
          </div>
          <div>
            <label>Währung</label>
            <select id="currency">
              <option value="EUR">EUR €</option>
              <option value="CHF">CHF</option>
              <option value="USD">USD $</option>
            </select>
          </div>
        </div>

        <div class="row">
          <div>
            <label>Wunsch-Netto pro Monat</label>
            <input id="netMonthly" type="number" min="0" step="50" value="3000">
          </div>
          <div>
            <label>Fixkosten pro Monat (Software, Miete, etc.)</label>
            <input id="fixedMonthly" type="number" min="0" step="50" value="400">
          </div>
        </div>

        <div class="row">
          <div>
            <label>Arbeitsstunden pro Woche (geplant)</label>
            <input id="hoursPerWeek" type="number" min="1" step="1" value="35">
          </div>
          <div>
            <label>Auslastung (billable Anteil) %</label>
            <input id="utilization" type="number" min="10" max="100" step="1" value="65">
          </div>
        </div>

        <div class="row">
          <div>
            <label>Urlaubstage pro Jahr</label>
            <input id="vacDays" type="number" min="0" step="1" value="25">
          </div>
          <div>
            <label>Kranktage pro Jahr</label>
            <input id="sickDays" type="number" min="0" step="1" value="8">
          </div>
        </div>

        <div class="row">
          <div>
            <label>Feiertage (Arbeitstage) pro Jahr</label>
            <input id="holDays" type="number" min="0" step="1" value="10">
          </div>
          <div>
            <label>Steuer- & Abgabenquote % (effektiv)</label>
            <input id="taxRate" type="number" min="0" max="70" step="0.5" value="35">
          </div>
        </div>

        <div class="row">
          <div>
            <label>Sicherheitsfaktor (Risiko/Reserve)</label>
            <input id="safety" type="number" min="1" max="2" step="0.05" value="1.30">
          </div>
          <div>
            <label>Rundung</label>
            <select id="rounding">
              <option value="1">auf 1</option>
              <option value="5" selected>auf 5</option>
              <option value="10">auf 10</option>
            </select>
          </div>
        </div>

        <div class="actions">
          <button id="calcBtn">Berechnen</button>
          <button class="secondary" id="resetBtn">Reset</button>
          <button class="secondary" id="shareBtn">Link mit Werten</button>
        </div>

        <p class="note">
          Tipp: <span class="warn">Auslastung</span> ist der Killer. 35h/Woche arbeiten heißt nicht 35h/Woche abrechnen.
          Gute Annahmen: 50–75% (je nach Sales/Meetings/Orga).
        </p>
      </section>

      <aside class="card">
        <div class="split">
          <h2>Ergebnisse</h2>
          <span class="pill" id="calcMeta">—</span>
        </div>

        <div class="kpi">
          <div class="box">
            <div class="t">Mindest-Stundensatz</div>
            <div class="v" id="minHourly">—</div>
          </div>
          <div class="box">
            <div class="t">Empfehlung (mit Reserve)</div>
            <div class="v" id="recHourly">—</div>
          </div>
          <div class="box">
            <div class="t">Tagessatz (8h)</div>
            <div class="v" id="dayRate">—</div>
          </div>
          <div class="box">
            <div class="t">Monats-Umsatz Ziel (brutto)</div>
            <div class="v" id="grossMonthly">—</div>
          </div>
        </div>

        <div class="hr"></div>

        <div class="box">
          <div class="t">Arbeitszeit-Modell</div>
          <div style="margin-top:8px; color:var(--muted); line-height:1.55;">
            <div>Effektive abrechenbare Stunden/Jahr: <b id="billableYear">—</b></div>
            <div>Arbeitsstunden/Jahr (geplant): <b id="workHoursYear">—</b></div>
            <div>Freie Tage (Urlaub/Krank/Feiertag): <b id="daysOff">—</b></div>
          </div>
        </div>

        <div class="hr"></div>

        <div class="note">
          <b>Interpretation:</b>
          <ul class="list">
            <li><b>Mindest</b>: deckt Netto + Fixkosten + Steuern bei deiner Auslastung.</li>
            <li><b>Empfehlung</b>: Mindest × Sicherheitsfaktor (für Leerlauf, Kundenrisiko, Investitionen).</li>
            <li>Wenn du merkst, dass du unter dem Markt bist: erhöhe Reserve oder senke „billable“ nicht künstlich.</li>
          </ul>
        </div>

        <div class="actions">
          <button class="secondary" id="copyBtn">Ergebnisse kopieren</button>
        </div>
      </aside>
    </div>
  </div>

<script>
  const $ = (id) => document.getElementById(id);

  function fmtMoney(value, cur){
    if (!isFinite(value)) return "—";
    try {
      return new Intl.NumberFormat('de-AT', { style:'currency', currency:cur, maximumFractionDigits:0 }).format(value);
    } catch {
      return Math.round(value).toString() + " " + cur;
    }
  }
  function roundTo(x, step){
    step = Number(step) || 1;
    return Math.round(x/step)*step;
  }
  function clamp(n, a, b){ return Math.min(Math.max(n, a), b); }

  function getInputs(){
    return {
      country: $("country").value,
      currency: $("currency").value,
      netMonthly: Number($("netMonthly").value || 0),
      fixedMonthly: Number($("fixedMonthly").value || 0),
      hoursPerWeek: Number($("hoursPerWeek").value || 0),
      utilization: clamp(Number($("utilization").value || 0)/100, 0.1, 1),
      vacDays: Number($("vacDays").value || 0),
      sickDays: Number($("sickDays").value || 0),
      holDays: Number($("holDays").value || 0),
      taxRate: clamp(Number($("taxRate").value || 0)/100, 0, 0.7),
      safety: clamp(Number($("safety").value || 1.3), 1, 2),
      rounding: Number($("rounding").value || 5),
    };
  }

  function compute(i){
    const weeksPerYear = 52;
    const workDaysPerWeek = 5;
    const hoursPerDay = i.hoursPerWeek / workDaysPerWeek;

    const plannedWorkHoursYear = i.hoursPerWeek * weeksPerYear;

    const daysOff = i.vacDays + i.sickDays + i.holDays;
    const hoursOff = daysOff * hoursPerDay;

    const effectiveWorkHoursYear = Math.max(plannedWorkHoursYear - hoursOff, 1);

    // billable hours based on utilization
    const billableHoursYear = Math.max(effectiveWorkHoursYear * i.utilization, 1);

    // annual net target + annual fixed costs
    const annualNet = i.netMonthly * 12;
    const annualFixed = i.fixedMonthly * 12;

    // We need gross profit before tax such that after tax we have net+fixed.
    // If taxRate is effective tax on profit, required profit before tax = (net+fixed)/(1-taxRate)
    const requiredPreTax = (annualNet + annualFixed) / Math.max(1 - i.taxRate, 0.0001);

    const minHourly = requiredPreTax / billableHoursYear;
    const recHourly = minHourly * i.safety;

    const minHourlyR = roundTo(minHourly, i.rounding);
    const recHourlyR = roundTo(recHourly, i.rounding);

    const dayRate = recHourlyR * 8;

    const grossMonthlyTarget = requiredPreTax / 12;

    return {
      plannedWorkHoursYear,
      effectiveWorkHoursYear,
      billableHoursYear,
      daysOff,
      minHourly: minHourlyR,
      recHourly: recHourlyR,
      dayRate: roundTo(dayRate, i.rounding),
      grossMonthlyTarget: roundTo(grossMonthlyTarget, i.rounding),
    };
  }

  function render(){
    const i = getInputs();

    // basic validation for status pill
    const ok = i.netMonthly >= 0 && i.hoursPerWeek > 0 && i.utilization > 0;
    $("statusPill").textContent = ok ? "Berechnet" : "Bitte Eingaben prüfen";

    const r = compute(i);

    $("minHourly").textContent = fmtMoney(r.minHourly, i.currency);
    $("recHourly").textContent = fmtMoney(r.recHourly, i.currency);
    $("dayRate").textContent = fmtMoney(r.dayRate, i.currency);
    $("grossMonthly").textContent = fmtMoney(r.grossMonthlyTarget, i.currency);

    $("billableYear").textContent = Math.round(r.billableHoursYear).toLocaleString('de-AT') + " h";
    $("workHoursYear").textContent = Math.round(r.plannedWorkHoursYear).toLocaleString('de-AT') + " h";
    $("daysOff").textContent = Math.round(r.daysOff).toLocaleString('de-AT') + " Tage";

    $("calcMeta").textContent = Auslastung ${Math.round(i.utilization*100)}% · Steuer ${Math.round(i.taxRate*100)}% · Reserve ×${i.safety.toFixed(2)};
  }

  function reset(){
    $("country").value="AT";
    $("currency").value="EUR";
    $("netMonthly").value=3000;
    $("fixedMonthly").value=400;
    $("hoursPerWeek").value=35;
    $("utilization").value=65;
    $("vacDays").value=25;
    $("sickDays").value=8;
    $("holDays").value=10;
    $("taxRate").value=35;
    $("safety").value=1.30;
    $("rounding").value=5;
    render();
  }

  function copyResults(){
    const i = getInputs();
    const r = compute(i);
    const text =
`Freelancer Stundensatz Rechner (${i.country})
Währung: ${i.currency}
Mindest-Stundensatz: ${fmtMoney(r.minHourly, i.currency)}
Empfehlung (Reserve ×${i.safety.toFixed(2)}): ${fmtMoney(r.recHourly, i.currency)}
Tagessatz (8h): ${fmtMoney(r.dayRate, i.currency)}
Monats-Umsatz Ziel (brutto): ${fmtMoney(r.grossMonthlyTarget, i.currency)}
Abrechenbare Stunden/Jahr: ${Math.round(r.billableHoursYear)} h
Arbeitsstunden/Jahr: ${Math.round(r.plannedWorkHoursYear)} h
Freie Tage: ${Math.round(r.daysOff)} Tage
Auslastung: ${Math.round(i.utilization*100)}%
Steuerquote: ${Math.round(i.taxRate*100)}%`;
    navigator.clipboard.writeText(text).then(()=>{
      $("statusPill").textContent="Kopiert ✓";
      setTimeout(()=> $("statusPill").textContent="Berechnet", 1200);
    });
  }

  function toQuery(){
    const i = getInputs();
    const params = new URLSearchParams({
      c:i.country, cur:i.currency,
      net:i.netMonthly, fix:i.fixedMonthly,
      hpw:i.hoursPerWeek, u:Math.round(i.utilization*100),
      vac:i.vacDays, sick:i.sickDays, hol:i.holDays,
      tax:Math.round(i.taxRate*1000)/10,
      s:i.safety, r:i.rounding
    });
    return params.toString();
  }

  function fromQuery(){
    const p = new URLSearchParams(location.search);
    if(!p.has("net")) return;
    if(p.get("c")) $("country").value = p.get("c");
    if(p.get("cur")) $("currency").value = p.get("cur");
    const map = [
      ["netMonthly","net"],["fixedMonthly","fix"],["hoursPerWeek","hpw"],
      ["utilization","u"],["vacDays","vac"],["sickDays","sick"],["holDays","hol"],
      ["taxRate","tax"],["safety","s"],["rounding","r"]
    ];
    for (const [id,key] of map){
      if(!p.has(key)) continue;
      let v = p.get(key);
      if(id==="taxRate"){ v = Number(v); $("taxRate").value = v; }
      else $(""+id).value = v;
    }
    render();
  }

  $("calcBtn").addEventListener("click", render);
  $("resetBtn").addEventListener("click", reset);
  $("copyBtn").addEventListener("click", copyResults);
  $("shareBtn").addEventListener("click", ()=>{
    const url = location.origin + location.pathname + "?" + toQuery();
    navigator.clipboard.writeText(url).then(()=>{
      $("statusPill").textContent="Link kopiert ✓";
      setTimeout(()=> $("statusPill").textContent="Berechnet", 1200);
    });
  });

  // instant calc on change
  for (const id of ["country","currency","netMonthly","fixedMonthly","hoursPerWeek","utilization","vacDays","sickDays","holDays","taxRate","safety","rounding"]){
    $(id).addEventListener("input", render);
    $(id).addEventListener("change", render);
  }

  fromQuery();
  render();
</script>
</body>
</html>
