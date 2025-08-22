<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Venda Produtos - Lupuleira Cervejaria</title>
<style>
body { font-family: Arial, sans-serif; background-color: #001f3f; color: #f1c40f; margin: 0; padding: 0; }
header { background-color: #001a35; padding: 20px; text-align: center; }
header h1 { margin: 0; color: #f1c40f; }
.container { padding: 20px; max-width: 1000px; margin: auto; display: flex; flex-wrap: wrap; gap: 20px; }
.card { background-color: #00264d; border-radius: 12px; padding: 20px; box-shadow: 0 4px 8px rgba(0,0,0,0.2); width: 250px; display: flex; flex-direction: column; align-items: center; gap: 10px; }
.card img { width: 180px; height: 180px; object-fit: cover; border-radius: 10px; border: 2px solid #f1c40f; cursor: pointer; }
.card h2 { color: #f1c40f; text-align: center; }
.card p { color: #ffffff; text-align: center; }
.placeholder-preco { background-color: #f1c40f; color: #001f3f; padding: 4px 8px; border-radius: 6px; font-weight: bold; display: inline-block; cursor: pointer; }
.pedido-btn, .remove-btn { display: inline-block; margin-top: 5px; padding: 8px 16px; font-weight: bold; border: none; border-radius: 6px; cursor: pointer; }
.pedido-btn { background-color: #f1c40f; color: #001f3f; }
.pedido-btn:hover { background-color: #ffd633; }
.remove-btn { background-color: #ff4d4d; color: #fff; margin-left: 5px; }
.remove-btn:hover { background-color: #ff1a1a; }
.add-product-btn { display: block; margin: 20px auto; padding: 12px 24px; font-size: 16px; font-weight: bold; background-color: #f1c40f; color: #001f3f; border: none; border-radius: 8px; cursor: pointer; }
.add-product-btn:hover { background-color: #ffd633; }

/* Modal */
.modal { display: none; position: fixed; z-index: 1000; left: 0; top: 0; width: 100%; height: 100%; overflow: auto; background-color: rgba(0,0,0,0.7); }
.modal-content { background-color: #00264d; margin: 100px auto; padding: 20px; border: 1px solid #888; width: 320px; border-radius: 12px; color: #fff; display: flex; flex-direction: column; gap: 10px; }
.modal-content input, .modal-content textarea { width: 100%; padding: 6px; border-radius: 6px; border: 1px solid #f1c40f; background-color: #001f3f; color: #f1c40f; }
.modal-content button { padding: 8px 16px; font-weight: bold; border: none; border-radius: 6px; cursor: pointer; }
.modal-content .save-btn { background-color: #f1c40f; color: #001f3f; }
.modal-content .save-btn:hover { background-color: #ffd633; }
.modal-content .cancel-btn { background-color: #ff4d4d; color: #fff; margin-top: 5px; }
.modal-content .cancel-btn:hover { background-color: #ff1a1a; }

/* Área de selecionar imagem */
#drop-area { border: 2px dashed #f1c40f; padding: 20px; text-align: center; border-radius: 8px; cursor: pointer; color: #f1c40f; font-weight: bold; }
#preview-img { width: 100%; max-height: 200px; object-fit: cover; border-radius: 8px; margin-top: 10px; display: none; }
</style>
</head>
<body>
<header>
<h1>Venda Produtos - Lupuleira Cervejaria 🍺</h1>
</header>

<div class="container" id="products">
  <!-- Produto inicial -->
  <div class="card">
    <img src="/images/pilsen.jpg" alt="Pilsen Puro Malte" onclick="editarImagem(this)">
    <h2>Pilsen Puro Malte</h2>
    <p>Uma clássica cerveja leve e refrescante, perfeita para qualquer ocasião.</p>
    <span class="placeholder-preco" onclick="editarPreco(this)">R$ 10,00</span>
    <div>
      <button class="pedido-btn" onclick="fazerPedido('Pilsen Puro Malte')">Pedir</button>
      <button class="remove-btn" onclick="removerProduto(this)">Remover</button>
    </div>
  </div>
</div>

<button class="add-product-btn" onclick="abrirModal()">Adicionar Produto</button>

<!-- Modal -->
<div id="modal" class="modal">
  <div class="modal-content">
    <h2>Adicionar Produto</h2>
    <input type="text" id="produtoNome" placeholder="Nome do produto">
    <textarea id="produtoDescricao" placeholder="Descrição do produto"></textarea>
    <input type="text" id="produtoPreco" placeholder="Preço (ex: R$ 10,00)">
    
    <div id="drop-area">
      <button type="button" onclick="inputImagem.click()">Selecionar Foto do PC ou Celular</button>
      <input type="file" id="produtoImagem" accept="image/*" style="display:none;" onchange="handleFile(this.files[0])">
      <img id="preview-img">
    </div>
    
    <button class="save-btn" onclick="salvarProduto()">Salvar Produto</button>
    <button class="cancel-btn" onclick="fecharModal()">Cancelar</button>
  </div>
</div>

<footer>
<p>Venda Produtos - Lupuleira Cervejaria © 2025 - Todos os direitos reservados</p>
</footer>

<script>
const modal = document.getElementById('modal');
const inputNome = document.getElementById('produtoNome');
const inputDescricao = document.getElementById('produtoDescricao');
const inputPreco = document.getElementById('produtoPreco');
const inputImagem = document.getElementById('produtoImagem');
const previewImg = document.getElementById('preview-img');

let selectedFile = null;

function abrirModal() {
  inputNome.value = '';
  inputDescricao.value = '';
  inputPreco.value = '';
  inputImagem.value = '';
  previewImg.src = '';
  previewImg.style.display = 'none';
  selectedFile = null;
  modal.style.display = 'block';
}

function fecharModal() {
  modal.style.display = 'none';
}

function handleFile(file) {
  if(!file) return;
  selectedFile = file;
  const reader = new FileReader();
  reader.onload = function(e) {
    previewImg.src = e.target.result;
    previewImg.style.display = 'block';
  };
  reader.readAsDataURL(file);
}

function salvarProduto() {
  const nome = inputNome.value.trim();
  const descricao = inputDescricao.value.trim();
  const preco = inputPreco.value.trim();
  if(!nome || !descricao || !preco || !selectedFile) {
    alert("Preencha todos os campos e selecione uma imagem!");
    return;
  }
  const reader = new FileReader();
  reader.onload = function(e) {
    adicionarProduto(nome, descricao, preco, e.target.result);
    fecharModal();
  };
  reader.readAsDataURL(selectedFile);
}

function adicionarProduto(nome, descricao, preco, imagemSrc) {
  const container = document.getElementById('products');
  const card = document.createElement('div');
  card.classList.add('card');

  const img = document.createElement('img');
  img.src = imagemSrc;
  img.alt = nome;
  img.onclick = function() { editarImagem(img); };

  const h2 = document.createElement('h2');
  h2.textContent = nome;

  const p = document.createElement('p');
  p.textContent = descricao;

  const spanPreco = document.createElement('span');
  spanPreco.classList.add('placeholder-preco');
  spanPreco.textContent = preco;
  spanPreco.onclick = function() { editarPreco(spanPreco); };

  const divBtns = document.createElement('div');
  const btnPedir = document.createElement('button');
  btnPedir.classList.add('pedido-btn');
  btnPedir.textContent = 'Pedir';
  btnPedir.onclick = function() { fazerPedido(nome); };

  const btnRemover = document.createElement('button');
  btnRemover.classList.add('remove-btn');
  btnRemover.textContent = 'Remover';
  btnRemover.onclick = function() { removerProduto(btnRemover); };

  divBtns.appendChild(btnPedir);
  divBtns.appendChild(btnRemover);

  card.appendChild(img);
  card.appendChild(h2);
  card.appendChild(p);
  card.appendChild(spanPreco);
  card.appendChild(divBtns);

  container.appendChild(card);
}

function editarPreco(element) {
  const novoPreco = prompt("Digite o novo preço:", element.textContent);
  if(novoPreco) element.textContent = novoPreco;
}

function editarImagem(imgElement) {
  const fileInput = document.createElement('input');
  fileInput.type = 'file';
  fileInput.accept = 'image/*';
  fileInput.onchange = function(e) {
    const file = e.target.files[0];
    if(!file) return;
    const reader = new FileReader();
    reader.onload = function(ev) {
      imgElement.src = ev.target.result;
    };
    reader.readAsDataURL(file);
  };
  fileInput.click();
}

function removerProduto(botao) {
  if(confirm("Deseja realmente remover este produto?")) {
    const card = botao.closest('.card');
    card.remove();
  }
}

function fazerPedido(produto) {
  const telefone = "5514996269554";
  const mensagem = encodeURIComponent(`Olá! Gostaria de pedir o produto: ${produto}`);
  window.open(`https://wa.me/${telefone}?text=${mensagem}`, "_blank");
}
</script>
</body>
</html>

