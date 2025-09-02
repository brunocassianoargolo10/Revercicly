<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>ReverCicly</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f9f9f9;
      margin: 0;
      padding: 0;
    }

    header {
      background: #333;
      color: white;
      padding: 20px;
      text-align: center;
    }

    nav a {
      color: white;
      text-decoration: none;
      margin: 0 15px;
      font-weight: bold;
    }

    main {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 20px;
      padding: 20px;
    }

    .produto {
      background: white;
      border-radius: 10px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
      padding: 15px;
      text-align: center;
    }

    .produto img {
      max-width: 100%;
      border-radius: 8px;
    }

    .preco {
      font-size: 16px;
      font-weight: bold;
      margin: 10px 0;
      color: #28a745;
    }

    .btn {
      display: inline-block;
      background: #28a745;
      color: white;
      padding: 10px 15px;
      border-radius: 5px;
      text-decoration: none;
      cursor: pointer;
      transition: background 0.3s;
    }

    .btn:hover {
      background: #218838;
    }

    #carrinho, #checkout {
      background: white;
      padding: 20px;
      margin: 20px;
      border-radius: 10px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }

    #carrinho h2, #checkout h2 {
      margin-top: 0;
    }

    #lista-carrinho {
      list-style: none;
      padding: 0;
    }

    #lista-carrinho li {
      margin: 5px 0;
      padding: 8px;
      background: #f1f1f1;
      border-radius: 5px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    #total {
      font-weight: bold;
      margin-top: 10px;
    }

    footer {
      text-align: center;
      background: #333;
      color: white;
      padding: 15px;
      margin-top: 20px;
    }
  </style>
