<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Credit Fraud Detection — Visual Overview</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", sans-serif;
    background: #0d1117;
    color: #e6edf3;
    padding: 2rem;
  }
  h1 {
    font-size: 2.5rem;
    font-weight: 700;
    letter-spacing: -0.03em;
    margin-bottom: .25rem;
  }
  h1 span { color: #f0883e; }
  .subtitle {
    color: #8b949e;
    font-size: 1.05rem;
    margin-bottom: 2rem;
  }
  .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(340px,1fr)); gap: 1.25rem; margin-bottom: 2rem; }
  .card {
    background: #161b22;
    border: 1px solid #30363d;
    border-radius: 12px;
    padding: 1.25rem;
    transition: border-color .2s, box-shadow .2s;
  }
  .card:hover { border-color: #f0883e; box-shadow: 0 0 0 1px #f0883e33; }
  .card h2 {
    font-size: 1.1rem;
    font-weight: 600;
    margin-bottom: .75rem;
    display: flex;
    align-items: center;
    gap: .5rem;
  }
  .card h2 .badge {
    font-size: .65rem;
    background: #f0883e33;
    color: #f0883e;
    padding: .15rem .55rem;
    border-radius: 999px;
    font-weight: 500;
  }
  .card ul { list-style: none; }
  .card ul li {
    padding: .3rem 0;
    font-size: .9rem;
    color: #c9d1d9;
    border-bottom: 1px solid #21262d;
  }
  .card ul li:last-child { border: none; }
  .card ul li code {
    background: #0d1117;
    padding: .1rem .4rem;
    border-radius: 4px;
    font-size: .82rem;
    color: #58a6ff;
  }
  .card p, .card li { color: #c9d1d9; font-size: .9rem; line-height: 1.55; }
  .card li strong { color: #e6edf3; }

  .flow { margin: 2rem 0; }
  .flow h2 { margin-bottom: 1rem; font-size: 1.3rem; }
  .flow-row {
    display: flex; align-items: center; flex-wrap: wrap; gap: .5rem;
    background: #161b22; border: 1px solid #30363d; border-radius: 12px;
    padding: 1.25rem 1.5rem;
  }
  .flow-step {
    background: #0d1117; border: 1px solid #30363d; border-radius: 8px;
    padding: .5rem .9rem; font-size: .82rem; text-align: center; min-width: 80px;
  }
  .flow-step .fn { color: #f0883e; font-weight: 600; display: block; }
  .flow-step .pkg { color: #8b949e; font-size: .72rem; }
  .arrow { color: #484f58; font-size: 1.2rem; }

  .tree-wrap {
    background: #161b22; border: 1px solid #30363d; border-radius: 12px; padding: 1.25rem 1.5rem;
    font-family: "SF Mono", "Fira Code", "Cascadia Code", monospace; font-size: .85rem; line-height: 1.7;
  }
  .tree-wrap .dir { color: #58a6ff; }
  .tree-wrap .file { color: #c9d1d9; }
  .tree-wrap .desc { color: #8b949e; font-size: .78rem; margin-left: .75rem; }

  .columns { display: grid; grid-template-columns: 1fr 1fr; gap: 1.25rem; margin-top: 2rem; }
  @media (max-width:700px) { .columns { grid-template-columns: 1fr; } }

  .tag {
    display: inline-block; background: #f0883e22; color: #f0883e;
    font-size: .7rem; padding: .1rem .5rem; border-radius: 999px; margin-right: .3rem;
  }
  .tag-blue { background: #1f6feb33; color: #58a6ff; }

  .metric-row { display: flex; gap: 1rem; flex-wrap: wrap; margin: 1rem 0; }
  .metric {
    background: #161b22; border: 1px solid #30363d; border-radius: 10px;
    padding: 1rem 1.25rem; text-align: center; flex: 1; min-width: 120px;
  }
  .metric .val { font-size: 1.6rem; font-weight: 700; color: #f0883e; }
  .metric .lbl { font-size: .75rem; color: #8b949e; margin-top: .25rem; }

  /* ---- Mini-DB section ---- */
  .db-section { margin-top: 3rem; padding-top: 2rem; border-top: 1px solid #30363d; }
  .db-section h1 span { color: #58a6ff; }
  .db-section .flow-step .fn { color: #58a6ff; }
  .db-section .card:hover { border-color: #58a6ff; box-shadow: 0 0 0 1px #58a6ff33; }
  .db-section .badge { background: #1f6feb33; color: #58a6ff; }
  .db-section .tag { background: #1f6feb22; color: #58a6ff; }
  .db-section .highlight { color: #f0883e; }
  .db-section code { color: #f0883e; }
</style>
</head>
<body>

<h1>Credit Card <span>Fraud Detection</span></h1>
<div class="subtitle">Random Forest classifier deployed via Streamlit — detects fraudulent transactions in real-time</div>

<div class="metric-row">
  <div class="metric"><div class="val">4</div><div class="lbl">Python Files</div></div>
  <div class="metric"><div class="val">3</div><div class="lbl">Data Files</div></div>
  <div class="metric"><div class="val">~250</div><div class="lbl">Total Lines</div></div>
  <div class="metric"><div class="val">30</div><div class="lbl">Features (V1-V28, Time, Amount)</div></div>
  <div class="metric"><div class="val">50</div><div class="lbl">Random Forest Estimators</div></div>
</div>

<!---- Pipeline Flow ---->
<div class="flow">
  <h2>ML Pipeline</h2>
  <div class="flow-row">
    <div class="flow-step"><span class="fn">Download Data</span><span class="pkg">data/download_data.py</span></div>
    <span class="arrow">→</span>
    <div class="flow-step"><span class="fn">Train</span><span class="pkg">train_model.py</span></div>
    <span class="arrow">→</span>
    <div class="flow-step"><span class="fn">Model</span><span class="pkg">models/fraud_model.pkl</span></div>
    <span class="arrow">→</span>
    <div class="flow-step"><span class="fn">Predict</span><span class="pkg">predict.py</span></div>
    <span class="arrow">→</span>
    <div class="flow-step"><span class="fn">Streamlit UI</span><span class="pkg">app.py</span></div>
  </div>
</div>

<!---- Component Cards ---->
<div class="grid">
  <div class="card">
    <h2>Data Pipeline <span class="badge">data/</span></h2>
    <ul>
      <li><code>data/download_data.py</code> (70 lines) — Downloads from Kaggle (or GitHub mirror), or generates synthetic data as fallback</li>
      <li><code>data/creditcard.csv</code> — 284,807 transactions, ~492 frauds (0.17%)</li>
      <li><code>data/sample_batch.csv</code> — 20 sample transactions for batch testing</li>
    </ul>
  </div>

  <div class="card">
    <h2>Training <span class="badge">train_model.py</span></h2>
    <ul>
      <li><span class="tag">Model</span> Random Forest Classifier — 50 estimators, max_depth=12, balanced class weights</li>
      <li><span class="tag">Scaler</span> StandardScaler applied to <code>Amount</code> & <code>Time</code></li>
      <li><span class="tag">Sample</span> Trains on 492 fraud + 50,000 legitimate (random sample)</li>
      <li><span class="tag">Eval</span> Classification report, confusion matrix, ROC-AUC score</li>
      <li><code>models/fraud_model.pkl</code> & <code>models/scaler.pkl</code> persisted with joblib</li>
    </ul>
  </div>

  <div class="card">
    <h2>Prediction <span class="badge">predict.py</span></h2>
    <ul>
      <li><code>predict(features: dict) ⇒ dict</code> — Single transaction prediction</li>
      <li><code>predict_from_df(df) ⇒ DataFrame</code> — Batch CSV prediction</li>
      <li><span class="tag">Output</span> <code>is_fraud</code>, <code>fraud_probability</code>, <code>confidence</code></li>
      <li>Lazy-loads model on first call (singleton pattern)</li>
    </ul>
  </div>

  <div class="card">
    <h2>Web App <span class="badge">app.py</span></h2>
    <ul>
      <li><span class="tag-blue">Streamlit</span> Interactive UI with sidebar info panel</li>
      <li>Input sliders for all 30 features (V1-V28, Time, Amount)</li>
      <li><span class="tag-blue">✓ Legitimate</span> / <span class="tag">⚠ Fraud</span> result with confidence %</li>
      <li>Batch CSV uploader with download link for sample file</li>
    </ul>
  </div>
</div>

<!---- Project Structure ---->
<h2 style="margin:2rem 0 .75rem;">Project Structure</h2>
<div class="tree-wrap">
  <span class="dir">credit_fraud_detection/</span><br>
  <span class="dir">├ data/</span><br>
  <span class="dir">&boxv └ </span><span class="file">download_data.py</span><span class="desc">Downloader / synthetic data generator</span><br>
  <span class="dir">&boxv └ </span><span class="file">creditcard.csv</span><span class="desc">Kaggle dataset (284,807 transactions)</span><br>
  <span class="dir">&boxv └ </span><span class="file">sample_batch.csv</span><span class="desc">20 test transactions for batch upload</span><br>
  <span class="dir">├ models/</span><br>
  <span class="dir">&boxv └ </span><span class="file">fraud_model.pkl</span><span class="desc">Trained Random Forest (joblib)</span><br>
  <span class="dir">&boxv └ </span><span class="file">scaler.pkl</span><span class="desc">StandardScaler (joblib)</span><br>
  <span class="dir">├ </span><span class="file">train_model.py</span><span class="desc">Training script (81 lines)</span><br>
  <span class="dir">&boxv </span><span class="file">predict.py</span><span class="desc">Prediction module (60 lines)</span><br>
  <span class="dir">&boxv </span><span class="file">app.py</span><span class="desc">Streamlit web UI (108 lines)</span><br>
  <span class="dir">└ </span><span class="file">requirements.txt</span><span class="desc">streamlit, pandas, scikit-learn, imbalanced-learn, joblib, kagglehub</span><br>
</div>

<!---- Bottom Details ---->
<div class="columns">
  <div class="card">
    <h2>Dataset</h2>
    <ul>
      <li><strong>Source:</strong> Kaggle — European cardholders, Sept 2013</li>
      <li><strong>Size:</strong> 284,807 transactions, 492 fraud (0.172%)</li>
      <li><strong>Features:</strong> 28 PCA components (V1-V28) + Time + Amount</li>
      <li><strong>Target:</strong> Class (0 = legit, 1 = fraud)</li>
      <li><strong>Challenge:</strong> Highly imbalanced (99.83% legit)</li>
    </ul>
  </div>
  <div class="card">
    <h2>How to Run</h2>
    <ul>
      <li><code>pip install -r requirements.txt</code></li>
      <li><code>python data/download_data.py</code>  <span class="desc" style="color:#8b949e;font-size:.8rem"># get dataset</span></li>
      <li><code>python train_model.py</code>  <span class="desc" style="color:#8b949e;font-size:.8rem"># train + save model</span></li>
      <li><code>streamlit run app.py</code>  <span class="desc" style="color:#8b949e;font-size:.8rem"># launch UI</span></li>
      <li><span class="tag-blue">Single</span> Input features manually, click <strong>Check Transaction</strong></li>
      <li><span class="tag-blue">Batch</span> Upload CSV, run bulk predictions</li>
    </ul>
  </div>
</div>


<!==================== Mini-DB ====================>
<div class="db-section">

<h1>Mini <span>DB</span></h1>
<div class="subtitle">A SQL Database Engine Built From Scratch in Python — No external dependencies</div>

<!---- Architecture Flow ---->
<div class="flow">
  <h2>Architecture & Data Flow</h2>
  <div class="flow-row">
    <div class="flow-step"><span class="fn">cli.py</span><span class="pkg">REPL Shell</span></div>
    <span class="arrow">→</span>
    <div class="flow-step"><span class="fn">Lexer</span><span class="pkg">sql/lexer.py</span></div>
    <span class="arrow">→</span>
    <div class="flow-step"><span class="fn">Parser</span><span class="pkg">sql/parser.py</span></div>
    <span class="arrow">→</span>
    <div class="flow-step"><span class="fn">Planner</span><span class="pkg">engine/planner.py</span></div>
    <span class="arrow">→</span>
    <div class="flow-step"><span class="fn">Executor</span><span class="pkg">engine/executor.py</span></div>
    <span class="arrow">↓</span>
    <div style="display:flex;gap:.5rem;flex-wrap:wrap;">
      <div class="flow-step" style="border-color:#f0883e44"><span class="fn" style="color:#f0883e">Pages</span><span class="pkg">storage/page.py</span></div>
      <div class="flow-step" style="border-color:#f0883e44"><span class="fn" style="color:#f0883e">Disk I/O</span><span class="pkg">storage/disk_manager.py</span></div>
      <div class="flow-step" style="border-color:#f0883e44"><span class="fn" style="color:#f0883e">B+ Tree</span><span class="pkg">storage/bptree.py</span></div>
    </div>
  </div>
</div>

<!---- Component Cards ---->
<div class="grid">
  <div class="card">
    <h2>SQL Layer <span class="badge">3 modules</span></h2>
    <ul>
      <li><code>sql/lexer.py</code> (282 lines) — Tokenises SQL into keywords, identifiers, literals</li>
      <li><code>sql/parser.py</code> (338 lines) — Recursive-descent parser ⇒ AST</li>
      <li><code>sql/ast_nodes.py</code> (168 lines) — Typed AST nodes: <em>SelectNode, InsertNode</em>, etc.</li>
    </ul>
  </div>

  <div class="card">
    <h2>Engine <span class="badge">3 modules</span></h2>
    <ul>
      <li><code>engine/catalog.py</code> (110 lines) — JSON-persisted schema: tables, columns, types, PK</li>
      <li><code>engine/planner.py</code> (250 lines) — Validates queries, chooses <span class="highlight">index scan</span> vs <span class="highlight">full scan</span></li>
      <li><code>engine/executor.py</code> (474 lines) — Runs plans: CRUD, serialisation, WHERE evaluation</li>
    </ul>
  </div>

  <div class="card">
    <h2>Storage <span class="badge">3 modules</span></h2>
    <ul>
      <li><code>storage/page.py</code> (153 lines) — 4 KB fixed-size pages with length-prefixed records</li>
      <li><code>storage/disk_manager.py</code> (93 lines) — Binary I/O at <code>page_id × 4096</code> offsets</li>
      <li><code>storage/bptree.py</code> (407 lines) — B+ Tree with split propagation, range scans, superblock root</li>
    </ul>
  </div>

  <div class="card">
    <h2>Transactions <span class="badge">1 module</span></h2>
    <ul>
      <li><code>transaction/txn_manager.py</code> (70 lines) — Full-file snapshot: <em>BEGIN</em> copies  .db/.idx ⇒ .bak; <em>ROLLBACK</em> restores; <em>COMMIT</em> deletes backups</li>
    </ul>
  </div>

  <div class="card">
    <h2>CLI <span class="badge">1 module</span></h2>
    <ul>
      <li><code>cli.py</code> (207 lines) — Interactive REPL with multi-line input, formatted table output, SQL execution pipeline</li>
    </ul>
  </div>

  <div class="card">
    <h2>Tests <span class="badge">8 scripts</span></h2>
    <ul>
      <li><code>test_step1.py</code> — Page + DiskManager</li>
      <li><code>test_step2.py</code> — B+ Tree</li>
      <li><code>test_step3.py</code> — Lexer</li>
      <li><code>test_step4.py</code> — Parser + AST</li>
      <li><code>test_step5.py</code> — Catalog</li>
      <li><code>test_step6.py</code> — Planner</li>
      <li><code>test_step7.py</code> — Executor (end-to-end)</li>
      <li><code>test_step8.py</code> — Transactions</li>
    </ul>
  </div>
</div>

<!---- Project Structure ---->
<h2 style="margin:2rem 0 .75rem;">Project Structure</h2>
<div class="tree-wrap">
  <span class="dir">mini-db/</span><br>
  <span class="dir">├ engine/</span><br>
  <span class="dir">&boxv └ </span><span class="file">catalog.py</span><span class="desc">Schema persistence (JSON)</span><br>
  <span class="dir">&boxv └ </span><span class="file">planner.py</span><span class="desc">Query planning & validation</span><br>
  <span class="dir">&boxv └ </span><span class="file">executor.py</span><span class="desc">Plan execution & row I/O</span><br>
  <span class="dir">├ sql/</span><br>
  <span class="dir">&boxv └ </span><span class="file">lexer.py</span><span class="desc">Tokeniser</span><br>
  <span class="dir">&boxv └ </span><span class="file">parser.py</span><span class="desc">Recursive-descent parser</span><br>
  <span class="dir">&boxv └ </span><span class="file">ast_nodes.py</span><span class="desc">AST node classes</span><br>
  <span class="dir">├ storage/</span><br>
  <span class="dir">&boxv └ </span><span class="file">page.py</span><span class="desc">4 KB fixed-size pages</span><br>
  <span class="dir">&boxv └ </span><span class="file">disk_manager.py</span><span class="desc">Binary file I/O</span><br>
  <span class="dir">&boxv └ </span><span class="file">bptree.py</span><span class="desc">B+ Tree index</span><br>
  <span class="dir">├ transaction/</span><br>
  <span class="dir">&boxv └ </span><span class="file">txn_manager.py</span><span class="desc">Snapshot-based BEGIN/COMMIT/ROLLBACK</span><br>
  <span class="dir">└ </span><span class="file">cli.py</span><span class="desc">Interactive REPL</span><br>
  <span class="dir">└ </span><span class="file">README.md</span><span class="desc">Documentation (294 lines)</span><br>
  <span class="dir">└ </span><span class="file">test_step1.py .. test_step8.py</span><span class="desc">Step-by-step tests</span><br>
  <br>
  <span>Total: <strong>14 files</strong>, <strong>~2,634 lines</strong></span>
</div>

<!---- Bottom Details ---->
<div class="columns">
  <div class="card">
    <h2>SQL Dialect</h2>
    <ul>
      <li><strong>DDL:</strong> <code>CREATE TABLE</code> (INT, TEXT, VARCHAR, BOOL, PRIMARY KEY)</li>
      <li><strong>DML:</strong> <code>INSERT</code>, <code>SELECT</code> (with WHERE), <code>UPDATE</code>, <code>DELETE</code></li>
      <li><strong>Transactions:</strong> <code>BEGIN</code>, <code>COMMIT</code>, <code>ROLLBACK</code></li>
      <li><strong>Meta:</strong> <code>EXIT</code>, <code>QUIT</code></li>
    </ul>
  </div>
  <div class="card">
    <h2>Key Design Decisions</h2>
    <ul>
      <li><span class="tag">Page</span> Fixed 4 KB pages, length-prefixed records, tombstone deletes</li>
      <li><span class="tag">Index</span> B+ Tree with ~339-way leaf fanout, linked leaves for range scans</li>
      <li><span class="tag">Txn</span> File-snapshot isolation — simple, correct, single-user</li>
      <li><span class="tag">Types</span> INT, TEXT, VARCHAR, BOOL — binary serialisation with struct</li>
      <li><span class="tag">Dep</span> Zero external dependencies — stdlib only</li>
    </ul>
  </div>
</div>

</div>

</body>
</html>
