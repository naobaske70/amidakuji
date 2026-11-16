<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>あみだくじ</title>

<style>
  * {
    box-sizing: border-box;
  }

  body {
    margin: 0;
    min-height: 100vh;
    color: #173e59;
    font-family: "Hiragino Kaku Gothic ProN", "Meiryo", sans-serif;
    background: linear-gradient(135deg, #e4f8ff, #c7efff, #e8f7ff);
  }

  main {
    max-width: 1200px;
    margin: 24px auto;
    padding: 24px;
    background: rgba(255, 255, 255, 0.96);
    border: 3px solid white;
    border-radius: 24px;
    box-shadow: 0 12px 35px rgba(35, 120, 160, 0.15);
  }

  h1 {
    margin: 0 0 24px;
    padding: 18px;
    color: white;
    text-align: center;
    border-radius: 16px;
    background: linear-gradient(100deg, #32b7e3, #63cfee, #40a5db);
    box-shadow: 0 5px 0 #a7dcef;
  }

  h2 {
    margin-top: 28px;
    padding: 10px 14px;
    color: #17668b;
    border-left: 7px solid #62c9eb;
    border-radius: 6px;
    background: #eaf8ff;
  }

  h3 {
    color: #287b9f;
  }

  input,
  select,
  button {
    font: inherit;
    padding: 9px 10px;
    border: 2px solid #bbdfed;
    border-radius: 10px;
  }

  input,
  select {
    color: #173e59;
    background: white;
  }

  input:focus,
  select:focus,
  button:focus-visible {
    outline: 3px solid #50bfe7;
    outline-offset: 2px;
  }

  input[type="number"] {
    width: 90px;
  }

  button {
    cursor: pointer;
    color: #17668b;
    font-weight: bold;
    background: #e6f7ff;
    transition:
      transform 0.15s,
      box-shadow 0.15s,
      background 0.15s;
  }

  button:hover:not(:disabled) {
    transform: translateY(-2px);
    background: #ccefff;
    box-shadow: 0 4px 0 #adddec;
  }

  button:active:not(:disabled) {
    transform: translateY(1px);
    box-shadow: none;
  }

  button:disabled {
    cursor: not-allowed;
    opacity: 0.5;
  }

  .primary {
    color: white;
    border-color: #289ecb;
    background: linear-gradient(100deg, #38b9e3, #278fc9);
    box-shadow: 0 4px 0 #a3d8e9;
  }

  .primary:hover:not(:disabled) {
    color: white;
    background: linear-gradient(100deg, #219fc9, #1879b2);
  }

  .row {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    align-items: center;
    margin: 8px 0;
  }

  .name-row label {
    min-width: 95px;
  }

  .error {
    min-height: 1.5em;
    color: #c42b44;
    font-weight: bold;
  }

  .hint {
    color: #50758a;
    font-size: 0.9em;
  }

  .board-wrap {
    overflow-x: auto;
    margin: 16px 0;
    background: white;
    border: 3px solid #ccecf8;
    border-radius: 14px;
  }

  svg {
    display: block;
  }

  /* 参加者の配置画面 */
  .position-board {
    width: max-content;
  }

  .position-selects {
    display: flex;
  }

  .position-cell {
    flex: 0 0 150px;
    width: 150px;
    padding: 10px 5px 0;
    text-align: center;
  }

  .position-cell label {
    display: block;
    margin-bottom: 4px;
    color: #287b9f;
    font-size: 0.85em;
    font-weight: bold;
  }

  .position-cell select {
    width: 140px;
    max-width: 100%;
  }

  /* 結果画面 */
  .tabs {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    margin-top: 20px;
    border-bottom: 3px solid #a8deef;
  }

  .tab {
    border-radius: 12px 12px 0 0;
    border-bottom: 0;
  }

  .tab.active {
    color: white;
    border-color: #299dca;
    background: #299dca;
  }

  .person-buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    margin: 12px 0;
  }

  .person-button.active {
    color: white;
    border-color: #197cad;
    background: #197cad;
  }

  #selectedResult {
    padding: 12px;
    color: #17668b;
    font-weight: bold;
    border-radius: 10px;
    background: #e8f8ff;
  }

  table {
    border-collapse: collapse;
    margin: 16px 0;
  }

  th,
  td {
    padding: 8px 16px;
    border: 1px solid #c7e5ef;
    text-align: left;
    overflow-wrap: anywhere;
  }

  th {
    color: #17668b;
    background: #dff4fd;
  }

  tr:nth-child(even) td {
    background: #f4fbfe;
  }

  [hidden] {
    display: none !important;
  }

  /* 印刷用の内容は画面上では非表示 */
  #printArea {
    display: none;
  }

  @media (prefers-reduced-motion: reduce) {
    button {
      transition: none;
    }

    button:hover:not(:disabled) {
      transform: none;
    }
  }

  @media print {
    body {
      color: black;
      background: white;
    }

    main {
      display: none;
    }

    #printArea {
      display: block;
    }

    #printArea h1,
    #printArea h2 {
      padding: 0;
      color: black;
      text-align: left;
      background: none;
      border: 0;
      box-shadow: none;
    }

    #printArea table {
      width: 100%;
      margin: 10px 0;
    }

    #printArea th,
    #printArea td {
      padding: 6px;
      color: black;
      border: 1px solid black;
      background: white;
    }

    #printArea tr {
      break-inside: avoid;
      page-break-inside: avoid;
    }

    #printArea thead {
      display: table-header-group;
    }

    .print-board-section {
      break-before: page;
      page-break-before: always;
    }

    #printBoard svg {
      width: 100%;
      height: auto;
    }
  }
