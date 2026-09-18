# CMB-1701-Calculation-Sol
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CMB Simple BIR 1701 Calculator + Printable Guide</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; }
    body { background: #f0f4f8; padding: 20px; color: #1a1a1a; }
    .container { max-width: 900px; margin: 0 auto; }
    h1 { text-align: center; color: #1e40af; margin-bottom: 8px; font-size: 1.6rem; }
    .subtitle { text-align: center; color: #64748b; margin-bottom: 24px; font-size: 0.95rem; }
    
    .card { background: white; border-radius: 16px; padding: 24px; margin-bottom: 20px; box-shadow: 0 4px 12px rgba(0,0,0,0.08); }
    .card h2 { font-size: 1.15rem; color: #1e40af; margin-bottom: 16px; border-bottom: 2px solid #e0e7ff; padding-bottom: 8px; }
    
    label { display: block; font-weight: 600; margin-bottom: 6px; font-size: 0.9rem; }
    input[type="number"], select { width: 100%; padding: 12px 14px; border: 2px solid #e2e8f0; border-radius: 10px; font-size: 1rem; margin-bottom: 16px; }
    input:focus, select:focus { outline: none; border-color: #3b82f6; }
    
    .method-toggle { display: flex; gap: 12px; margin-bottom: 20px; }
    .method-btn { flex: 1; padding: 12px; border: 2px solid #e2e8f0; border-radius: 10px; background: white; cursor: pointer; font-weight: 600; text-align: center; transition: all 0.2s; }
    .method-btn.active { background: #3b82f6; color: white; border-color: #3b82f6; }
    
    .result-box { background: #f8fafc; border-radius: 12px; padding: 16px; margin-top: 12px; }
    .result-row { display: flex; justify-content: space-between; padding: 8px 0; border-bottom: 1px solid #e2e8f0; }
    .result-row:last-child { border-bottom: none; font-weight: 700; font-size: 1.1rem; color: #1e40af; }
    
    .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
    @media (max-width: 600px) { .two-col { grid-template-columns: 1fr; } }
    
    table { width: 100%; border-collapse: collapse; font-size: 0.85rem; margin-top: 12px; }
    th, td { border: 1px solid #cbd5e1; padding: 8px 10px; text-align: left; }
    th { background: #1e40af; color: white; }
    
    .print-section { background: white; border: 2px solid #1e40af; border-radius: 12px; padding: 24px; margin-top: 24px; }
    .print-header { text-align: center; margin-bottom: 20px; }
    .print-header h2 { color: #1e40af; font-size: 1.3rem; }
    .print-field { display: flex; justify-content: space-between; padding: 6px 0; border-bottom: 1px dashed #cbd5e1; font-size: 0.95rem; }
    .print-field strong { color: #1e40af; }
    
    .btn { display: inline-block; background: #1e40af; color: white; border: none; padding: 14px 28px; border-radius: 10px; font-size: 1rem; font-weight: 600; cursor: pointer; margin-top: 16px; width: 100%; }
    .btn:hover { background: #1e3a8a; }
    .btn-secondary { background: #64748b; }
    
    .note { background: #fff7ed; border-left: 4px solid #f97316; padding: 12px 16px; border-radius: 0 8px 8px 0; font-size: 0.9rem; margin-top: 16px; color: #9a3412; }
    
    @media print {
      body { background: white; padding: 0; }
      .no-print { display: none !important; }
      .print-section { border: none; box-shadow: none; }
      .card { box-shadow: none; border: 1px solid #e2e8f0; }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>CMB Simple BIR 1701 Calculator</h1>
    <p class="subtitle">OSD + Printable Form Guide (For Reference Only)</p>

    <!-- INPUT SECTION -->
    <div class="card no-print">
      <h2>1. Enter Your Figures</h2>
      
      <label>Gross Sales / Receipts (₱)</label>
      <input type="number" id="gross" value="3000000" min="0" step="1">
      
      <label>Other Income (₱)</label>
      <input type="number" id="other" value="500000" min="0" step="1">
      
      <label>Tax Credits / Payments Already Made (₱)</label>
      <input type="number" id="credits" value="0" min="0" step="1">
      
      <label>Choose OSD Method</label>
      <div class="method-toggle">
        <div class="method-btn active" id="btnA" onclick="setMethod('A')">Method A<br><small>OSD on Gross only</small></div>
        <div class="method-btn" id="btnB" onclick="setMethod('B')">Method B<br><small>OSD on Total</small></div>
      </div>
      
      <button class="btn" onclick="calculate()">Calculate Now</button>
    </div>

    <!-- RESULTS -->
    <div class="card" id="resultsCard" style="display:none;">
      <h2>2. Computation Results</h2>
      
      <div class="two-col">
        <div>
          <h3 style="font-size:1rem; margin-bottom:8px;">Method A – OSD on Gross only</h3>
          <div class="result-box" id="resultA"></div>
        </div>
        <div>
          <h3 style="font-size:1rem; margin-bottom:8px;">Method B – OSD on Total</h3>
          <div class="result-box" id="resultB"></div>
        </div>
      </div>
      
      <div class="note">
        <strong>Note:</strong> Method A follows the structure of BIR Form 1701 more closely (OSD only on Sales/Receipts, then add Other Income). Method B is what some BIR agents use (OSD on the combined total). Choose the one advised by your RDO / agent.
      </div>
    </div>

    <!-- TAX TABLES -->
    <div class="card no-print">
      <h2>3. Official Tax Rate Tables (for double-checking)</h2>
      
      <h3 style="margin:16px 0 8px; font-size:0.95rem;">Table 2 – Current Rates (Jan 1, 2023 onwards)</h3>
      <table>
        <tr><th>If Taxable Income is:</th><th>Tax Due is:</th></tr>
        <tr><td>Not over ₱250,000</td><td>0%</td></tr>
        <tr><td>Over ₱250,000 but not over ₱400,000</td><td>15% of the excess over ₱250,000</td></tr>
        <tr><td>Over ₱400,000 but not over ₱800,000</td><td>₱22,500 + 20% of the excess over ₱400,000</td></tr>
        <tr><td>Over ₱800,000 but not over ₱2,000,000</td><td>₱102,500 + 25% of the excess over ₱800,000</td></tr>
        <tr><td>Over ₱2,000,000 but not over ₱8,000,000</td><td>₱402,500 + 30% of the excess over ₱2,000,000</td></tr>
        <tr><td>Over ₱8,000,000</td><td>₱2,202,500 + 35% of the excess over ₱8,000,000</td></tr>
      </table>
      
      <h3 style="margin:20px 0 8px; font-size:0.95rem;">Table 1 – Old Rates (2018–2022) – for reference only</h3>
      <table>
        <tr><th>If Taxable Income is:</th><th>Tax Due is:</th></tr>
        <tr><td>Not over ₱250,000</td><td>0%</td></tr>
        <tr><td>Over ₱250,000 but not over ₱400,000</td><td>20% of the excess over ₱250,000</td></tr>
        <tr><td>Over ₱400,000 but not over ₱800,000</td><td>₱30,000 + 25% of the excess over ₱400,000</td></tr>
        <tr><td>Over ₱800,000 but not over ₱2,000,000</td><td>₱130,000 + 30% of the excess over ₱800,000</td></tr>
        <tr><td>Over ₱2,000,000 but not over ₱8,000,000</td><td>₱490,000 + 32% of the excess over ₱2,000,000</td></tr>
        <tr><td>Over ₱8,000,000</td><td>₱2,410,000 + 35% of the excess over ₱8,000,000</td></tr>
      </table>
    </div>

    <!-- PRINTABLE GUIDE -->
    <div class="print-section" id="printSection" style="display:none;">
      <div class="print-header">
        <h2>BIR Form 1701 – Computation Guide</h2>
        <p style="font-size:0.9rem; color:#64748b;">For Reference Only • Not for Official Filing</p>
        <p style="font-size:0.85rem; margin-top:4px;">Generated for guidance when filling eBIRForms or manual Form 1701</p>
      </div>
      
      <div id="printContent"></div>
      
      <button class="btn no-print" onclick="window.print()">🖨️ Print this Guide</button>
      <button class="btn btn-secondary no-print" onclick="calculate()" style="margin-top:10px;">Recalculate</button>
    </div>
  </div>

  <script>
    let currentMethod = 'A';

    function setMethod(m) {
      currentMethod = m;
      document.getElementById('btnA').classList.toggle('active', m === 'A');
      document.getElementById('btnB').classList.toggle('active', m === 'B');
      calculate();
    }

    function formatPeso(n) {
      return '₱' + Number(n).toLocaleString('en-PH', {minimumFractionDigits: 2, maximumFractionDigits: 2});
    }

    function computeTax(taxable) {
      if (taxable <= 250000) return 0;
      if (taxable <= 400000) return (taxable - 250000) * 0.15;
      if (taxable <= 800000) return 22500 + (taxable - 400000) * 0.20;
      if (taxable <= 2000000) return 102500 + (taxable - 800000) * 0.25;
      if (taxable <= 8000000) return 402500 + (taxable - 2000000) * 0.30;
      return 2202500 + (taxable - 8000000) * 0.35;
    }

    function calculate() {
      const gross = Number(document.getElementById('gross').value) || 0;
      const other = Number(document.getElementById('other').value) || 0;
      const credits = Number(document.getElementById('credits').value) || 0;

      // Method A: OSD only on Gross
      const osdA = gross * 0.40;
      const taxableA = (gross - osdA) + other;
      const taxA = computeTax(taxableA);
      const payableA = Math.max(0, taxA - credits);

      // Method B: OSD on Total
      const totalB = gross + other;
      const osdB = totalB * 0.40;
      const taxableB = totalB - osdB;
      const taxB = computeTax(taxableB);
      const payableB = Math.max(0, taxB - credits);

      // Show results
      document.getElementById('resultsCard').style.display = 'block';
      document.getElementById('resultA').innerHTML = `
        <div class="result-row"><span>Gross Sales</span><span>${formatPeso(gross)}</span></div>
        <div class="result-row"><span>OSD 40%</span><span>- ${formatPeso(osdA)}</span></div>
        <div class="result-row"><span>Other Income</span><span>+ ${formatPeso(other)}</span></div>
        <div class="result-row"><span>Taxable Income</span><span>${formatPeso(taxableA)}</span></div>
        <div class="result-row"><span>Tax Due</span><span>${formatPeso(taxA)}</span></div>
        <div class="result-row"><span>Tax Payable</span><span>${formatPeso(payableA)}</span></div>
      `;
      document.getElementById('resultB').innerHTML = `
        <div class="result-row"><span>Total Income</span><span>${formatPeso(totalB)}</span></div>
        <div class="result-row"><span>OSD 40%</span><span>- ${formatPeso(osdB)}</span></div>
        <div class="result-row"><span>Taxable Income</span><span>${formatPeso(taxableB)}</span></div>
        <div class="result-row"><span>Tax Due</span><span>${formatPeso(taxB)}</span></div>
        <div class="result-row"><span>Tax Payable</span><span>${formatPeso(payableB)}</span></div>
      `;

      // Printable Guide (uses the currently selected method)
      const useA = currentMethod === 'A';
      const taxable = useA ? taxableA : taxableB;
      const taxDue = useA ? taxA : taxB;
      const payable = useA ? payableA : payableB;
      const osd = useA ? osdA : osdB;
      const methodLabel = useA ? 'Method A – OSD on Gross Sales/Receipts only' : 'Method B – OSD on Total (Gross + Other)';

      document.getElementById('printSection').style.display = 'block';
      document.getElementById('printContent').innerHTML = `
        <div class="print-field"><span>Taxpayer Type</span><strong>Single Proprietor / Business Income</strong></div>
        <div class="print-field"><span>Method of Deduction</span><strong>Optional Standard Deduction (OSD)</strong></div>
        <div class="print-field"><span>Computation Method Used</span><strong>${methodLabel}</strong></div>
        <br>
        <div class="print-field"><span>Gross Sales / Receipts</span><strong>${formatPeso(gross)}</strong></div>
        <div class="print-field"><span>Other Income</span><strong>${formatPeso(other)}</strong></div>
        <div class="print-field"><span>OSD (40%)</span><strong>${formatPeso(osd)}</strong></div>
        <div class="print-field"><span><strong>Taxable Income</strong></span><strong>${formatPeso(taxable)}</strong></div>
        <div class="print-field"><span><strong>Tax Due (Item 22)</strong></span><strong>${formatPeso(taxDue)}</strong></div>
        <div class="print-field"><span>Less: Tax Credits / Payments</span><strong>${formatPeso(credits)}</strong></div>
        <div class="print-field"><span><strong>Tax Payable / (Over