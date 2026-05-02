<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SENSIX PRO</title>
  <style>
    :root { --neon: #00ff9d; }
    * { margin:0; padding:0; box-sizing:border-box; }
    body {
      background: #0a0a1f;
      color: #e0ffe0;
      font-family: 'Inter', sans-serif;
      min-height: 100vh;
    }
    .container {
      max-width: 480px;
      margin: 0 auto;
      padding: 15px;
    }
    .header {
      text-align: center;
      margin: 20px 0 30px;
    }
    .header h1 {
      color: var(--neon);
      font-size: 29px;
      text-shadow: 0 0 20px var(--neon);
    }
    .card {
      background: #111827;
      border: 2px solid var(--neon);
      border-radius: 18px;
      padding: 22px;
      margin-bottom: 20px;
    }
    label {
      display: block;
      color: #a0ffe0;
      margin: 15px 0 6px;
      font-weight: 600;
    }
    input, select {
      width: 100%;
      padding: 14px;
      background: #1f2937;
      border: 1px solid var(--neon);
      border-radius: 10px;
      color: white;
      font-size: 16px;
    }
    button {
      width: 100%;
      padding: 16px;
      background: var(--neon);
      color: #000;
      border: none;
      border-radius: 12px;
      font-size: 17px;
      font-weight: bold;
      cursor: pointer;
      margin-top: 10px;
    }
    button.reset {
      background: #ff4444;
      color: white;
    }
    .result {
      background: #0f172a;
      border: 2px solid var(--neon);
      border-radius: 18px;
      padding: 20px;
      display: none;
    }
    .sens-item {
      display: flex;
      justify-content: space-between;
      padding: 14px 0;
      border-bottom: 1px solid #334155;
    }
    .tips {
      background: #1a2338;
      border: 1px solid var(--neon);
      border-radius: 14px;
      padding: 18px;
      margin-top: 20px;
      font-size: 14.5px;
      line-height: 1.6;
    }
  </style>
</head>
<body>

<div class="container">
  <div class="header">
    <h1>SENSIX PRO</h1>
    <p>Calibrador Personalizado Free Fire</p>
  </div>

  <div class="card">
    <label>Modelo do seu Celular</label>
    <input type="text" id="modelo" value="Sua Marca de Celular">

    <label>DPI da Tela</label>
    <input type="number" id="dpi" value="480" min="200" max="1250">

    <label>Tipo de Sensibilidade</label>
    <select id="tipo">
      <option value="alta">Alta Sensibilidade (Agressiva)</option>
      <option value="baixa">Baixa Sensibilidade (Precisa)</option>
    </select>

    <label>Quantos Dedos você joga?</label>
    <select id="dedos">
      <option value="2">2 Dedos</option>
      <option value="3" selected>3 Dedos (Claw)</option>
      <option value="4">4 Dedos</option>
    </select>

    <button onclick="gerarSensibilidade()">GERAR MINHA SENSIBILIDADE</button>
    <button onclick="reiniciarConfiguracoes()" class="reset">🔄 REINICIAR CONFIGURAÇÕES</button>
  </div>

  <div class="result" id="resultado">
    <h2 style="color:var(--neon); text-align:center; margin-bottom:15px;">✅ Sua Sensibilidade</h2>
    <div id="sensList"></div>
    <button onclick="copiarTudo()" style="background:#00ff9d;">📋 COPIAR CONFIGURAÇÃO</button>

    <div class="tips">
      <strong>Como Otimizar Seu Celular</strong><br><br>
      <strong>Configurações Obrigatórias:</strong><br>
      • Ative 120Hz (ou 90Hz) na taxa de atualização da tela<br>
      • Ative o Game Booster / Game Turbo<br>
      • Desative o Modo de Economia de Bateria<br><br>
      <strong>Configurações Recomendadas:</strong><br>
      • Feche todos os aplicativos em segundo plano<br>
      • Ative o Modo de Alto Desempenho<br>
      • Limpe a tela antes de jogar<br><br>
      <strong>Dicas Extras:</strong><br>
      • Use DPI entre 600 e 900 para melhor precisão<br>
      • Use uma capinha fina ou jogue sem capinha<br>
      • Experimente usar dedeira de silicone para melhor deslize
    </div>
  </div>
</div>

<script>
function gerarSensibilidade() {
  const dpi = parseInt(document.getElementById('dpi').value) || 480;
  const tipo = document.getElementById('tipo').value;
  const dedos = document.getElementById('dedos').value;
  const modelo = document.getElementById('modelo').value || "Seu Celular";

  let multiplier = tipo === "alta" ? 1.28 : 0.72;
  if (dedos === "4") multiplier *= 0.87;
  if (dedos === "2") multiplier *= 1.15;

  const sens = {
    geral: Math.min(200, Math.round(112 * (dpi / 480) * multiplier)),
    redDot: Math.min(200, Math.round(107 * (dpi / 480) * multiplier)),
    scope2x: Math.min(200, Math.round(97 * (dpi / 480) * multiplier)),
    scope4x: Math.min(200, Math.round(86 * (dpi / 480) * multiplier)),
    awm: Math.min(200, Math.round(65 * (dpi / 480) * multiplier)),
    freeLook: Math.min(200, Math.round(90 * (dpi / 480) * multiplier))
  };

  let html = `<p style="color:#88ffcc; margin-bottom:12px;">${modelo} • ${dedos} dedos</p>`;

  const nomes = {
    geral: "Geral",
    redDot: "Red Dot",
    scope2x: "Mira 2x",
    scope4x: "Mira 4x",
    awm: "Mira AWM",
    freeLook: "Olhadinha"
  };

  Object.keys(sens).forEach(key => {
    html += `
      <div class="sens-item">
        <span>${nomes[key]}</span>
        <strong>${sens[key]}</strong>
      </div>`;
  });

  document.getElementById('sensList').innerHTML = html;
  document.getElementById('resultado').style.display = 'block';
}

function reiniciarConfiguracoes() {
  document.getElementById('modelo').value = "Sua Marca de Celular";
  document.getElementById('dpi').value = "480";
  document.getElementById('tipo').value = "alta";
  document.getElementById('dedos').value = "3";
  
  document.getElementById('resultado').style.display = 'none';
  
  alert("✅ Configurações reiniciadas para o padrão!");
}

function copiarTudo() {
  const texto = document.getElementById('sensList').innerText;
  navigator.clipboard.writeText(texto);
  alert("✅ Configuração copiada com sucesso!");
}
</script>
</body>
</html>