</style>
</head>

<body>

<main>
  <h1>あみだくじ</h1>

  <!-- 1. 参加者とゴールの設定 -->
  <section id="settings">
    <h2>1. 参加者とゴールの設定</h2>

    <label for="count">参加者の人数：</label>
    <input
      id="count"
      type="number"
      min="2"
      max="40"
      value="4"
    >

    <p class="hint">2〜40人で設定できます。</p>

    <h3>参加者の名前</h3>
    <div id="nameInputs"></div>

    <h3>ゴールの項目</h3>

    <p class="hint">
      項目名と個数を入力してください。
      個数の合計は参加者の人数と同じにしてください。
    </p>

    <div id="goalInputs"></div>

    <div class="row">
      <button id="addGoal" type="button">
        ゴールの項目を追加
      </button>

      <button id="numberGoals" type="button">
        ゴールを連番で設定する
      </button>
    </div>

    <p class="hint">
      連番で設定すると、現在のゴール項目を置き換え、
      1から参加人数までの数字を各1個ずつ設定します。
    </p>

    <p id="settingsError" class="error" role="alert"></p>

    <button id="finishSettings" class="primary" type="button">
      設定を完了する
    </button>
  </section>

  <!-- 2. 参加者の配置 -->
  <section id="positions" hidden>
    <h2>2. 参加者の位置を決める</h2>

    <p>
      縦線の上のプルダウンから参加者を選択してください。
    </p>

    <p class="hint">
      人数が多い場合は、横にスクロールできます。
    </p>

    <div class="row">
      <button id="randomPositions" type="button">
        参加者の位置をランダムに設定
      </button>
    </div>

    <div class="board-wrap">
      <div class="position-board">
        <div id="positionInputs" class="position-selects"></div>
        <div id="positionLines"></div>
      </div>
    </div>

    <p id="positionError" class="error" role="alert"></p>

    <div class="row">
      <button id="backSettings" type="button">
        設定に戻る
      </button>

      <button id="toLines" class="primary" type="button" disabled>
        次に進む
      </button>
    </div>
  </section>

  <!-- 3. 横線 -->
  <section id="lineStage" hidden>
    <h2>3. 横線を決める</h2>

    <p>横線はランダムに作成されています。</p>

    <div class="row">
      <button id="regenerate" type="button">
        横線をランダムに作り直す
      </button>

      <button
        id="toggleEdit"
        type="button"
        aria-pressed="false"
      >
        横線の追加
      </button>
    </div>

    <p class="hint">
      「横線の追加」を押すと、点線を選んで横線を追加できます。
      既存の横線を選ぶと削除できます。
    </p>

    <div id="lineBoard" class="board-wrap"></div>

    <div class="row">
      <button id="backPositions" type="button">
        参加者の位置に戻る
      </button>

      <button id="finishLines" class="primary" type="button">
        ゴールをランダムに設定して結果へ
      </button>
    </div>
  </section>

  <!-- 4. 結果 -->
  <section id="resultStage" hidden>
    <h2>4. 結果</h2>

    <div class="row">
      <button id="printResults" type="button">
        結果表とあみだくじを印刷
      </button>

      <button id="restart" type="button">
        最初からやり直す
      </button>
    </div>

    <p class="hint">
      印刷時は、全員の結果表とあみだくじの全体図が出力されます。
    </p>

    <div
      class="tabs"
      role="tablist"
      aria-label="結果の表示方法"
    >
      <button
        id="personTab"
        class="tab active"
        type="button"
        role="tab"
        aria-selected="true"
        aria-controls="personPanel"
      >
        人別に見る
      </button>

      <button
        id="tableTab"
        class="tab"
        type="button"
        role="tab"
        aria-selected="false"
        aria-controls="tablePanel"
      >
        結果の表を見る
      </button>
    </div>

    <!-- 人別の結果 -->
    <div
      id="personPanel"
      role="tabpanel"
      aria-labelledby="personTab"
    >
      <h3>参加者を選択</h3>

      <p class="hint">
        名前を押すと、選んだ人の経路を赤色で表示します。
        人数が多い場合は、あみだくじを横にスクロールできます。
      </p>

      <div id="personButtons" class="person-buttons"></div>

      <p id="selectedResult" aria-live="polite">
        参加者を選択してください。
      </p>

      <div id="resultBoard" class="board-wrap"></div>
    </div>

    <!-- 全員の結果表 -->
    <div
      id="tablePanel"
      role="tabpanel"
      aria-labelledby="tableTab"
      hidden
    >
      <h3>全員の結果</h3>
      <div id="resultTable"></div>
    </div>
  </section>
