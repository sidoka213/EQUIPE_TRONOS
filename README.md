<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>EQUIPE TRONOS ULTIMATE</title>
<style>
body {margin:0; font-family:Arial,sans-serif; background: linear-gradient(135deg,#0f2027,#203a43,#2c5364); color:#fff;}
#loginScreen, #tutorial {display:flex; flex-direction:column; align-items:center; justify-content:center; height:100vh;}
#mainApp {display:none; flex-direction:row; height:100vh;}
#menu {width:220px; background:rgba(0,0,0,.7); padding:20px; display:flex; flex-direction:column; gap:10px;}
#menu ul {list-style:none; padding:0;}
#menu ul li {margin-bottom:10px;}
#menu ul li a {color:#00c6ff; text-decoration:none; font-weight:bold;}
#content {flex:1; padding:20px; overflow-y:auto;}
.section {display:none;}
.section.active {display:block;}
button {padding:8px 12px; margin-top:5px; border:none; border-radius:8px; cursor:pointer; background:#00c6ff; color:#000; font-weight:bold;}
input, textarea {padding:8px; border-radius:6px; border:none; width:100%; margin-top:5px;}
h2,h3 {margin-top:0;}
.slide {background:#fff;color:#000;padding:15px;border-radius:10px;margin-bottom:10px;}
canvas {max-width:100%;}
#aiBtn {position:fixed;bottom:20px;right:20px;width:60px;height:60px;border-radius:50%;background:#00c6ff;font-size:30px;cursor:pointer;}
#aiBox {position:fixed;bottom:90px;right:20px;width:300px;background:#111;border-radius:12px;display:none;flex-direction:column;}
#aiBox header {background:#00c6ff;color:#000;padding:10px;border-radius:12px 12px 0 0;}
#aiMessages {padding:10px;height:200px;overflow-y:auto;font-size:14px;}
#aiInput {border:none;padding:10px;border-radius:0 0 12px 12px;}
</style>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
</head>
<body>

<!-- LOGIN -->
<div id="loginScreen">
  <h1>EQUIPE TRONOS ULTIMATE</h1>
  <input type="text" id="username" placeholder="Digite seu nome">
  <button onclick="login()">Entrar</button>
</div>

<!-- TUTORIAL -->
<div id="tutorial" style="display:none; background:rgba(0,0,0,.85); position:fixed; inset:0; z-index:999; align-items:center; justify-content:center;">
  <div style="background:#fff;color:#000;padding:20px;border-radius:12px;max-width:400px;text-align:center;">
    <h3>Bem-vindo 👋</h3>
    <p>Use o menu lateral para acessar slides, aulas, atividades, provas, currículos, jogos e galeria. Complete atividades para ganhar XP e desbloquear conquistas!</p>
    <button onclick="closeTutorial()">Começar</button>
  </div>
</div>

<!-- APP PRINCIPAL -->
<div id="mainApp">
  <aside id="menu">
    <ul>
      <li><a href="#" onclick="showSection('home')">Home</a></li>
      <li><a href="#" onclick="showSection('slides')">Slides</a></li>
      <li><a href="#" onclick="showSection('aulas')">Aulas</a></li>
      <li><a href="#" onclick="showSection('atividades')">Atividades</a></li>
      <li><a href="#" onclick="showSection('provas')">Provas</a></li>
      <li><a href="#" onclick="showSection('curriculos')">Currículos</a></li>
      <li><a href="#" onclick="showSection('perfil')">Perfil</a></li>
      <li><a href="#" onclick="showSection('jogos')">Jogos</a></li>
      <li><a href="#" onclick="showSection('galeria')">Galeria</a></li>
      <li><a href="#" onclick="showSection('horarios')">Horários</a></li>
    </ul>
  </aside>

  <section id="content">
    <!-- HOME -->
    <div id="home" class="section active">
      <h2>Bem-vindo, <span id="userDisplay"></span>!</h2>
      <p>Escolha uma opção no menu lateral para começar a estudar, criar slides ou se divertir com os jogos educativos!</p>
      <p>XP: <span id="userXP">0</span> | Conquistas: <span id="userBadges">0</span> | Ranking: <span id="userRank">#1</span></p>
    </div>

    <!-- SLIDES -->
    <div id="slides" class="section">
      <h2>Slides</h2>
      <textarea id="slideText" placeholder="Texto do slide"></textarea>
      <input type="file" id="slideImage" accept="image/*">
      <input id="labels" placeholder="Labels do gráfico, separados por vírgula">
      <input id="values" placeholder="Valores do gráfico, separados por vírgula">
      <button onclick="addSlide()">Adicionar Slide</button>
      <button onclick="startPresentation()">Modo Apresentação</button>
      <button onclick="exportPDF()">Exportar PDF</button>
      <div id="slidesContainer"></div>
    </div>

    <!-- AULAS -->
    <div id="aulas" class="section">
      <h2>Aulas</h2>
      <ul>
        <li>Matemática</li>
        <li>Português</li>
        <li>Ciências</li>
        <li>Filosofia</li>
        <li>História</li>
        <li>Geografia</li>
        <li>Educação Física</li>
        <li>Robótica</li>
        <li>Redação</li>
      </ul>
    </div>

    <!-- ATIVIDADES -->
    <div id="atividades" class="section">
      <h2>Atividades Interativas</h2>
      <p>Quanto é 5 + 7?</p>
      <input type="number" id="atividade1">
      <button onclick="checkAtividade()">Enviar</button>
      <p id="atividadeResult"></p>
    </div>

    <!-- PROVAS -->
    <div id="provas" class="section">
      <h2>Provas Semanais</h2>
      <p>Qual é a capital do Brasil?</p>
      <input type="text" id="prova1">
      <button onclick="checkProva()">Enviar</button>
      <p id="provaResult"></p>
    </div>

    <!-- CURRÍCULOS -->
    <div id="curriculos" class="section">
      <h2>Criador de Currículos</h2>
      <input type="text" id="nomeCurriculo" placeholder="Nome completo">
      <input type="text" id="cursoCurriculo" placeholder="Curso desejado">
      <textarea id="experienciaCurriculo" placeholder="Experiência"></textarea>
      <button onclick="exportCurriculo()">Exportar PDF</button>
    </div>

    <!-- PERFIL -->
    <div id="perfil" class="section">
      <h2>Perfil</h2>
      <p>Nome: <span id="perfilNome"></span></p>
      <p>XP: <span id="perfilXP">0</span></p>
      <p>Conquistas: <span id="perfilBadges">0</span></p>
      <p>Ranking: <span id="perfilRank">#1</span></p>
    </div>

    <!-- JOGOS -->
    <div id="jogos" class="section">
      <h2>Mini-jogos educativos</h2>
      <p>Em breve: jogos para testar conhecimentos e ganhar XP!</p>
    </div>

    <!-- GALERIA -->
    <div id="galeria" class="section">
      <h2>Galeria do grupo</h2>
      <input type="file" id="fotoGaleria" accept="image/*">
      <button onclick="addFotoGaleria()">Adicionar Foto</button>
      <div id="galeriaContainer"></div>
    </div>

    <!-- HORÁRIOS -->
    <div id="horarios" class="section">
      <h2>Horários Escolares</h2>
      <ul>
        <li><strong>Segunda:</strong> Libras, Ciências, Ciências, Recreio, História, Sócio emocional</li>
        <li><strong>Terça:</strong> Ciências, Ed física, Ed física, Recreio, Robótica, Espanhol</li>
        <li><strong>Quarta:</strong> Matemática, Filosofia, Ed financeira, Recreio, Matemática, Geografia</li>
        <li><strong>Quinta:</strong> Português, Português, Inglês, Recreio, Artes, Geografia</li>
        <li><strong>Sexta:</strong> Matemática, Matemática, Redação, Recreio, Ciências, Português</li>
      </ul>
    </div>

  </section>
</div>

<!-- IA ASSISTENTE -->
<button id="aiBtn" onclick="toggleAI()">🤖</button>
<div id="aiBox">
  <header>IA Tronos</header>
  <div id="aiMessages"></div>
  <input id="aiInput" placeholder="Digite sua dúvida..." onkeydown="if(event.key==='Enter') askAI()">
</div>

<script>
// TODO: Inclua todo o JavaScript do index.html anterior aqui (login, slides, aulas, provas, currículos, XP, galeria, IA)
</script>
</body>
</html>