</head>
<body>

  <!-- ====== CABEÇALHO ====== -->
  <header>
    <h1>ReverCicly</h1>
    <nav>
      <a href="#">Início</a>
      <a href="#">Produtos</a>
      <a href="#carrinho">Carrinho</a>
    </nav>
  </header>

  <!-- ====== LISTA DE PRODUTOS ====== -->
  <main>
    <div class="produto">
      <img src="https://via.placeholder.com/200" alt="Produto 1">
      <h2>film strech colorido</h2>
      <p>Descrição rápida do produto.</p>
      <div class="preco" data-preco="99.90">R$ 99,90</div>
      <button class="btn">Comprar</button>
    </div>

    <div class="produto">
      <img src="https://via.placeholder.com/200" alt="Produto 2">
      <h2>film stcreh s/tubete 1,5 kl</h2>
      <p>Descrição rápida do produto.</p>
      <div class="preco" data-preco="149.90">R$ 149,90</div>
      <button class="btn">Comprar</button>
    </div>

    <div class="produto">
      <img src="https://via.placeholder.com/200" alt="Produto 3">
      <h2>film strech 25mm</h2>
      <p>Descrição rápida do produto.</p>
      <div class="preco" data-preco="79.90">R$ 79,90</div>
      <button class="btn">Comprar</button>
    </div>
  </main>

  <!-- ====== CARRINHO ====== -->
  <section id="carrinho">
    <h2>🛒 Carrinho de Compras</h2>
    <ul id="lista-carrinho"></ul>
    <p id="total">Total: R$ 0,00</p>
  </section>

  <!-- ====== CHECKOUT ====== -->
  <section id="checkout">
    <h2>💳 Formas de Pagamento</h2>

    <form id="form-pagamento">
      <label>
        <input type="radio" name="pagamento" value="debito" required>
        Cartão de Débito
      </label><br>

      <label>
        <input type="radio" name="pagamento" value="credito">
        Cartão de Crédito
      </label><br>

      <label>
        <input type="radio" name="pagamento" value="pix">
        Pix
      </label><br><br>

      <!-- Campos para cartão -->
      <div id="dados-cartao" style="display:none;">
        <input type="text" placeholder="Número do cartão" required><br><br>
        <input type="text" placeholder="Validade (MM/AA)" required><br><br>
        <input type="text" placeholder="CVV" required><br><br>
        <input type="text" placeholder="Nome impresso no cartão" required><br><br>

        <!-- Parcelamento -->
        <label>Parcelamento:</label><br>
        <select id="parcelas">
          <option value="1">1x sem juros</option>
          <option value="2">2x sem juros</option>
          <option value="3">3x sem juros</option>
          <option value="4">4x sem juros</option>
          <option value="5">5x sem juros</option>
          <option value="6">6x sem juros</option>
        </select><br><br>
      </div>

      <!-- Campos para Pix -->
      <div id="dados-pix" style="display:none;">
        <p>Escaneie o QR Code ou copie a chave Pix:</p>
        <img src="https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=11945845699" alt="QR Code Pix">
        <p><strong>Chave Pix:</strong> 11945845699</p>
      </div>

      <br>
      <button type="submit" class="btn">Finalizar Compra</button>
    </form>
  </section>

  <!-- ====== RODAPÉ ====== -->
  <footer>
    <p>&copy; 2025 Minha Primeira Loja. Todos os direitos reservados.</p>
  </footer>

  <!-- ====== JAVASCRIPT DO CARRINHO + PAGAMENTO ====== -->
  <script>
    const botoes = document.querySelectorAll(".btn");
    const listaCarrinho = document.getElementById("lista-carrinho");
    const totalSpan = document.getElementById("total");

    let total = 0;

    botoes.forEach((btn, index) => {
      btn.addEventListener("click", () => {
        const produto = btn.parentElement;
        const nome = produto.querySelector("h2").innerText;
        const preco = parseFloat(produto.querySelector(".preco").getAttribute("data-preco"));

        // adicionar no carrinho
        const li = document.createElement("li");
        li.textContent = `${nome} - R$ ${preco.toFixed(2)}`;

        // botão remover
        const remover = document.createElement("button");
        remover.textContent = "❌";
        remover.style.marginLeft = "10px";
        remover.style.cursor = "pointer";

        remover.addEventListener("click", () => {
          listaCarrinho.removeChild(li);
          total -= preco;
          atualizarTotal();
        });

        li.appendChild(remover);
        listaCarrinho.appendChild(li);

        // atualizar total
        total += preco;
        atualizarTotal();
      });
    });

    function atualizarTotal() {
      totalSpan.textContent = "Total: R$ " + total.toFixed(2);
    }

    // ====== PAGAMENTO ======
    const radios = document.querySelectorAll("input[name='pagamento']");
    const dadosCartao = document.getElementById("dados-cartao");
    const dadosPix = document.getElementById("dados-pix");
    const formPagamento = document.getElementById("form-pagamento");
    const parcelasSelect = document.getElementById("parcelas");

    radios.forEach(radio => {
      radio.addEventListener("change", () => {
        if (radio.value === "debito") {
          dadosCartao.style.display = "block";
          parcelasSelect.parentElement.style.display = "none"; // não mostra parcelas no débito
          dadosPix.style.display = "none";
        } else if (radio.value === "credito") {
          dadosCartao.style.display = "block";
          parcelasSelect.parentElement.style.display = "block"; // mostra parcelas no crédito
          dadosPix.style.display = "none";
        } else if (radio.value === "pix") {
          dadosPix.style.display = "block";
          dadosCartao.style.display = "none";
        }
      });
    });

    formPagamento.addEventListener("submit", (e) => {
      e.preventDefault();
      const metodo = document.querySelector("input[name='pagamento']:checked").value;

      if (metodo === "credito") {
        const qtdParcelas = parseInt(parcelasSelect.value);
        const valorParcela = total / qtdParcelas;
        alert("Pagamento selecionado: Crédito\n" +
              "Total: R$ " + total.toFixed(2) + "\n" +
              "Parcelado em " + qtdParcelas + "x de R$ " + valorParcela.toFixed(2));
      } else if (metodo === "debito") {
        alert("Pagamento selecionado: Débito\nTotal da compra: R$ " + total.toFixed(2));
      } else if (metodo === "pix") {
        alert("Pagamento selecionado: Pix\nChave Pix: 11945845699\nTotal da compra: R$ " + total.toFixed(2));
      }
    });
  </script>

</body>
</html>