</main>

<!-- 印刷時だけ表示 -->
<section id="printArea">
  <h1>あみだくじ 結果一覧</h1>
  <div id="printTable"></div>

  <div class="print-board-section">
    <h2>あみだくじ 全体図</h2>
    <div id="printBoard"></div>
  </div>
</section>

<script>
"use strict";

const MAX_PEOPLE = 40;
const ROWS = 12;

/*
  配置画面のプルダウンと縦線を合わせるため、
  1人分の横幅を150pxにしています。
*/
const GAP = 150;
const X0 = GAP / 2;

const TOP = 45;
const FIRST_Y = 80;
const ROW_GAP = 30;
const BOTTOM = FIRST_Y + (ROWS - 1) * ROW_GAP + 45;

const SVG_NS = "http://www.w3.org/2000/svg";
const $ = id => document.getElementById(id);

let names = [];
let goalPool = [];
let positions = [];
let lines = [];
let finalGoals = [];
let results = [];

let editing = false;
let selectedPerson = -1;

/* ------------------------------
   共通処理
------------------------------ */

function showStage(id) {
  [
    "settings",
    "positions",
    "lineStage",
    "resultStage"
  ].forEach(stage => {
    $(stage).hidden = stage !== id;
  });

  window.scrollTo(0, 0);
}

function shuffle(array) {
  const copy = [...array];

  for (let i = copy.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [copy[i], copy[j]] = [copy[j], copy[i]];
  }

  return copy;
}

function xAt(column) {
  return X0 + column * GAP;
}

function yAt(row) {
  return FIRST_Y + row * ROW_GAP;
}

function boardWidth() {
  return positions.length * GAP;
}

function svgElement(tag, attributes = {}) {
  const element = document.createElementNS(SVG_NS, tag);

  for (const [key, value] of Object.entries(attributes)) {
    element.setAttribute(key, value);
  }

  return element;
}

