<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>SINAG | Solar Inclination Numerical Analysis and Guidance</title>
<style>
:root{--navy:#123047;--teal:#087e8b;--gold:#f4b942;--paper:#f4f7fa;--ink:#1b2b34;--muted:#627580;--line:#d8e1e7;--white:#fff;--red:#b43b42}
*{box-sizing:border-box}body{margin:0;background:var(--paper);color:var(--ink);font:15px/1.45 system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif}
header{background:#071a68;color:white;padding:0;border-bottom:5px solid var(--gold);line-height:0}
header img{display:block;width:100%;height:clamp(180px, 42vw, 560px);object-fit:fill;margin:0 auto}
@media(max-width:520px){header img{height:clamp(150px, 48vw, 260px)}}
main{max-width:1240px;margin:22px auto;padding:0 18px 36px}.notice{background:#fff8df;border:1px solid #f0d889;padding:12px 15px;border-radius:10px;margin-bottom:18px}
.layout{display:grid;grid-template-columns:minmax(300px,390px) minmax(0,1fr);gap:18px;align-items:start}.panel{background:white;border:1px solid var(--line);border-radius:14px;padding:18px;box-shadow:0 3px 12px #1533470a;margin-bottom:18px}
h2{font-size:19px;margin:0 0 12px;color:var(--navy)}h3{font-size:15px;margin:16px 0 8px;color:var(--navy)}p.help,.muted{color:var(--muted);font-size:13px}
label{display:block;font-weight:650;font-size:13px;margin:10px 0 4px}input,select,textarea{width:100%;padding:9px 10px;border:1px solid #bfcdd5;border-radius:7px;background:white;color:var(--ink);font:inherit}input:focus,select:focus,textarea:focus{outline:2px solid #77cbd0;border-color:var(--teal)}
.row{display:grid;grid-template-columns:1fr 1fr;gap:10px}.btns{display:flex;flex-wrap:wrap;gap:8px;margin-top:14px}
button,.filelabel{border:0;border-radius:8px;padding:10px 13px;background:var(--teal);color:white;font-weight:700;cursor:pointer;font:inherit;font-size:13px}button.secondary{background:#e7eef2;color:var(--navy)}button.warn{background:#fff0ee;color:var(--red);border:1px solid #efc7c3}button:hover,.filelabel:hover{filter:brightness(.96)}
.tablewrap{overflow:auto;border:1px solid var(--line);border-radius:8px}table{width:100%;border-collapse:collapse;font-size:13px;min-width:590px}th,td{padding:8px;border-bottom:1px solid var(--line);text-align:left}th{background:#edf4f6;color:var(--navy);position:sticky;top:0}td input{min-width:78px;padding:6px;border-radius:5px}td button{padding:5px 8px}
.kpis{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:10px;margin-bottom:18px}.kpi{background:#fff;border:1px solid var(--line);border-radius:12px;padding:14px}.kpi span{display:block;color:var(--muted);font-size:12px}.kpi strong{display:block;font-size:22px;color:var(--navy);margin-top:5px;overflow-wrap:anywhere}
canvas{width:100%;height:340px;display:block;border:1px solid var(--line);border-radius:8px;background:#fff}.legend{display:flex;flex-wrap:wrap;gap:14px;font-size:12px;color:var(--muted);margin:8px 0}.dot{display:inline-block;width:10px;height:10px;border-radius:50%;margin-right:5px}
.metricgrid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px}.metric{background:#f5f8fa;border-radius:8px;padding:11px}.metric b{display:block;color:var(--navy);font-size:18px}.metric span{font-size:12px;color:var(--muted)}
.status{padding:10px 12px;background:#e8f6f2;border-radius:8px;color:#15594f;font-size:13px;margin-top:12px}.status.error{background:#fff0ee;color:#8e272e}.formula{font-family:ui-monospace,SFMono-Regular,Consolas,monospace;background:#f3f6f8;padding:9px;border-radius:7px;overflow-wrap:anywhere;font-size:12px}
footer{color:var(--muted);font-size:12px;text-align:center;padding:10px}
@media(max-width:900px){.layout{grid-template-columns:1fr}.kpis{grid-template-columns:repeat(2,1fr)}}@media(max-width:520px){.row,.metricgrid{grid-template-columns:1fr}.kpi strong{font-size:18px}}
</style>
</head>
<body>
<header aria-label="SINAG website header"><img src="images/sinag-header.png" alt="SINAG — Solar Inclination Numerical Analysis and Guidance, solar panel and sun logo"></header>
<main>
  <div class="notice"><b>Research integrity:</b> results are only as good as the measurements entered. The included rows are clearly marked <b>SAMPLE DATA — NOT FIELD MEASUREMENTS</b>. Replace them with your own readings before reporting findings. This file runs locally and does not send data to a server.</div>
  <div class="layout">
    <section>
      <div class="panel">
        <h2>1. Study settings</h2>
        <div class="row">
          <div><label for="site">Study location</label><input id="site" value="Bangui, Ilocos Norte"></div>
          <div><label for="date">Measurement date</label><input id="date" type="date"></div>
        </div>
        <div class="row">
          <div><label for="vunit">Voltage unit</label><select id="vunit"><option value="V">Volts (V)</option><option value="mV">Millivolts (mV)</option></select></div>
          <div><label for="iunit">Current unit</label><select id="iunit"><option value="A">Amperes (A)</option><option value="mA">Milliamperes (mA)</option></select></div>
        </div>
        <div class="row">
          <div><label for="fit">Curve-fit model</label><select id="fit"><option value="quadratic">Quadratic regression</option><option value="cosine">Trigonometric cosine model</option></select></div>
          <div><label for="step">Angle search step (°)</label><select id="step"><option>0.1</option><option selected>0.5</option><option>1</option></select></div>
        </div>
        <div class="row">
          <div><label for="minAngle">Minimum tested angle (°)</label><input id="minAngle" type="number" value="0" min="0" max="90" step="1"></div>
          <div><label for="maxAngle">Maximum tested angle (°)</label><input id="maxAngle" type="number" value="90" min="0" max="90" step="1"></div>
        </div>
        <p class="help">Angles are panel tilt from horizontal. Keep all angles within the range actually studied; the model's optimum is not a guarantee of year-round optimum tilt.</p>
        <div class="btns"><button id="recalc">Analyze data</button><button class="secondary" id="sample">Load sample data</button><button class="secondary" id="clear">Clear all rows</button></div>
      </div>
      <div class="panel">
        <h2>2. Measurement data</h2>
        <p class="help">Enter one reading per row. Power is calculated as P = V × I. For repeated trials, add separate rows with the same angle; the graph displays average power for each angle.</p>
        <div class="tablewrap"><table id="dataTable">
          <thead><tr><th>Angle (°)</th><th>Voltage</th><th>Current</th><th>Time / trial</th><th>Power (W)</th><th></th></tr></thead>
          <tbody id="rows"></tbody>
        </table></div>
        <div class="btns"><button id="addRow">+ Add measurement</button><button class="secondary" id="csvTemplate">Download CSV template</button><button class="secondary" id="export">Export results CSV</button><label class="filelabel" for="upload" style="margin:0">Import CSV</label><input id="upload" type="file" accept=".csv,text/csv" hidden></div>
        <p class="help">CSV import accepts headers like angle, voltage, current, trial. Use plain numeric values; the voltage/current units above apply to every row.</p>
      </div>
    </section>
    <section>
      <div class="kpis">
        <div class="kpi"><span>Observed best angle</span><strong id="observed">—</strong></div>
        <div class="kpi"><span>Model-predicted optimum</span><strong id="optimal">—</strong></div>
        <div class="kpi"><span>Maximum measured power</span><strong id="maxPower">—</strong></div>
        <div class="kpi"><span>Valid measurements</span><strong id="nRows">0</strong></div>
      </div>
      <div class="panel">
        <h2>3. Interactive power curve</h2>
        <canvas id="chart" width="1000" height="520" aria-label="Power against panel tilt angle graph"></canvas>
        <div class="legend"><span><i class="dot" style="background:#087e8b"></i>Measured mean power</span><span><i class="dot" style="background:#f0a51b"></i>Fitted model</span><span><i class="dot" style="background:#b43b42"></i>Predicted optimum</span></div>
        <p class="help">Move your pointer over the graph to inspect angle and power. The curve is fitted to measured mean power at each unique angle.</p>
        <div id="graphStatus" class="status">Enter or load data, then choose Analyze data.</div>
      </div>
      <div class="panel">
        <h2>4. Fit quality and validation metrics</h2>
        <div class="metricgrid">
          <div class="metric"><span>Mean absolute error (MAE)</span><b id="mae">—</b><span>Average absolute residual, W</span></div>
          <div class="metric"><span>Root mean square error (RMSE)</span><b id="rmse">—</b><span>Penalizes larger residuals, W</span></div>
          <div class="metric"><span>Coefficient of determination (R²)</span><b id="r2">—</b><span>Fit to angle-mean data</span></div>
        </div>
        <h3>Model and interpretation</h3>
        <div id="modelEquation" class="formula">Model equation will appear here after analysis.</div>
        <p id="interpretation" class="help">R² describes how closely the selected curve fits the observed angle means. It does not by itself prove the model is physically correct.</p>
        <h3>Validation checklist</h3>
        <p class="help">For a fair validation, test the predicted angle in fresh repeated trials under comparable sunlight, then compare measured power at the predicted angle with the model prediction and with nearby angles. Keep time, weather/cloud cover, load, panel, and sensor setup as consistent as possible.</p>
        <div class="row">
          <div><label for="valAngle">Validation angle (°)</label><input id="valAngle" type="number" min="0" max="90" step="0.1" placeholder="e.g. 25"></div>
          <div><label for="valPower">Measured validation power (W)</label><input id="valPower" type="number" min="0" step="any" placeholder="Enter fresh measured mean"></div>
        </div>
        <div class="btns"><button id="validate">Compare validation reading</button></div>
        <div id="validationResult" class="status">Validation comparison not yet entered.</div>
      </div>
      <div class="panel">
        <h2>5. Formula notes</h2>
        <p class="formula">Electrical power: P = V × I</p>
        <p class="formula">Quadratic regression: P(θ) = aθ² + bθ + c</p>
        <p class="formula">Quadratic vertex: θ* = −b / (2a), only a maximum if a &lt; 0</p>
        <p class="formula">Cosine fit: P(θ) = A cos((θ − α)π/180) + C</p>
        <p class="help">The cosine model estimates A, α, and C by a numerical grid search for α and linear least squares for A and C. The quadratic model uses least squares. Optimum search is limited to your selected angle range. If a quadratic has no concave-down vertex in range, the program reports the best point on the fitted curve within the range.</p>
      </div>
      <div class="panel">
        <h2>6. Optimal tilt calculation</h2>
        <p class="help">This section shows how SINAG obtains the model-predicted optimal angle from the entered data. It updates when you select Analyze data.</p>
        <div id="optimalCalculation" class="formula">Enter at least three measurements at three distinct angles, then select Analyze data to display the calculation.</div>
      </div>
    </section>
  </div>
  <footer>SINAG research aid • Offline single-file HTML • Record instrument details and experimental conditions in your research notebook.</footer>
</main>
<script>
(() => {
'use strict';
const $ = id => document.getElementById(id);
const tbody = $('rows');
let fitted = null, grouped = [], graphPoints = [], sampleLoaded = false, hover = null;
const fmt = (x,d=3) => Number.isFinite(x) ? x.toFixed(d) : '—';
const powerFactorV = () => $('vunit').value === 'mV' ? 0.001 : 1;
const powerFactorI = () => $('iunit').value === 'mA' ? 0.001 : 1;
function addRow(d={angle:'',voltage:'',current:'',trial:''}) {
 const tr=document.createElement('tr');
 ['angle','voltage','current','trial'].forEach(k=>{
   const td=document.createElement('td'), input=document.createElement('input');
   input.type=k==='trial'?'text':'number'; input.dataset.field=k;
   if(k!=='trial'){input.step='any';input.min=k==='angle'?'0':'0';if(k==='angle')input.max='90';}
   input.value=d[k]??''; input.setAttribute('aria-label',k); input.addEventListener('input',()=>{updatePowerCells();});
   td.appendChild(input);tr.appendChild(td);
 });
 const p=document.createElement('td');p.className='powerCell';p.textContent='—';tr.appendChild(p);
 const action=document.createElement('td'), del=document.createElement('button');del.textContent='×';del.className='warn';del.title='Remove row';del.addEventListener('click',()=>{tr.remove();updatePowerCells();});action.appendChild(del);tr.appendChild(action);
 tbody.appendChild(tr);updatePowerCells();
}
function readRows(){
 return [...tbody.querySelectorAll('tr')].map(tr=>{
  const v={};tr.querySelectorAll('input').forEach(i=>v[i.dataset.field]=i.value.trim());
  const angle=Number(v.angle), voltage=Number(v.voltage), current=Number(v.current);
  const valid=v.angle!==''&&v.voltage!==''&&v.current!==''&&Number.isFinite(angle)&&Number.isFinite(voltage)&&Number.isFinite(current)&&angle>=0&&angle<=90&&voltage>=0&&current>=0;
  return {...v,angle,voltage,current,power:voltage*powerFactorV()*current*powerFactorI(),valid};
 });
}
function updatePowerCells(){const rows=readRows();[...tbody.querySelectorAll('tr')].forEach((tr,i)=>tr.querySelector('.powerCell').textContent=rows[i]?.valid?fmt(rows[i].power,4):'—');}
function mean(a){return a.length?a.reduce((s,x)=>s+x,0)/a.length:NaN}
function solveLinear(A,b){
 const n=b.length, M=A.map((r,i)=>[...r,b[i]]);
 for(let c=0;c<n;c++){let p=c;for(let r=c+1;r<n;r++)if(Math.abs(M[r][c])>Math.abs(M[p][c]))p=r;
 if(Math.abs(M[p][c])<1e-12)return null;[M[c],M[p]]=[M[p],M[c]];
 const div=M[c][c];for(let j=c;j<=n;j++)M[c][j]/=div;
 for(let r=0;r<n;r++)if(r!==c){const f=M[r][c];for(let j=c;j<=n;j++)M[r][j]-=f*M[c][j];}}
 return M.map(r=>r[n]);
}
function leastSquares(X,y){
 const m=X[0].length, A=Array.from({length:m},()=>Array(m).fill(0)),b=Array(m).fill(0);
 for(let i=0;i<X.length;i++)for(let j=0;j<m;j++){b[j]+=X[i][j]*y[i];for(let k=0;k<m;k++)A[j][k]+=X[i][j]*X[i][k];}
 return solveLinear(A,b);
}
function fitQuadratic(data){
 if(data.length<3)return null;
 const coeff=leastSquares(data.map(d=>[d.x*d.x,d.x,1]),data.map(d=>d.y));
 if(!coeff)return null;const [a,b,c]=coeff;
 return {type:'quadratic',predict:x=>a*x*x+b*x+c,eq:`P(θ) = ${fmt(a,6)}θ² + ${fmt(b,6)}θ + ${fmt(c,6)} (W)`,coeff};
}
function fitCosine(data){
 if(data.length<3)return null;
 let best=null;const min=Number($('minAngle').value),max=Number($('maxAngle').value);
 // Search phase over a full angular cycle. For each phase, solve P=A*cos(theta-alpha)+C.
 for(let alpha=-180;alpha<=180;alpha+=0.5){
  const X=data.map(d=>[Math.cos((d.x-alpha)*Math.PI/180),1]), beta=leastSquares(X,data.map(d=>d.y));
  if(!beta)continue;const [A,C]=beta;
  const pred=x=>A*Math.cos((x-alpha)*Math.PI/180)+C;
  const sse=data.reduce((s,d)=>s+(d.y-pred(d.x))**2,0);
  if(!best||sse<best.sse)best={type:'cosine',A,alpha,C,predict:pred,sse,eq:`P(θ) = ${fmt(A,5)} cos((θ − ${fmt(alpha,2)})π/180) + ${fmt(C,5)} (W)`};
 }
 return best;
}
function calcMetrics(data,model){
 const residuals=data.map(d=>d.y-model.predict(d.x)), mae=mean(residuals.map(Math.abs)), rmse=Math.sqrt(mean(residuals.map(x=>x*x)));
 const ybar=mean(data.map(d=>d.y)), ssTot=data.reduce((s,d)=>s+(d.y-ybar)**2,0),ssRes=residuals.reduce((s,r)=>s+r*r,0);
 return {mae,rmse,r2:ssTot<1e-12?NaN:1-ssRes/ssTot};
}
function findOptimum(model,min,max,step){
 let best={x:min,y:model.predict(min)};
 for(let x=min;x<=max+step/2;x+=step){const xx=Math.min(x,max), y=model.predict(xx);if(Number.isFinite(y)&&y>best.y)best={x:xx,y};}
 const yEnd=model.predict(max);if(yEnd>best.y)best={x:max,y:yEnd};
 return best;
}
function analyze(){
 updatePowerCells();
 const all=readRows(), valid=all.filter(d=>d.valid), min=Number($('minAngle').value),max=Number($('maxAngle').value),step=Number($('step').value);
 if(!Number.isFinite(min)||!Number.isFinite(max)||min<0||max>90||min>=max){setStatus('graphStatus','Angle range must satisfy 0 ≤ minimum < maximum ≤ 90 degrees.','error');return;}
 if(valid.length<3){setStatus('graphStatus','Enter at least 3 valid measurements across at least 3 distinct angles.','error');return;}
 const map=new Map();valid.forEach(d=>{if(!map.has(d.angle))map.set(d.angle,[]);map.get(d.angle).push(d.power)});
 grouped=[...map.entries()].map(([x,ys])=>({x,y:mean(ys),n:ys.length})).sort((a,b)=>a.x-b.x);
 if(grouped.length<3){setStatus('graphStatus','At least 3 distinct tilt angles are needed for curve fitting.','error');return;}
 const model=$('fit').value==='quadratic'?fitQuadratic(grouped):fitCosine(grouped);
 if(!model){setStatus('graphStatus','Could not fit the selected model. Add more varied measurements.','error');return;}
 fitted=model;
 const metrics=calcMetrics(grouped,model),opt=findOptimum(model,min,max,step),observed=grouped.reduce((a,b)=>b.y>a.y?b:a);
 $('observed').textContent=`${fmt(observed.x,1)}°`;
 $('optimal').textContent=`${fmt(opt.x,1)}°`;
 $('maxPower').textContent=`${fmt(observed.y,4)} W`;
 $('nRows').textContent=`${valid.length} / ${all.length}`;
 $('mae').textContent=fmt(metrics.mae,4)+' W';$('rmse').textContent=fmt(metrics.rmse,4)+' W';$('r2').textContent=Number.isFinite(metrics.r2)?fmt(metrics.r2,4):'N/A';
 $('modelEquation').textContent=model.eq;
 const calcBox=$('optimalCalculation');
 if(model.type==='quadratic'){
  const [a,b,c]=model.coeff, vertex=-model.coeff[1]/(2*model.coeff[0]), vertexPower=model.predict(vertex);
  const isMaximum=a<0, vertexInRange=vertex>=min&&vertex<=max;
  const lines=[
   'Step 1 — Quadratic regression equation:',
   `P(θ) = (${fmt(a,8)})θ² + (${fmt(b,8)})θ + (${fmt(c,8)})`,
   '',
   'Step 2 — Calculate the vertex angle:',
   'θ* = −b / (2a)',
   `θ* = −(${fmt(b,8)}) / [2 × (${fmt(a,8)})]`,
   `θ* = ${fmt(vertex,4)}°`,
   '',
   'Step 3 — Check the vertex:',
   `Since a = ${fmt(a,8)}, the parabola ${isMaximum?'opens downward, so its vertex is a maximum':'does not open downward, so its vertex is not a maximum'}.`,
   `Is the vertex within the selected range (${fmt(min,2)}°–${fmt(max,2)}°)? ${vertexInRange?'Yes':'No'}.`
  ];
  if(isMaximum&&vertexInRange){
   lines.push('', 'Step 4 — Substitute the vertex angle to estimate power:',
    `P(${fmt(vertex,4)}°) = (${fmt(a,8)})(${fmt(vertex,4)})² + (${fmt(b,8)})(${fmt(vertex,4)}) + (${fmt(c,8)})`,
    `P(${fmt(vertex,4)}°) ≈ ${fmt(vertexPower,4)} W`);
  }
  lines.push('', `SINAG's reported range-search optimum (step ${fmt(step,2)}°): ${fmt(opt.x,2)}° at ${fmt(opt.y,4)} W.`);
  calcBox.textContent=lines.join('\\n');
 } else {
  const {A,alpha}=model;
  calcBox.textContent=[
   'Step 1 — Fitted cosine model:',
   model.eq,
   '',
   'Step 2 — Cosine peak condition:',
   'P(θ) = A cos((θ − α)π/180) + C',
   A>=0?'For A ≥ 0, the cosine term is largest when (θ − α) = 0° modulo 360°.':'For A < 0, the product is largest when (θ − α) = 180° modulo 360°.',
   `Fitted phase α = ${fmt(alpha,2)}°. The program checks the selected angle range (${fmt(min,2)}°–${fmt(max,2)}°).`,
   '',
   `Step 3 — Range search using ${fmt(step,2)}° increments:`,
   `Predicted optimum = ${fmt(opt.x,2)}°`,
   `Predicted power = ${fmt(opt.y,4)} W`,
   '',
   'The reported angle is the highest fitted value found in the selected range at the chosen search step.'
  ].join('\\n');
 }
 $('interpretation').textContent=`Fit uses ${grouped.length} unique angle means from ${valid.length} valid readings. MAE and RMSE are in watts. R² = ${Number.isFinite(metrics.r2)?fmt(metrics.r2,4):'not defined for constant data'}. These are in-sample fit metrics, not independent validation.`;
 $('graphStatus').className='status';$('graphStatus').textContent=`Analysis complete: ${grouped.length} unique angles, ${valid.length} valid readings. Predicted maximum in selected range: ${fmt(opt.x,2)}° at ${fmt(opt.y,4)} W.`;
 drawChart(min,max,opt);
}
function setStatus(id,msg,cls=''){const e=$(id);e.className='status '+cls;e.textContent=msg;}
function drawChart(min,max,opt){
 const c=$('chart'),ctx=c.getContext('2d'),w=c.width,h=c.height,pad={l:75,r:25,t:25,b:58};
 ctx.clearRect(0,0,w,h);const values=grouped.map(d=>d.y);for(let x=min;x<=max;x+=(max-min)/180)values.push(fitted.predict(x));values.push(opt.y);
 let ymin=Math.min(0,...values),ymax=Math.max(...values);if(ymax-ymin<1e-8)ymax=ymin+1;const margin=(ymax-ymin)*.12;ymax+=margin;ymin=Math.min(0,ymin);
 const X=x=>pad.l+(x-min)/(max-min)*(w-pad.l-pad.r),Y=y=>h-pad.b-(y-ymin)/(ymax-ymin)*(h-pad.t-pad.b);
 ctx.font='13px system-ui';ctx.lineWidth=1;ctx.strokeStyle='#e1e8ec';ctx.fillStyle='#60727c';
 for(let i=0;i<=5;i++){const y=ymin+(ymax-ymin)*i/5,py=Y(y);ctx.beginPath();ctx.moveTo(pad.l,py);ctx.lineTo(w-pad.r,py);ctx.stroke();ctx.textAlign='right';ctx.fillText(y.toFixed(3),pad.l-9,py+4);}
 for(let i=0;i<=6;i++){const x=min+(max-min)*i/6,px=X(x);ctx.beginPath();ctx.moveTo(px,pad.t);ctx.lineTo(px,h-pad.b);ctx.stroke();ctx.textAlign='center';ctx.fillText(x.toFixed(0)+'°',px,h-pad.b+23);}
 ctx.strokeStyle='#526a76';ctx.lineWidth=1.5;ctx.beginPath();ctx.moveTo(pad.l,pad.t);ctx.lineTo(pad.l,h-pad.b);ctx.lineTo(w-pad.r,h-pad.b);ctx.stroke();
 ctx.fillStyle='#123047';ctx.textAlign='center';ctx.font='bold 14px system-ui';ctx.fillText('Panel tilt angle (degrees)',(pad.l+w-pad.r)/2,h-12);
 ctx.save();ctx.translate(20,(h-pad.b+pad.t)/2);ctx.rotate(-Math.PI/2);ctx.fillText('Electrical power (W)',0,0);ctx.restore();
 ctx.strokeStyle='#f0a51b';ctx.lineWidth=3;ctx.beginPath();let started=false;for(let i=0;i<=300;i++){const x=min+(max-min)*i/300,y=fitted.predict(x);if(!Number.isFinite(y))continue;if(!started){ctx.moveTo(X(x),Y(y));started=true}else ctx.lineTo(X(x),Y(y));}ctx.stroke();
 ctx.fillStyle='#087e8b';grouped.forEach(d=>{ctx.beginPath();ctx.arc(X(d.x),Y(d.y),5,0,Math.PI*2);ctx.fill();});
 ctx.fillStyle='#b43b42';ctx.beginPath();ctx.arc(X(opt.x),Y(opt.y),6,0,Math.PI*2);ctx.fill();ctx.setLineDash([5,4]);ctx.strokeStyle='#b43b42';ctx.beginPath();ctx.moveTo(X(opt.x),pad.t);ctx.lineTo(X(opt.x),h-pad.b);ctx.stroke();ctx.setLineDash([]);
 graphPoints={X,Y,min,max,ymin,ymax,opt}; 
}
$('chart').addEventListener('mousemove',e=>{
 if(!fitted||!graphPoints.X)return;const rect=e.target.getBoundingClientRect(),xpx=(e.clientX-rect.left)/rect.width*e.target.width,ypx=(e.clientY-rect.top)/rect.height*e.target.height;
 const g=graphPoints,angle=g.min+(xpx-75)/(e.target.width-100)*(g.max-g.min);
 if(angle<g.min||angle>g.max)return;const power=fitted.predict(angle);setStatus('graphStatus',`Pointer estimate: θ = ${fmt(angle,2)}°, fitted power = ${fmt(power,4)} W. Predicted optimum: ${fmt(g.opt.x,2)}°.`); 
});
$('chart').addEventListener('mouseleave',()=>{if(fitted)setStatus('graphStatus',`Analysis complete. Predicted maximum in selected range: ${fmt(graphPoints.opt.x,2)}° at ${fmt(graphPoints.opt.y,4)} W.`)});
function loadSample(){
 tbody.innerHTML='';sampleLoaded=true;
 // Synthetic illustrative data only; not measured in Bangui.
 const rows=[{angle:0,voltage:5.20,current:.42,trial:'SAMPLE 1'},{angle:0,voltage:5.16,current:.43,trial:'SAMPLE 2'},{angle:15,voltage:5.48,current:.47,trial:'SAMPLE 1'},{angle:15,voltage:5.52,current:.46,trial:'SAMPLE 2'},{angle:30,voltage:5.71,current:.50,trial:'SAMPLE 1'},{angle:30,voltage:5.68,current:.51,trial:'SAMPLE 2'},{angle:45,voltage:5.56,current:.48,trial:'SAMPLE 1'},{angle:45,voltage:5.59,current:.47,trial:'SAMPLE 2'},{angle:60,voltage:5.20,current:.43,trial:'SAMPLE 1'},{angle:60,voltage:5.24,current:.42,trial:'SAMPLE 2'},{angle:75,voltage:4.62,current:.36,trial:'SAMPLE 1'},{angle:75,voltage:4.58,current:.37,trial:'SAMPLE 2'},{angle:90,voltage:3.91,current:.29,trial:'SAMPLE 1'},{angle:90,voltage:3.95,current:.28,trial:'SAMPLE 2'}];
 rows.forEach(addRow);setStatus('graphStatus','SAMPLE DATA loaded: synthetic illustrative values only, not collected in Bangui. Replace before using results in research.');analyze();
}
function csvEscape(v){return '"'+String(v??'').replace(/"/g,'""')+'"'}
function download(name,content,type='text/csv;charset=utf-8;'){const a=document.createElement('a');a.href=URL.createObjectURL(new Blob([content],{type}));a.download=name;document.body.appendChild(a);a.click();a.remove();setTimeout(()=>URL.revokeObjectURL(a.href),1000)}
function exportCSV(){
 const data=readRows(),lines=[['site','date','angle_deg','voltage_input','current_input','voltage_unit','current_unit','power_W','trial','row_valid'].join(',')];
 data.forEach(d=>lines.push([ $('site').value,$('date').value,d.angle,d.voltage,d.current,$('vunit').value,$('iunit').value,d.valid?d.power:'',d.trial,d.valid].map(csvEscape).join(',')));
 if(fitted){const min=Number($('minAngle').value),max=Number($('maxAngle').value),step=Number($('step').value),opt=findOptimum(fitted,min,max,step),m=calcMetrics(grouped,fitted);lines.push('');lines.push(['model',$('fit').value].map(csvEscape).join(','));lines.push(['equation',fitted.eq].map(csvEscape).join(','));lines.push(['predicted_optimum_angle_deg',opt.x].map(csvEscape).join(','));lines.push(['predicted_power_W',opt.y].map(csvEscape).join(','));lines.push(['MAE_W',m.mae].map(csvEscape).join(','));lines.push(['RMSE_W',m.rmse].map(csvEscape).join(','));lines.push(['R_squared',m.r2].map(csvEscape).join(','));}
 download('SINAG_results.csv',lines.join('\r\n'));
}
$('addRow').addEventListener('click',()=>addRow());
$('recalc').addEventListener('click',analyze);
$('sample').addEventListener('click',loadSample);
$('clear').addEventListener('click',()=>{tbody.innerHTML='';for(let i=0;i<3;i++)addRow();fitted=null;grouped=[];$('observed').textContent=$('optimal').textContent=$('maxPower').textContent='—';$('nRows').textContent='0';$('mae').textContent=$('rmse').textContent=$('r2').textContent='—';$('modelEquation').textContent='Model equation will appear here after analysis.';$('optimalCalculation').textContent='Enter at least three measurements at three distinct angles, then select Analyze data to display the calculation.';$('chart').getContext('2d').clearRect(0,0,1000,520);setStatus('graphStatus','Rows cleared. Enter real measurements, then Analyze data.');});
$('csvTemplate').addEventListener('click',()=>download('SINAG_template.csv','angle,voltage,current,trial\r\n0,5.2,0.42,trial 1\r\n15,5.4,0.45,trial 1\r\n30,5.6,0.48,trial 1\r\n'));
$('export').addEventListener('click',exportCSV);
$('upload').addEventListener('change',async e=>{
 const file=e.target.files[0];if(!file)return;const text=await file.text();
 const parseLine=line=>{const out=[];let cur='',quoted=false;for(let i=0;i<line.length;i++){const ch=line[i];if(ch==='"'&&line[i+1]==='"'&&quoted){cur+='"';i++}else if(ch==='"')quoted=!quoted;else if(ch===','&&!quoted){out.push(cur);cur=''}else cur+=ch}out.push(cur);return out};
 const lines=text.split(/\r?\n/).filter(x=>x.trim());if(lines.length<2){alert('CSV needs a header and at least one data row.');return;}
 const headers=parseLine(lines[0]).map(x=>x.trim().toLowerCase().replace(/[°()]/g,'').replace(/\s+/g,'_'));
 const find=names=>headers.findIndex(h=>names.includes(h));
 const ai=find(['angle','angle_deg','tilt','tilt_angle']),vi=find(['voltage','voltage_v','v']),ii=find(['current','current_a','i']),ti=find(['trial','time','time_trial']);
 if(ai<0||vi<0||ii<0){alert('CSV must include angle, voltage, and current columns.');return;}
 tbody.innerHTML='';lines.slice(1).forEach(line=>{const a=parseLine(line);if(!a.some(x=>x.trim()))return;addRow({angle:a[ai]??'',voltage:a[vi]??'',current:a[ii]??'',trial:ti>=0?(a[ti]??''):''})});
 sampleLoaded=false;setStatus('graphStatus',`Imported ${Math.max(0,lines.length-1)} CSV rows. Check units and values, then Analyze data.`);e.target.value='';
});
$('validate').addEventListener('click',()=>{
 if(!fitted){setStatus('validationResult','Analyze a dataset first.','error');return;}
 const angle=Number($('valAngle').value),power=Number($('valPower').value);
 if($('valAngle').value===''||$('valPower').value===''||!Number.isFinite(angle)||!Number.isFinite(power)||angle<0||angle>90||power<0){setStatus('validationResult','Enter a valid angle from 0° to 90° and a non-negative measured mean power in watts.','error');return;}
 const pred=fitted.predict(angle),err=power-pred,abs=Math.abs(err),pct=Math.abs(pred)>1e-9?abs/Math.abs(pred)*100:NaN;
 setStatus('validationResult',`At ${fmt(angle,2)}°: model predicts ${fmt(pred,4)} W; fresh measured mean = ${fmt(power,4)} W; signed error (measured − predicted) = ${fmt(err,4)} W; absolute percentage error = ${Number.isFinite(pct)?fmt(pct,2)+'%':'not defined'}. Record the fresh trial conditions and repeat the measurement for stronger validation.`);
});
$('date').value=new Date().toISOString().slice(0,10);
for(let i=0;i<3;i++)addRow();
})();
</script>
</body>
</html>
