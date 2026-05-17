import React, { useState, useMemo } from 'react';

// VA Multifamily Strategy Calculator
// Built for someone doing zero-down VA acquisitions in RI

const PRESETS = {
  bristol: { label: 'Bristol, RI', taxRate: 0.0132, insRate: 0.005 },
  tiverton: { label: 'Tiverton, RI', taxRate: 0.014, insRate: 0.005 },
  warren: { label: 'Warren, RI', taxRate: 0.0155, insRate: 0.005 },
  portsmouth: { label: 'Portsmouth, RI', taxRate: 0.0117, insRate: 0.005 },
  custom: { label: 'Custom', taxRate: 0.014, insRate: 0.005 },
};

const fmt = (n) => {
  if (!isFinite(n) || isNaN(n)) return '—';
  const abs = Math.abs(n);
  const sign = n < 0 ? '−' : '';
  if (abs >= 1000000) return `${sign}$${(abs / 1000000).toFixed(2)}M`;
  if (abs >= 1000) return `${sign}$${Math.round(abs).toLocaleString()}`;
  return `${sign}$${abs.toFixed(0)}`;
};

const fmtMo = (n) => {
  if (!isFinite(n) || isNaN(n)) return '—';
  const sign = n < 0 ? '−' : '';
  return `${sign}$${Math.round(Math.abs(n)).toLocaleString()}/mo`;
};

const fmtPct = (n) => {
  if (!isFinite(n) || isNaN(n)) return '—';
  return `${(n * 100).toFixed(1)}%`;
};

// Monthly mortgage payment
const pmt = (principal, annualRate, years) => {
  const r = annualRate / 12;
  const n = years * 12;
  if (r === 0) return principal / n;
  return (principal * r * Math.pow(1 + r, n)) / (Math.pow(1 + r, n) - 1);
};