function shortText(value) {
  const characters = Array.from(value);

  if (characters.length <= 9) {
    return value;
  }

  return characters.slice(0, 8).join("") + "…";
}

function svgText(svg, x, y, value) {
  const text = svgElement("text", {
    x,
    y,
    "text-anchor": "middle",
    "font-size": "13",
    fill: "#173e59"
  });

  text.textContent = shortText(value);

  const title = svgElement("title");
  title.textContent = value;
  text.append(title);

  svg.append(text);
}

/* ------------------------------
   参加者とゴールの設定
------------------------------ */

function updateNameInputs() {
  const oldValues = [
    ...$("nameInputs").querySelectorAll("input")
  ].map(input => input.value);

  const n = Number($("count").value);

  $("nameInputs").replaceChildren();

  if (!Number.isInteger(n) || n < 2 || n > MAX_PEOPLE) {
    return;
  }

  for (let i = 0; i < n; i++) {
    const row = document.createElement("div");
    row.className = "row name-row";

    const label = document.createElement("label");
    label.textContent = `参加者 ${i + 1}：`;
    label.htmlFor = `name-${i}`;

    const input = document.createElement("input");
    input.id = `name-${i}`;
    input.type = "text";
    input.placeholder = "名前";
    input.maxLength = 30;
    input.value = oldValues[i] || "";

    row.append(label, input);
    $("nameInputs").append(row);
  }
}

function addGoalRow(labelValue = "", countValue = 1) {
  const row = document.createElement("div");
  row.className = "row";

  const label = document.createElement("input");
  label.type = "text";
  label.placeholder = "ゴールの項目";
  label.maxLength = 30;
  label.value = labelValue;
  label.setAttribute("aria-label", "ゴールの項目");

  const count = document.createElement("input");
  count.type = "number";
  count.min = "1";
  count.max = String(MAX_PEOPLE);
  count.value = countValue;
  count.setAttribute("aria-label", "ゴールの個数");

  const unit = document.createElement("span");
  unit.textContent = "個";

  const remove = document.createElement("button");
  remove.type = "button";
  remove.textContent = "削除";
  remove.addEventListener("click", () => row.remove());

  row.append(label, count, unit, remove);
  $("goalInputs").append(row);
}

$("count").addEventListener("input", updateNameInputs);

$("addGoal").addEventListener("click", () => {
  addGoalRow();
});

/* ゴールを1から参加人数までの連番に設定 */
$("numberGoals").addEventListener("click", () => {
  const n = Number($("count").value);

  if (!Number.isInteger(n) || n < 2 || n > MAX_PEOPLE) {
    $("settingsError").textContent =
      `参加者の人数は2〜${MAX_PEOPLE}人で入力してください。`;
    return;
  }

  $("settingsError").textContent = "";
  $("goalInputs").replaceChildren();

  for (let i = 1; i <= n; i++) {
    addGoalRow(String(i), 1);
  }
});

$("finishSettings").addEventListener("click", () => {
  const error = $("settingsError");
  error.textContent = "";

  const n = Number($("count").value);

  if (!Number.isInteger(n) || n < 2 || n > MAX_PEOPLE) {
    error.textContent =
      `参加者の人数は2〜${MAX_PEOPLE}人で入力してください。`;
    return;
  }

  const enteredNames = [
    ...$("nameInputs").querySelectorAll("input")
  ].map(input => input.value.trim());

  if (
    enteredNames.length !== n ||
    enteredNames.some(name => !name)
  ) {
    error.textContent = "参加者の名前をすべて入力してください。";
    return;
  }

  if (new Set(enteredNames).size !== n) {
    error.textContent =
      "参加者の名前が重複しています。異なる名前を入力してください。";
    return;
  }

  const goalRows = [...$("goalInputs").children];

  if (goalRows.length === 0) {
    error.textContent = "ゴールの項目を1つ以上設定してください。";
    return;
  }

  const pool = [];

  for (const row of goalRows) {
    const inputs = row.querySelectorAll("input");
    const label = inputs[0].value.trim();
    const count = Number(inputs[1].value);

    if (
      !label ||
      !Number.isInteger(count) ||
      count < 1 ||
      count > MAX_PEOPLE
    ) {
      error.textContent =
        `ゴールの項目と、1〜${MAX_PEOPLE}の整数の個数を入力してください。`;
      return;
    }

    for (let i = 0; i < count; i++) {
      pool.push(label);
    }
  }

  if (pool.length !== n) {
    error.textContent =
      "全体の数が合いません。参加者の人数とゴールの合計を確認してください。";
    return;
  }

  names = enteredNames;
  goalPool = pool;
  positions = Array(n).fill("");

  renderPositions();
  showStage("positions");
});

