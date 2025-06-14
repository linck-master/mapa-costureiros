<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mapa de Caça a Costureiros</title>
<link rel="icon" href="logo.png">
<style>
  body {font-family: Arial; margin:0; background:#f5f5f5;}
  header {background:#007bff; color:white; padding:10px; text-align:center;}
  nav {display:flex; justify-content:center; background:#eee;}
  nav button {margin:5px; padding:10px 20px; border:none; border-radius:5px; cursor:pointer;}
  nav button.active {background:#007bff; color:white;}
  .container {max-width:1000px; margin:auto; padding:20px; background:white; border-radius:10px;}
  .hidden {display:none;}
  canvas {border:2px solid #ccc; border-radius:10px; max-width:100%;}
  textarea {width:100%; height:100px; margin-top:10px; border-radius:8px; border:1px solid #ccc; padding:8px;}
  .tools {margin:10px 0;}
  .tools label {margin-right:10px;}
</style>
</head>
<body>
<header>
  <img src="logo.png" alt="Logo" style="width:50px; vertical-align:middle;">
  <span style="font-size:20px;">Mapa de Caça a Costureiros</span>
</header>

<nav>
  <button id="mapaBtn" class="active" onclick="showSection('mapa')">Mapa</button>
  <button id="planoBtn" onclick="showSection('plano')">Plano de Caça</button>
</nav>

<div class="container">
  <div id="mapaSection">
    <div class="tools">
      <label>Cor:
        <select id="color">
          <option value="#ff0000">Vermelho</option>
          <option value="#0000ff">Azul</option>
          <option value="#00aa00">Verde</option>
          <option value="#000000">Preto</option>
        </select>
      </label>
      <label>Espessura:
        <select id="size">
          <option value="2">Fino</option>
          <option value="5">Médio</option>
          <option value="8">Grosso</option>
        </select>
      </label>
    </div>
    <canvas id="canvas" width="1000" height="700"></canvas><br>
    <button onclick="clearCanvas()">Limpar</button>
    <button onclick="downloadCanvas()">Baixar</button>

    <h3>📋 Ficha de Anotação</h3>
    <textarea placeholder="Anote aqui: Nome, Endereço, Contato, Tipo de Costura, Impressão..."></textarea>
  </div>

  <div id="planoSection" class="hidden">
    <h2>🔎 PLANO AVANÇADO: MAPA DE CAÇA A COSTUREIROS ESCONDIDOS</h2>
    <p><strong>🎯 Objetivo:</strong> Mapear e localizar costureiros escondidos no seu bairro, rua, vielas, e até dentro das casas, mesmo os que não têm placa ou redes sociais.</p>
    <h3>1. 📍 Divida sua área em setores</h3>
    <p>Desenhe um raio de até 2 km e divida em setores menores por rua ou quadra. Ex.: "Rua A", "Travessa 1", "Fundos do mercado X".</p>
    <h3>2. 👀 Sinais visuais</h3>
    <ul>
      <li>🧵 Barulho de máquina</li>
      <li>👗 Varal estranho</li>
      <li>🧶 Pedaços de tecido</li>
      <li>📦 Caixas na calçada</li>
      <li>👨‍👩‍👧 Movimentação de sacolas</li>
    </ul>
    <h3>3. 🧭 Abordagem eficiente</h3>
    <p>"Oi, tudo bem? Tô procurando costureiras aqui do bairro pra fazer parceria..." Leve uma amostra pronta, seja direto e respeitoso.</p>
    <h3>4. 📋 Ficha rápida</h3>
    <ul>
      <li>Endereço, Nome, Contato, Tipo de Costura</li>
      <li>Impressão inicial, Vale testar ✔️❌</li>
    </ul>
    <h3>5. 👟 Dicas práticas</h3>
    <p>Ande simples, leve uma amostra e pergunte em padarias, pontos de ônibus, salões ou senhoras do bairro.</p>
    <h3>6. 💡 Ideias bônus</h3>
    <p>Use grupos de WhatsApp/Facebook, pergunte para conhecidos e comerciantes.</p>
    <h3>🧠 Mentalidade de Caça</h3>
    <p>Busque 5 testáveis para achar 1 ou 2 bons. Volte em quem estava ocupado. Persistência é chave.</p>
  </div>
</div>

<script>
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
const img = new Image();
img.src = 'mapa.png';
img.onload = () => ctx.drawImage(img, 0, 0, canvas.width, canvas.height);

let drawing = false;
canvas.addEventListener('mousedown', () => drawing = true);
canvas.addEventListener('mouseup', () => {drawing = false; ctx.beginPath();});
canvas.addEventListener('mouseout', () => drawing = false);
canvas.addEventListener('mousemove', draw);

function draw(event) {
  if (!drawing) return;
  const rect = canvas.getBoundingClientRect();
  ctx.lineWidth = document.getElementById('size').value;
  ctx.lineCap = 'round';
  ctx.strokeStyle = document.getElementById('color').value;
  ctx.lineTo(event.clientX - rect.left, event.clientY - rect.top);
  ctx.stroke();
  ctx.beginPath();
  ctx.moveTo(event.clientX - rect.left, event.clientY - rect.top);
}

function clearCanvas() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
}

function downloadCanvas() {
  const link = document.createElement('a');
  link.download = 'mapa-costureiros.png';
  link.href = canvas.toDataURL();
  link.click();
}

function showSection(section) {
  document.getElementById('mapaSection').classList.add('hidden');
  document.getElementById('planoSection').classList.add('hidden');
  document.getElementById(section + 'Section').classList.remove('hidden');
  document.getElementById('mapaBtn').classList.remove('active');
  document.getElementById('planoBtn').classList.remove('active');
  document.getElementById(section + 'Btn').classList.add('active');
}
</script>
</body>
</html>