export default function Calculator() {
  // --- Property inputs ---
  const [town, setTown] = useState('bristol');
  const [purchasePrice, setPurchasePrice] = useState(700000);
  const [sqft, setSqft] = useState(2200);
  const [bedrooms, setBedrooms] = useState(3);
  const [baths, setBaths] = useState(2);
  const [taxRate, setTaxRate] = useState(0.0132);
  const [insRate, setInsRate] = useState(0.005);

  // --- Loan ---
  const [rate, setRate] = useState(0.0625);
  const [term, setTerm] = useState(30);
  const [closingPct, setClosingPct] = useState(0.03);
  const [fundingFee, setFundingFee] = useState(0); // exempt
  const [helocRate, setHelocRate] = useState(0.085);

  // --- Market rents ---
  const [rentADU1BR, setRentADU1BR] = useState(1800);
  const [rentADU2BR, setRentADU2BR] = useState(2200);
  const [rentDuplex, setRentDuplex] = useState(2400);
  const [rentMFUnit, setRentMFUnit] = useState(2800);
  const [vacancy, setVacancy] = useState(0.05);
  const [maintenance, setMaintenance] = useState(0.10);
  const [pmFee, setPmFee] = useState(0);

  // --- Construction ---
  const [intADUPsf, setIntADUPsf] = useState(140);
  const [detADUPsf, setDetADUPsf] = useState(350);
  const [duplexPsf, setDuplexPsf] = useState(200);
  const [intADUSf, setIntADUSf] = useState(800);
  const [detADUSf, setDetADUSf] = useState(900);
  const [duplexSf, setDuplexSf] = useState(1000);
  const [softCosts, setSoftCosts] = useState(25000);
  const [contingency, setContingency] = useState(0.20);
  const [mfReno, setMfReno] = useState(40000);

  // --- Multifamily unit count (for existing MF scenario) ---
  const [mfUnitsRented, setMfUnitsRented] = useState(2); // owner occupies 1

  const handleTownChange = (t) => {
    setTown(t);
    if (t !== 'custom') {
      setTaxRate(PRESETS[t].taxRate);
      setInsRate(PRESETS[t].insRate);
    }
  };

  // --- Scenario calculations ---
  const scenarios = useMemo(() => {
    const closing = purchasePrice * closingPct;
    const funding = purchasePrice * fundingFee;
    const loanAmt = purchasePrice + funding; // funding fee rolled in
    const monthlyPI = pmt(loanAmt, rate, term);
    const monthlyTax = (purchasePrice * taxRate) / 12;
    const monthlyIns = (purchasePrice * insRate) / 12;
    const basePITI = monthlyPI + monthlyTax + monthlyIns;

    const build = (cfg) => {
      const hard = cfg.constructionCost;
      const soft = cfg.softCosts;
      const cont = hard * cfg.contingencyPct;
      const totalConstruction = hard + soft + cont;
      const totalProject = purchasePrice + closing + totalConstruction;

      const helocPortion = totalConstruction * 0.6;
      const cashConstruction = totalConstruction * 0.4;
      const helocMonthly = (helocPortion * helocRate) / 12;
      const totalMonthlyDebt = basePITI + helocMonthly;

      const grossRent = cfg.grossRent;
      const vacLoss = grossRent * vacancy;
      const maintReserve = grossRent * maintenance;
      const pm = grossRent * pmFee;
      const netRent = grossRent - vacLoss - maintReserve - pm;

      const monthlyCF = netRent - totalMonthlyDebt;
      const effectiveHousing = totalMonthlyDebt - netRent;
      const cashOutPocket = closing + cashConstruction;
      const cocReturn = (monthlyCF * 12) / cashOutPocket;

      return {
        ...cfg,
        totalProject,
        cashOutPocket,
        totalConstruction,
        monthlyPI,
        monthlyTax,
        monthlyIns,
        helocMonthly,
        totalMonthlyDebt,
        grossRent,
        netRent,
        monthlyCF,
        effectiveHousing,
        cocReturn,
      };
    };

    return {
      adu_interior: build({
        name: 'SFH + Interior ADU',
        subtitle: 'Convert basement/attic to legal 800sf unit',
        constructionCost: intADUSf * intADUPsf,
        softCosts: softCosts,
        contingencyPct: contingency,
        grossRent: rentADU1BR,
        timeline: '14–18 mo to first rent',
        risk: 'Medium',
      }),
      adu_detached: build({
        name: 'SFH + Detached ADU',
        subtitle: 'Ground-up 900sf backyard cottage',
        constructionCost: detADUSf * detADUPsf,
        softCosts: softCosts,
        contingencyPct: contingency,
        grossRent: rentADU1BR,
        timeline: '18–24 mo to first rent',
        risk: 'High',
      }),
      duplex_conv: build({
        name: 'SFH → Legal Duplex',
        subtitle: 'Full conversion (needs R-2 zoning)',
        constructionCost: duplexSf * duplexPsf,
        softCosts: softCosts,
        contingencyPct: contingency,
        grossRent: rentDuplex,
        timeline: '18–30 mo to first rent',
        risk: 'Highest',
      }),
      existing_mf: build({
        name: 'Existing 2-4 Unit',
        subtitle: 'Buy an already-multifamily, occupy one unit',
        constructionCost: mfReno,
        softCosts: 5000,
        contingencyPct: 0.15,
        grossRent: rentMFUnit * mfUnitsRented,
        timeline: 'Day 1 — rent in place',
        risk: 'Lowest',
      }),
    };
  }, [
    purchasePrice, closingPct, fundingFee, rate, term, taxRate, insRate,
    helocRate, vacancy, maintenance, pmFee,
    intADUSf, intADUPsf, detADUSf, detADUPsf, duplexSf, duplexPsf,
    softCosts, contingency, mfReno,
    rentADU1BR, rentDuplex, rentMFUnit, mfUnitsRented,
  ]);

  return (
    <div style={styles.app}>
      <style>{globalCSS}</style>

      <header style={styles.header}>
        <div style={styles.headerLeft}>
          <div style={styles.spec}>FORM 1099-RE / REV. 2026</div>
          <h1 style={styles.title}>VA MULTIFAMILY<br />SCENARIO CALCULATOR</h1>
          <div style={styles.subtitle}>Zero-down acquisition modeling · Bristol County, RI</div>
        </div>
        <div style={styles.headerRight}>
          <div style={styles.statBlock}>
            <div style={styles.statLabel}>STATUS</div>
            <div style={styles.statValueGreen}>● LIVE</div>
          </div>
          <div style={styles.statBlock}>
            <div style={styles.statLabel}>FUNDING FEE</div>
            <div style={styles.statValue}>EXEMPT</div>
          </div>
        </div>
      </header>

      <div style={styles.grid}>
        {/* === LEFT COLUMN: INPUTS === */}
        <div style={styles.inputPanel}>

          <Section title="01 · PROPERTY">
            <div style={styles.row2}>
              <Field label="Town" type="select" value={town} onChange={handleTownChange}
                options={Object.entries(PRESETS).map(([k, v]) => ({ value: k, label: v.label }))} />
              <Field label="Purchase Price" value={purchasePrice} onChange={setPurchasePrice} prefix="$" step={10000} />
            </div>
            <div style={styles.row3}>
              <Field label="Total Sq Ft" value={sqft} onChange={setSqft} step={100} />
              <Field label="Beds" value={bedrooms} onChange={setBedrooms} step={1} />
              <Field label="Baths" value={baths} onChange={setBaths} step={0.5} />
            </div>
            <div style={styles.row2}>
              <Field label="Tax Rate" value={taxRate} onChange={setTaxRate} suffix="%" pct step={0.001} />
              <Field label="Insurance Rate" value={insRate} onChange={setInsRate} suffix="%" pct step={0.0005} />
            </div>
            <Hint>Paste from homes.com / Zillow listing → adjust town for accurate tax rate</Hint>
          </Section>

          <Section title="02 · VA LOAN">
            <div style={styles.row2}>
              <Field label="Interest Rate" value={rate} onChange={setRate} suffix="%" pct step={0.0025} />
              <Field label="Term (yrs)" value={term} onChange={setTerm} step={1} />
            </div>
            <div style={styles.row2}>
              <Field label="Closing %" value={closingPct} onChange={setClosingPct} suffix="%" pct step={0.005} />
              <Field label="HELOC Rate" value={helocRate} onChange={setHelocRate} suffix="%" pct step={0.0025} />
            </div>
          </Section>

          <Section title="03 · RENTAL MARKET">
            <div style={styles.row2}>
              <Field label="ADU 1BR Rent" value={rentADU1BR} onChange={setRentADU1BR} prefix="$" suffix="/mo" step={50} />
              <Field label="ADU 2BR Rent" value={rentADU2BR} onChange={setRentADU2BR} prefix="$" suffix="/mo" step={50} />
            </div>
            <div style={styles.row2}>
              <Field label="Duplex Unit Rent" value={rentDuplex} onChange={setRentDuplex} prefix="$" suffix="/mo" step={50} />
              <Field label="MF Unit Rent" value={rentMFUnit} onChange={setRentMFUnit} prefix="$" suffix="/mo" step={50} />
            </div>
            <div style={styles.row3}>
              <Field label="Vacancy" value={vacancy} onChange={setVacancy} suffix="%" pct step={0.01} />
              <Field label="Maintenance" value={maintenance} onChange={setMaintenance} suffix="%" pct step={0.01} />
              <Field label="PM Fee" value={pmFee} onChange={setPmFee} suffix="%" pct step={0.01} />
            </div>
            <Field label="MF: Units You'll Rent (occupy 1)" value={mfUnitsRented} onChange={setMfUnitsRented} step={1} />
          </Section>

          <Section title="04 · CONSTRUCTION">
            <div style={styles.row2}>
              <Field label="Interior ADU $/sf" value={intADUPsf} onChange={setIntADUPsf} prefix="$" step={10} />
              <Field label="Interior ADU sf" value={intADUSf} onChange={setIntADUSf} step={50} />
            </div>
            <div style={styles.row2}>
              <Field label="Detached ADU $/sf" value={detADUPsf} onChange={setDetADUPsf} prefix="$" step={10} />
              <Field label="Detached ADU sf" value={detADUSf} onChange={setDetADUSf} step={50} />
            </div>
            <div style={styles.row2}>
              <Field label="Duplex Conv $/sf" value={duplexPsf} onChange={setDuplexPsf} prefix="$" step={10} />
              <Field label="Duplex Conv sf" value={duplexSf} onChange={setDuplexSf} step={50} />
            </div>
            <div style={styles.row2}>
              <Field label="Soft Costs" value={softCosts} onChange={setSoftCosts} prefix="$" step={1000} />
              <Field label="Contingency" value={contingency} onChange={setContingency} suffix="%" pct step={0.05} />
            </div>
            <Field label="Existing MF Light Reno Budget" value={mfReno} onChange={setMfReno} prefix="$" step={5000} />
          </Section>
        </div>

        {/* === RIGHT COLUMN: SCENARIOS === */}
        <div style={styles.outputPanel}>
          <div style={styles.outputHeader}>
            <div style={styles.outputTitle}>SCENARIO COMPARISON</div>
            <div style={styles.outputSpec}>4 PATHS · LIVE CALCULATION</div>
          </div>

          {Object.entries(scenarios).map(([key, s], idx) => (
            <ScenarioCard key={key} scenario={s} index={idx + 1} />
          ))}

          <SummaryTable scenarios={scenarios} />

          <Footer />
        </div>
      </div>
    </div>
  );
}