/* ------------------------------
   参加者の配置
------------------------------ */

function renderPositions() {
  const container = $("positionInputs");
  container.replaceChildren();

  positions.forEach((selectedName, index) => {
    const cell = document.createElement("div");
    cell.className = "position-cell";

    const label = document.createElement("label");
    label.textContent = `縦線 ${index + 1}`;
    label.htmlFor = `position-${index}`;

    const select = document.createElement("select");
    select.id = `position-${index}`;

    const blank = document.createElement("option");
    blank.value = "";
    blank.textContent = "選択してください";
    select.append(blank);

    names.forEach(name => {
      const option = document.createElement("option");
      option.value = name;
      option.textContent = name;
      select.append(option);
    });

    select.value = selectedName;

    select.addEventListener("change", () => {
      positions[index] = select.value;
      validatePositions();
    });

    cell.append(label, select);
    container.append(cell);
  });

  renderPositionLines();
  validatePositions();
}

function renderPositionLines() {
  const width = boardWidth();
  const height = 240;

  const svg = svgElement("svg", {
    width,
    height,
    viewBox: `0 0 ${width} ${height}`,
    role: "img",
    "aria-label": "参加者を配置する縦線"
  });

  positions.forEach((_, index) => {
    const x = xAt(index);

    svg.append(svgElement("line", {
      x1: x,
      y1: 0,
      x2: x,
      y2: height - 15,
      stroke: "#345b70",
      "stroke-width": 2
    }));
  });

  $("positionLines").replaceChildren(svg);
}

function validatePositions() {
  const selected = positions.filter(Boolean);
  const duplicate = new Set(selected).size !== selected.length;
  const missing = selected.length !== positions.length;

  if (duplicate) {
    $("positionError").textContent =
      "参加者が重複しています。各参加者を1回ずつ選択してください。";
  } else if (missing) {
    $("positionError").textContent =
      "すべての縦線に参加者を選択してください。";
  } else {
    $("positionError").textContent = "";
  }

  $("toLines").disabled = duplicate || missing;
}

$("randomPositions").addEventListener("click", () => {
  positions = shuffle(names);
  renderPositions();
});

$("backSettings").addEventListener("click", () => {
  showStage("settings");
});

$("toLines").addEventListener("click", () => {
  if ($("toLines").disabled) {
    return;
  }

  generateLines();
  editing = false;
  updateEditButton();
  renderLineBoard();
  showStage("lineStage");
});

/* ------------------------------
   横線
------------------------------ */

function generateLines() {
  lines = Array.from(
    { length: ROWS },
    () => Array(positions.length - 1).fill(false)
  );

  for (let r = 0; r < ROWS; r++) {
    for (let c = 0; c < positions.length - 1; c++) {
      const leftIsEmpty = c === 0 || !lines[r][c - 1];

      if (leftIsEmpty && Math.random() < 0.4) {
        lines[r][c] = true;
      }
    }
  }
}

function canAddLine(r, c) {
  return (
    !lines[r][c] &&
    (c === 0 || !lines[r][c - 1]) &&
    (c === lines[r].length - 1 || !lines[r][c + 1])
  );
}

function updateEditButton() {
  $("toggleEdit").textContent = editing
    ? "横線の追加を終了"
    : "横線の追加";

  $("toggleEdit").setAttribute("aria-pressed", String(editing));
}

