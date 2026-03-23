bot-ofertas/
 ├── bot.js
 ├── produtos.js
 ├── mensagens.js
 ├── config.json
 {
  "minDesconto": 20,
  "tempoEnvioMinutos": 60
}
function buscarProdutos() {
  return [
    {
      nome: "Smartphone Samsung Galaxy",
      preco: 1199,
      precoAntigo: 1599,
      link: "https://mercadolivre.com/...",
    },
    {
      nome: "Fone Bluetooth",
      preco: 99,
      precoAntigo: 199,
      link: "https://mercadolivre.com/...",
    }
  ];
}

function filtrarOfertas(produtos, minDesconto) {
  return produtos.filter(p => {
    const desconto = ((p.precoAntigo - p.preco) / p.precoAntigo) * 100;
    return desconto >= minDesconto;
  });
}

module.exports = { buscarProdutos, filtrarOfertas };
function gerarMensagem(produto) {
  return `🔥 OFERTA IMPERDÍVEL!

📦 ${produto.nome}
💰 De R$ ${produto.precoAntigo} por R$ ${produto.preco}

⚡ Corre antes que acabe!

👉 ${produto.link}`;
}

module.exports = { gerarMensagem };
const { default: makeWASocket, useMultiFileAuthState } = require("@whiskeysockets/baileys");
const { buscarProdutos, filtrarOfertas } = require("./produtos");
const { gerarMensagem } = require("./mensagens");
const config = require("./config.json");

async function iniciarBot() {
  const { state, saveCreds } = await useMultiFileAuthState("auth");

  const sock = makeWASocket({
    auth: state
  });

  sock.ev.on("creds.update", saveCreds);

  setInterval(async () => {
    const produtos = buscarProdutos();
    const ofertas = filtrarOfertas(produtos, config.minDesconto);

    for (let produto of ofertas) {
      const msg = gerarMensagem(produto);

      await sock.sendMessage("SEU_NUMERO@c.us", { text: msg });
    }

  }, config.tempoEnvioMinutos * 60 * 1000);
}

iniciarBot();
npm init -y
npm install @whiskeysockets/baileys