// ============ COMPONENTS ============

function Section({ title, children }) {
  return (
    <div style={styles.section}>
      <div style={styles.sectionTitle}>{title}</div>
      <div style={styles.sectionBody}>{children}</div>
    </div>
  );
}

function Field({ label, value, onChange, prefix, suffix, type, options, pct, step }) {
  const displayValue = pct ? (value * 100).toFixed(2) : value;

  const handleChange = (e) => {
    const v = e.target.value;
    if (type === 'select') {
      onChange(v);
      return;
    }
    const num = parseFloat(v);
    if (isNaN(num)) {
      onChange(0);
      return;
    }
    onChange(pct ? num / 100 : num);
  };

  return (
    <div style={styles.field}>
      <label style={styles.label}>{label}</label>
      <div style={styles.inputWrapper}>
        {prefix && <span style={styles.affix}>{prefix}</span>}
        {type === 'select' ? (
          <select value={value} onChange={handleChange} style={styles.select}>
            {options.map(o => <option key={o.value} value={o.value}>{o.label}</option>)}
          </select>
        ) : (
          <input
            type="number"
            value={displayValue}
            onChange={handleChange}
            step={step || 'any'}
            style={styles.input}
          />
        )}
        {suffix && <span style={styles.affix}>{suffix}</span>}
      </div>
    </div>
  );
}

