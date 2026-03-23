const axios = require("axios");

// Simulação de produto
function escolherProduto() {
  return {
    nome: "Smartphone Samsung",
    preco: "R$ 1.199",
    link: "https://mercadolivre.com/...",
  };
}

function gerarMensagem(produto) {
  return `🔥 OFERTA!

📱 ${produto.nome}
💰 ${produto.preco}

👉 Compre aqui: ${produto.link}`;
}

// Simulação envio
function enviarWhatsApp(msg) {
  console.log("Enviando:", msg);
}

const produto = escolherProduto();
const mensagem = gerarMensagem(produto);
enviarWhatsApp(mensagem);