function renderLineBoard() {
  createBoard($("lineBoard"), { edit: editing });
}

$("regenerate").addEventListener("click", () => {
  generateLines();
  renderLineBoard();
});

$("toggleEdit").addEventListener("click", () => {
  editing = !editing;
  updateEditButton();
  renderLineBoard();
});

$("backPositions").addEventListener("click", () => {
  renderPositions();
  showStage("positions");
});

/* ------------------------------
   あみだくじの図
------------------------------ */

function createBoard(container, options = {}) {
  const {
    edit = false,
    showGoals = false,
    path = null
  } = options;

  const width = boardWidth();
  const height = BOTTOM + 65;

  const svg = svgElement("svg", {
    width,
    height,
    viewBox: `0 0 ${width} ${height}`,
    "aria-label": "あみだくじの図"
  });

  if (!edit) {
    svg.setAttribute("role", "img");
  }

  positions.forEach((name, column) => {
    const x = xAt(column);

    svgText(svg, x, 24, name);

    svg.append(svgElement("line", {
      x1: x,
      y1: TOP,
      x2: x,
      y2: BOTTOM,
      stroke: "#345b70",
      "stroke-width": 2
    }));

    if (showGoals) {
      svgText(svg, x, BOTTOM + 25, finalGoals[column]);
    }
  });

  for (let r = 0; r < ROWS; r++) {
    for (let c = 0; c < positions.length - 1; c++) {
      const exists = lines[r][c];
      const available = canAddLine(r, c);

      const x1 = xAt(c);
      const x2 = xAt(c + 1);
      const y = yAt(r);

      if (exists) {
        svg.append(svgElement("line", {
          x1,
          y1: y,
          x2,
          y2: y,
          stroke: "#345b70",
          "stroke-width": 2
        }));
      } else if (edit && available) {
        svg.append(svgElement("line", {
          x1,
          y1: y,
          x2,
          y2: y,
          stroke: "#62b9d9",
          "stroke-width": 2,
          "stroke-dasharray": "5 5"
        }));
      }

      if (edit && (exists || available)) {
        const hit = svgElement("line", {
          x1,
          y1: y,
          x2,
          y2: y,
          stroke: "transparent",
          "stroke-width": 20,
          tabindex: 0,
          role: "button",
          "aria-label":
            `${r + 1}段目、縦線${c + 1}と${c + 2}の間の横線を` +
            (exists ? "削除" : "追加")
        });

        hit.style.cursor = "pointer";

        const toggle = () => {
          lines[r][c] = !lines[r][c];
          renderLineBoard();
        };

        hit.addEventListener("click", toggle);

        hit.addEventListener("keydown", event => {
          if (event.key === "Enter" || event.key === " ") {
            event.preventDefault();
            toggle();
          }
        });

        svg.append(hit);
      }
    }
  }

  let animatedPath = null;

  if (path) {
    animatedPath = svgElement("path", {
      d: path,
      fill: "none",
      stroke: "#e52222",
      "stroke-width": 5,
      "stroke-linecap": "round",
      "stroke-linejoin": "round",
      "pointer-events": "none"
    });

    svg.append(animatedPath);
  }

  container.replaceChildren(svg);

  if (animatedPath) {
    const reduceMotion =
      window.matchMedia &&
      window.matchMedia("(prefers-reduced-motion: reduce)").matches;

    if (reduceMotion) {
      return;
    }

    const length = animatedPath.getTotalLength();

    animatedPath.style.strokeDasharray = String(length);
    animatedPath.style.strokeDashoffset = String(length);

    // 初期状態を描画してからアニメーションを開始
    animatedPath.getBoundingClientRect();

    requestAnimationFrame(() => {
      animatedPath.style.transition =
        "stroke-dashoffset 1s linear";

      animatedPath.style.strokeDashoffset = "0";
    });
  }
}

/* ------------------------------
   経路と結果
------------------------------ */