function Hint({ children }) {
  return <div style={styles.hint}>► {children}</div>;
}

function ScenarioCard({ scenario, index }) {
  const positive = scenario.monthlyCF >= 0;
  const numStr = String(index).padStart(2, '0');

  return (
    <div style={styles.card}>
      <div style={styles.cardHeader}>
        <div>
          <div style={styles.cardIndex}>SCENARIO {numStr}</div>
          <div style={styles.cardTitle}>{scenario.name}</div>
          <div style={styles.cardSub}>{scenario.subtitle}</div>
        </div>
        <div style={styles.cardMeta}>
          <div style={styles.metaBlock}>
            <div style={styles.metaLabel}>RISK</div>
            <div style={styles.metaValue}>{scenario.risk}</div>
          </div>
          <div style={styles.metaBlock}>
            <div style={styles.metaLabel}>TIMELINE</div>
            <div style={styles.metaValue}>{scenario.timeline}</div>
          </div>
        </div>
      </div>

      <div style={styles.bigNumberRow}>
        <BigNumber label="MONTHLY CASH FLOW" value={fmtMo(scenario.monthlyCF)}
          accent={positive ? 'green' : 'red'} primary />
        <BigNumber label="EFFECTIVE HOUSING COST" value={fmtMo(scenario.effectiveHousing)} />
        <BigNumber label="ALL-IN PROJECT" value={fmt(scenario.totalProject)} />
        <BigNumber label="CASH NEEDED" value={fmt(scenario.cashOutPocket)} />
      </div>

      <div style={styles.breakdown}>
        <BreakdownColumn title="MONTHLY DEBT SERVICE">
          <BreakdownLine label="P&I" value={fmtMo(scenario.monthlyPI)} />
          <BreakdownLine label="Taxes" value={fmtMo(scenario.monthlyTax)} />
          <BreakdownLine label="Insurance" value={fmtMo(scenario.monthlyIns)} />
          <BreakdownLine label="HELOC (construction)" value={fmtMo(scenario.helocMonthly)} />
          <BreakdownLine label="TOTAL" value={fmtMo(scenario.totalMonthlyDebt)} bold />
        </BreakdownColumn>

        <BreakdownColumn title="MONTHLY INCOME">
          <BreakdownLine label="Gross Rent" value={fmtMo(scenario.grossRent)} />
          <BreakdownLine label="− Vacancy" value={fmtMo(-scenario.grossRent * 0.05)} subtle />
          <BreakdownLine label="− Maintenance" value={fmtMo(-scenario.grossRent * 0.10)} subtle />
          <BreakdownLine label="Net Rent" value={fmtMo(scenario.netRent)} bold />
          <BreakdownLine label="Cash-on-Cash" value={fmtPct(scenario.cocReturn)} bold />
        </BreakdownColumn>
      </div>
    </div>
  );
}

