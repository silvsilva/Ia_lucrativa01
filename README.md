<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1.0">
  <title>ialuctativa</title>
  <style>
    body { font-family: Arial, sans-serif; margin:0; background: #f5f6fa; color:#222;}
    header { background: #232946; color: #fff; display: flex; align-items: center; justify-content: space-between; padding: 0.5rem 1rem; }
    .menu-btn { font-size: 2rem; cursor: pointer; background: none; border: none; color: #fff;}
    nav { display: none; flex-direction: column; position: absolute; top: 60px; left: 0; background: #232946; width: 200px; z-index: 10; border-radius:0 0 8px 0;}
    nav.active { display: flex;}
    nav a { color: #fff; text-decoration: none; padding: 1rem; border-bottom: 1px solid #445;}
    nav a:last-child { border-bottom: none;}
    .container { max-width: 400px; margin: 2rem auto; background: #fff; border-radius: 10px; box-shadow: 0 2px 12px #0001; padding: 2rem;}
    .hidden { display: none;}
    label { display: block; margin-top: 1rem;}
    input[type="email"], input[type="password"] { width: 100%; padding: 0.5rem; margin-top: 0.25rem; border: 1px solid #ccd; border-radius: 5px;}
    button, .edit-btn { background: #232946; color: #fff; border: none; padding: 0.7rem 1.2rem; border-radius: 5px; margin-top: 1rem; cursor: pointer;}
    .edit-btn { background: #eebbc3; color: #232946; margin-left: 1rem;}
    .ai-images { display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap; margin: 2rem 0;}
    .ai-images img { width: 100px; height: 100px; object-fit: cover; border-radius: 8px;}
    .content-section { margin: 2rem 0;}
    @media (max-width: 480px) {
      .container { max-width: 98vw; padding: 1rem;}
      .ai-images img { width: 80px; height: 80px;}
      header { flex-direction: column; align-items: start;}
    }
    .edit-mode [contenteditable] { border: 1px dashed #eebbc3; background: #fffceb;}
    .save-bar {position: fixed; bottom:0;left:0; right:0; background:#eebbc3; color:#232946; padding:1rem; display:none; justify-content:space-between;align-items:center; z-index:999;}
    .edit-mode .save-bar {display:flex;}
  </style>
</head>
<body>
  <header>
    <button class="menu-btn" id="menuBtn">&#9776;</button>
    <h1 id="siteTitle" contenteditable="false">ialuctativa</h1>
  </header>
  <nav id="sideMenu">
    <a href="#" data-section="inicio">Início</a>
    <a href="#" data-section="contenos">Conte-nos</a>
    <a href="#" data-section="ajuda">Ajuda</a>
    <a href="#" data-section="quemsomos">Quem somos</a>
  </nav>
  
  <div class="container" id="loginSection">
    <h2 contenteditable="false">Bem-vindo ao ialuctativa</h2>
    <div id="loginForm">
      <label>Email:
        <input type="email" id="loginEmail" placeholder="Digite seu email">
      </label>
      <label>Senha:
        <input type="password" id="loginPassword" placeholder="Digite sua senha">
      </label>
      <button onclick="login()">Entrar</button>
      <button onclick="showRegister()" style="background:#eebbc3;color:#232946;">Inscrever-se</button>
    </div>
    <div id="registerForm" class="hidden">
      <label>Email:
        <input type="email" id="registerEmail" placeholder="Digite seu email">
      </label>
      <label>Senha:
        <input type="password" id="registerPassword" placeholder="Digite sua senha">
      </label>
      <button onclick="register()">Registrar</button>
      <button onclick="showLogin()" style="background:#eebbc3;color:#232946;">Voltar</button>
    </div>
  </div>
  
  <div class="container hidden" id="mainPanel">
    <div class="content-section" id="inicioSection">
      <h2 contenteditable="false">Painel Inicial</h2>
      <div class="ai-images">
        <img src="https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=400&q=80" alt="IA 1">
        <img src="https://images.unsplash.com/photo-1465101162946-4377e57745c3?auto=format&fit=crop&w=400&q=80" alt="IA 2">
        <img src="https://images.unsplash.com/photo-1517694712202-14dd9538aa97?auto=format&fit=crop&w=400&q=80" alt="IA 3">
      </div>
      <p contenteditable="false">Bem-vindo ao painel de IA. Explore as funcionalidades e descubra como a inteligência artificial pode transformar o seu dia a dia!</p>
    </div>
    <div class="content-section hidden" id="contenosSection">
      <h2 contenteditable="false">Conte-nos</h2>
      <p contenteditable="false">Entre em contato para dúvidas, sugestões ou feedback. Estamos aqui para ajudar você a entender e utilizar IA de forma fácil.</p>
    </div>
    <div class="content-section hidden" id="ajudaSection">
      <h2 contenteditable="false">Ajuda</h2>
      <p contenteditable="false">Acesse nossos tutoriais, perguntas frequentes e suporte para tirar suas dúvidas sobre o funcionamento do painel de IA.</p>
    </div>
    <div class="content-section hidden" id="quemsomosSection">
      <h2 contenteditable="false">Quem Somos</h2>
      <p contenteditable="false">Somos apaixonados por tecnologia e inovação, e queremos democratizar o acesso à inteligência artificial.</p>
    </div>
    <button class="edit-btn" onclick="toggleEditMode()">Editar Conteúdo</button>
  </div>

  <div class="save-bar" id="saveBar">
    <span>Modo edição ativado. Salve para aplicar as mudanças.</span>
    <button onclick="saveEdits()">Salvar</button>
    <button onclick="toggleEditMode()" style="background: #232946;color:#fff;">Cancelar</button>
  </div>

  <script>
    // MENU
    const menuBtn = document.getElementById('menuBtn');
    const sideMenu = document.getElementById('sideMenu');
    menuBtn.onclick = () => sideMenu.classList.toggle('active');
    sideMenu.querySelectorAll('a').forEach(link => {
      link.onclick = e => {
        e.preventDefault();
        showSection(link.getAttribute('data-section'));
        sideMenu.classList.remove('active');
      }
    });

    // LOGIN & REGISTER (Simples, apenas local)
    function showRegister() {
      document.getElementById('loginForm').classList.add('hidden');
      document.getElementById('registerForm').classList.remove('hidden');
    }
    function showLogin() {
      document.getElementById('registerForm').classList.add('hidden');
      document.getElementById('loginForm').classList.remove('hidden');
    }
    function login() {
      // Apenas simula login (não armazena nada)
      document.getElementById('loginSection').classList.add('hidden');
      document.getElementById('mainPanel').classList.remove('hidden');
      showSection('inicio');
    }
    function register() {
      // Apenas simula registro
      alert("Registrado com sucesso! Faça login para continuar.");
      showLogin();
    }
    function showSection(section) {
      ['inicio','contenos','ajuda','quemsomos'].forEach(sec=>{
        document.getElementById(sec+'Section').classList.add('hidden');
      });
      document.getElementById(section+'Section').classList.remove('hidden');
    }

    // EDIT MODE
    let editMode = false, backup = {};
    function toggleEditMode() {
      editMode = !editMode;
      document.body.classList.toggle('edit-mode', editMode);
      document.querySelectorAll('[contenteditable]').forEach(el=>{
        el.setAttribute('contenteditable', editMode ? 'true' : 'false');
        if(editMode) backup[el.id||el.textContent] = el.innerHTML;
      });
      document.getElementById('saveBar').style.display = editMode ? 'flex' : 'none';
    }
    function saveEdits() {
      toggleEditMode();
      alert("Conteúdo editado! Para tornar permanente, clique em 'Compartilhar' no navegador e salve como PDF ou HTML.");
    }
    // Ao sair do modo edição sem salvar, restaura
    document.getElementById('saveBar').querySelector('button[style*="232946"]').onclick = () => {
      document.querySelectorAll('[contenteditable]').forEach(el=>{
        if(backup[el.id||el.textContent]) el.innerHTML = backup[el.id||el.textContent];
      });
      toggleEditMode();
    };

  </script>
</body>
</html>