function trace(startColumn) {
  let column = startColumn;
  let d = `M ${xAt(column)} ${TOP}`;

  for (let r = 0; r < ROWS; r++) {
    const y = yAt(r);

    d += ` L ${xAt(column)} ${y}`;

    if (
      column < positions.length - 1 &&
      lines[r][column]
    ) {
      column++;
      d += ` L ${xAt(column)} ${y}`;
    } else if (
      column > 0 &&
      lines[r][column - 1]
    ) {
      column--;
      d += ` L ${xAt(column)} ${y}`;
    }
  }

  d += ` L ${xAt(column)} ${BOTTOM}`;

  return {
    path: d,
    goalColumn: column
  };
}

$("finishLines").addEventListener("click", () => {
  finalGoals = shuffle(goalPool);

  results = positions.map((name, index) => {
    const route = trace(index);

    return {
      name,
      goal: finalGoals[route.goalColumn],
      path: route.path
    };
  });

  selectedPerson = -1;

  showStage("resultStage");
  showResultTab("person");
  renderPersonView();
  renderResultTables();

  // 印刷用のあみだくじ全体図
  createBoard($("printBoard"), {
    showGoals: true
  });
});

/* ------------------------------
   結果タブ
------------------------------ */

function showResultTab(tab) {
  const personActive = tab === "person";

  $("personPanel").hidden = !personActive;
  $("tablePanel").hidden = personActive;

  $("personTab").classList.toggle("active", personActive);
  $("tableTab").classList.toggle("active", !personActive);

  $("personTab").setAttribute(
    "aria-selected",
    String(personActive)
  );

  $("tableTab").setAttribute(
    "aria-selected",
    String(!personActive)
  );
}

$("personTab").addEventListener("click", () => {
  showResultTab("person");
});

$("tableTab").addEventListener("click", () => {
  showResultTab("table");
});

function renderPersonView() {
  const container = $("personButtons");
  container.replaceChildren();

  results.forEach((result, index) => {
    const button = document.createElement("button");
    button.type = "button";
    button.className = "person-button";
    button.textContent = result.name;

    button.addEventListener("click", () => {
      selectedPerson = index;
      updateSelectedPerson();
    });

    container.append(button);
  });

  updateSelectedPerson();
}

function updateSelectedPerson() {
  const buttons = $("personButtons").querySelectorAll("button");

  buttons.forEach((button, index) => {
    const active = selectedPerson === index;
    button.classList.toggle("active", active);
    button.setAttribute("aria-pressed", String(active));
  });

  if (selectedPerson === -1) {
    $("selectedResult").textContent = "参加者を選択してください。";

    createBoard($("resultBoard"), {
      showGoals: true
    });

    return;
  }

  const result = results[selectedPerson];

  $("selectedResult").textContent =
    `${result.name} → ${result.goal}`;

  createBoard($("resultBoard"), {
    showGoals: true,
    path: result.path
  });
}

function createResultTable() {
  const table = document.createElement("table");

  const thead = document.createElement("thead");
  const headerRow = document.createElement("tr");

  ["参加者", "ゴール"].forEach(label => {
    const th = document.createElement("th");
    th.scope = "col";
    th.textContent = label;
    headerRow.append(th);
  });

  thead.append(headerRow);
  table.append(thead);

  const tbody = document.createElement("tbody");

  results.forEach(result => {
    const row = document.createElement("tr");

    const nameCell = document.createElement("td");
    nameCell.textContent = result.name;

    const goalCell = document.createElement("td");
    goalCell.textContent = result.goal;

    row.append(nameCell, goalCell);
    tbody.append(row);
  });

  table.append(tbody);

  return table;
}

function renderResultTables() {
  $("resultTable").replaceChildren(createResultTable());
  $("printTable").replaceChildren(createResultTable());
}

$("printResults").addEventListener("click", () => {
  window.print();
});

$("restart").addEventListener("click", () => {
  showStage("settings");
});

/* 初期表示 */
updateNameInputs();
addGoalRow("当たり", 1);
addGoalRow("はずれ", 3);
</script>

</body>
</html>