function BigNumber({ label, value, accent, primary }) {
  let color = '#1a1a1a';
  if (accent === 'green') color = '#0d7c3e';
  if (accent === 'red') color = '#c63838';

  return (
    <div style={{ ...styles.bigNum, ...(primary ? styles.bigNumPrimary : {}) }}>
      <div style={styles.bigNumLabel}>{label}</div>
      <div style={{ ...styles.bigNumValue, color }}>{value}</div>
    </div>
  );
}

function BreakdownColumn({ title, children }) {
  return (
    <div style={styles.breakCol}>
      <div style={styles.breakTitle}>{title}</div>
      {children}
    </div>
  );
}

function BreakdownLine({ label, value, bold, subtle }) {
  return (
    <div style={{
      ...styles.breakLine,
      ...(bold ? { fontWeight: 700, borderTop: '1px solid #d4d4cc', paddingTop: 6, marginTop: 4 } : {}),
      ...(subtle ? { opacity: 0.6 } : {}),
    }}>
      <span>{label}</span>
      <span style={{ fontVariantNumeric: 'tabular-nums' }}>{value}</span>
    </div>
  );
}

function SummaryTable({ scenarios }) {
  const rows = Object.values(scenarios);
  const best = rows.reduce((a, b) => (a.monthlyCF > b.monthlyCF ? a : b));

  return (
    <div style={styles.summary}>
      <div style={styles.summaryHeader}>SUMMARY · RANKED BY MONTHLY CASH FLOW</div>
      <table style={styles.table}>
        <thead>
          <tr>
            <th style={styles.th}>Scenario</th>
            <th style={styles.thRight}>All-In</th>
            <th style={styles.thRight}>Cash Needed</th>
            <th style={styles.thRight}>Monthly CF</th>
            <th style={styles.thRight}>Eff. Housing</th>
            <th style={styles.thRight}>CoC</th>
          </tr>
        </thead>
        <tbody>
          {rows.sort((a, b) => b.monthlyCF - a.monthlyCF).map((s, i) => (
            <tr key={s.name} style={s === best ? styles.bestRow : {}}>
              <td style={styles.td}>
                {s === best && <span style={styles.bestTag}>BEST</span>}
                {s.name}
              </td>
              <td style={styles.tdRight}>{fmt(s.totalProject)}</td>
              <td style={styles.tdRight}>{fmt(s.cashOutPocket)}</td>
              <td style={{ ...styles.tdRight, color: s.monthlyCF >= 0 ? '#0d7c3e' : '#c63838', fontWeight: 700 }}>
                {fmtMo(s.monthlyCF)}
              </td>
              <td style={styles.tdRight}>{fmtMo(s.effectiveHousing)}</td>
              <td style={styles.tdRight}>{fmtPct(s.cocReturn)}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

function Footer() {
  return (
    <div style={styles.footer}>
      <div style={styles.footerLine}>
        <strong>Assumptions baked in:</strong> VA zero-down, funding fee exempt, owner-occupancy in Year 1.
        Construction financing modeled as 60% HELOC / 40% cash post-seasoning.
        Maintenance reserve and vacancy deducted from gross rent before debt service.
      </div>
      <div style={styles.footerLine}>
        <strong>Not modeled:</strong> RI veteran property tax exemption (varies by town — could reduce monthly tax 10-25%),
        rent escalation over time, depreciation tax shield, capital reserves for major systems (roof/HVAC at ~$200/mo).
      </div>
      <div style={styles.footerLine} className="mono">
        v1.0 · BUILT FOR D. AT VATN · NOT FINANCIAL ADVICE · GET REAL QUOTES FROM CONTRACTORS BEFORE COMMITTING
      </div>
    </div>
  );
}

// ============ STYLES ============

const globalCSS = `
  @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;700&family=Archivo:wght@400;500;600;700;900&display=swap');
  * { box-sizing: border-box; }
  body { margin: 0; }
  input[type="number"]::-webkit-outer-spin-button,
  input[type="number"]::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  input[type="number"] { -moz-appearance: textfield; }
  input:focus, select:focus { outline: 2px solid #ff6b35; outline-offset: -1px; }
  .mono { font-family: 'JetBrains Mono', monospace; }
`;

const styles = {
  app: {
    fontFamily: "'Archivo', sans-serif",
    background: '#e8e6dd',
    minHeight: '100vh',
    padding: '20px',
    color: '#1a1a1a',
  },
  header: {
    background: '#1a1a1a',
    color: '#e8e6dd',
    padding: '24px 28px',
    display: 'flex',
    justifyContent: 'space-between',
    alignItems: 'flex-start',
    borderBottom: '4px solid #ff6b35',
    marginBottom: '20px',
  },
  headerLeft: { flex: 1 },
  headerRight: { display: 'flex', gap: '32px' },
  spec: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '10px',
    letterSpacing: '0.15em',
    color: '#ff6b35',
    marginBottom: '8px',
  },
  title: {
    fontSize: '36px',
    fontWeight: 900,
    lineHeight: 1,
    margin: 0,
    letterSpacing: '-0.02em',
  },
  subtitle: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '12px',
    marginTop: '12px',
    opacity: 0.7,
  },
  statBlock: { textAlign: 'right' },
  statLabel: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '10px',
    letterSpacing: '0.1em',
    opacity: 0.6,
    marginBottom: '4px',
  },
  statValue: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '14px',
    fontWeight: 700,
  },
  statValueGreen: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '14px',
    fontWeight: 700,
    color: '#4ade80',
  },
  grid: {
    display: 'grid',
    gridTemplateColumns: '380px 1fr',
    gap: '20px',
    alignItems: 'flex-start',
  },
  inputPanel: {
    background: '#fcfaf3',
    border: '1px solid #1a1a1a',
    padding: '0',
    position: 'sticky',
    top: '20px',
    maxHeight: 'calc(100vh - 40px)',
    overflowY: 'auto',
  },
  outputPanel: {
    display: 'flex',
    flexDirection: 'column',
    gap: '16px',
  },
  outputHeader: {
    background: '#1a1a1a',
    color: '#e8e6dd',
    padding: '16px 20px',
    display: 'flex',
    justifyContent: 'space-between',
    alignItems: 'center',
  },
  outputTitle: {
    fontSize: '18px',
    fontWeight: 800,
    letterSpacing: '-0.01em',
  },
  outputSpec: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '10px',
    letterSpacing: '0.15em',
    color: '#ff6b35',
  },
  section: {
    borderBottom: '1px solid #1a1a1a',
  },
  sectionTitle: {
    background: '#1a1a1a',
    color: '#e8e6dd',
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '11px',
    letterSpacing: '0.15em',
    padding: '8px 16px',
    fontWeight: 700,
  },
  sectionBody: {
    padding: '14px 16px',
    display: 'flex',
    flexDirection: 'column',
    gap: '10px',
  },
  row2: {
    display: 'grid',
    gridTemplateColumns: '1fr 1fr',
    gap: '10px',
  },
  row3: {
    display: 'grid',
    gridTemplateColumns: '1fr 1fr 1fr',
    gap: '8px',
  },
  field: {
    display: 'flex',
    flexDirection: 'column',
    gap: '4px',
  },
  label: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '9px',
    letterSpacing: '0.1em',
    color: '#666',
    textTransform: 'uppercase',
  },
  inputWrapper: {
    display: 'flex',
    alignItems: 'center',
    border: '1px solid #1a1a1a',
    background: '#fff',
  },
  input: {
    flex: 1,
    border: 'none',
    padding: '8px 10px',
    fontSize: '14px',
    fontFamily: "'JetBrains Mono', monospace",
    fontWeight: 500,
    background: 'transparent',
    width: '100%',
    minWidth: 0,
  },
  select: {
    flex: 1,
    border: 'none',
    padding: '8px 10px',
    fontSize: '13px',
    fontFamily: "'JetBrains Mono', monospace",
    fontWeight: 500,
    background: 'transparent',
    cursor: 'pointer',
  },
  affix: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '12px',
    padding: '0 8px',
    color: '#666',
    background: '#f5f3ea',
    alignSelf: 'stretch',
    display: 'flex',
    alignItems: 'center',
  },
  hint: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '10px',
    color: '#ff6b35',
    marginTop: '4px',
    lineHeight: 1.4,
  },
  card: {
    background: '#fcfaf3',
    border: '1px solid #1a1a1a',
  },
  cardHeader: {
    padding: '18px 20px',
    borderBottom: '1px solid #1a1a1a',
    display: 'flex',
    justifyContent: 'space-between',
    alignItems: 'flex-start',
    gap: '20px',
  },
  cardIndex: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '10px',
    letterSpacing: '0.15em',
    color: '#ff6b35',
    marginBottom: '4px',
  },
  cardTitle: {
    fontSize: '22px',
    fontWeight: 800,
    letterSpacing: '-0.01em',
    lineHeight: 1.1,
  },
  cardSub: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '11px',
    color: '#666',
    marginTop: '6px',
  },
  cardMeta: {
    display: 'flex',
    gap: '20px',
    flexShrink: 0,
  },
  metaBlock: { textAlign: 'right' },
  metaLabel: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '9px',
    letterSpacing: '0.1em',
    color: '#666',
    marginBottom: '2px',
  },
  metaValue: {
    fontSize: '13px',
    fontWeight: 700,
  },
  bigNumberRow: {
    display: 'grid',
    gridTemplateColumns: '1fr 1fr 1fr 1fr',
    borderBottom: '1px solid #1a1a1a',
  },
  bigNum: {
    padding: '16px 18px',
    borderRight: '1px solid #1a1a1a',
  },
  bigNumPrimary: {
    background: '#fff8e7',
  },
  bigNumLabel: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '9px',
    letterSpacing: '0.1em',
    color: '#666',
    marginBottom: '6px',
  },
  bigNumValue: {
    fontSize: '22px',
    fontWeight: 800,
    fontVariantNumeric: 'tabular-nums',
    letterSpacing: '-0.02em',
  },
  breakdown: {
    display: 'grid',
    gridTemplateColumns: '1fr 1fr',
  },
  breakCol: {
    padding: '16px 20px',
    borderRight: '1px solid #1a1a1a',
  },
  breakTitle: {
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '10px',
    letterSpacing: '0.15em',
    color: '#666',
    marginBottom: '12px',
    fontWeight: 700,
  },
  breakLine: {
    display: 'flex',
    justifyContent: 'space-between',
    fontSize: '13px',
    padding: '3px 0',
    fontFamily: "'JetBrains Mono', monospace",
  },
  summary: {
    background: '#1a1a1a',
    color: '#e8e6dd',
  },
  summaryHeader: {
    padding: '14px 20px',
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '11px',
    letterSpacing: '0.15em',
    color: '#ff6b35',
    borderBottom: '1px solid #444',
  },
  table: {
    width: '100%',
    borderCollapse: 'collapse',
  },
  th: {
    textAlign: 'left',
    padding: '10px 20px',
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '10px',
    letterSpacing: '0.1em',
    color: '#aaa',
    fontWeight: 600,
    borderBottom: '1px solid #444',
  },
  thRight: {
    textAlign: 'right',
    padding: '10px 20px',
    fontFamily: "'JetBrains Mono', monospace",
    fontSize: '10px',
    letterSpacing: '0.1em',
    color: '#aaa',
    fontWeight: 600,
    borderBottom: '1px solid #444',
  },
  td: {
    padding: '12px 20px',
    fontSize: '13px',
    borderBottom: '1px solid #2a2a2a',
  },
  tdRight: {
    padding: '12px 20px',
    fontSize: '13px',
    textAlign: 'right',
    borderBottom: '1px solid #2a2a2a',
    fontFamily: "'JetBrains Mono', monospace",
    fontVariantNumeric: 'tabular-nums',
  },
  bestRow: {
    background: '#2a2a1a',
  },
  bestTag: {
    background: '#ff6b35',
    color: '#1a1a1a',
    fontSize: '9px',
    fontFamily: "'JetBrains Mono', monospace",
    fontWeight: 700,
    padding: '2px 6px',
    marginRight: '10px',
    letterSpacing: '0.1em',
  },
  footer: {
    background: '#fcfaf3',
    border: '1px solid #1a1a1a',
    padding: '16px 20px',
    fontSize: '11px',
    lineHeight: 1.6,
    color: '#444',
  },
  footerLine: {
    marginBottom: '8px',
  },
};
