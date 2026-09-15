<div align="center">

# @astracode/byles-new

<img src="https://i.ibb.co.com/xSwJ0hgs/Wallpaper-Alchemy-Wallpaper-Naruto-Pedesaan-Tenang-4-K.jpg" alt="Banner" width="100%" style="border-radius: 10px;" />

**Library WhatsApp Baileys yang dimodifikasi dari @whiskeysockets/baileys v6.7.x**

[![NPM](https://img.shields.io/badge/npm-latest-red?style=for-the-badge&logo=npm)](https://www.npmjs.com/package/@astracode/byles-new)
[![Node](https://img.shields.io/badge/node-%3E%3D%2020-brightgreen?style=for-the-badge&logo=node.js)](https://nodejs.org)
[![Telegram](https://img.shields.io/badge/Telegram-@astracodex-blue?style=for-the-badge&logo=telegram)](https://t.me/astracodex)

> Simple • Cepat • Support Button Message • Siap Pakai

</div>

---

## 📖 Description

**@astracode/byles-new** adalah library WhatsApp **Baileys** yang telah dimodifikasi dari `@whiskeysockets/baileys` **v6.7.x**.

Library ini dirancang untuk mempermudah pembuatan bot WhatsApp dengan dukungan **Button Message** yang lengkap, seperti:

- 🔘 Button Copy
- 📋 Button List
- ⚡ Button Quick Reply
- 🔗 Button Link
- 📊 Polling Result
- 🛒 Button Product

Cocok digunakan untuk bot WhatsApp modern dengan tampilan interaktif dan pesan yang lebih kaya.

---

## ⚙️ Installation Package

### 📦 Via `package.json`

```json
{
  "dependencies": {
    "@astracode/byles-new": "latest"
  }
}
```

💻 Via Terminal

```bash
npm i @astracode/byles-new
```

---

📥 Import ESM & CJS

✅ ESM

```js
import { makeWASocket } from '@astracode/byles-new'
```

✅ CJS (Tested and working on Node.js 24)

```js
const { makeWASocket } = require('@astracode/byles-new')
```

---

🎛️ Features Button Message

📋 Button Copy

🔹 No Media

```js
await sock.sendMessage(jid, {
    interactiveMessage: {
        header: "Hello World",
        title: "Hello World",
        footer: "Hello World",
        buttons: [
            {
                name: "cta_copy",
                buttonParamsJson: JSON.stringify({
                    display_text: "Copy Text",
                    id: "1234567",
                    copy_code: "ABCDEFG"
                })
            }
        ]
    }
}, { quoted: m });
```

🔹 Pake Media

```js
await sock.sendMessage(jid, {
    interactiveMessage: {
        header: "Hello World",
        title: "Hello World",
        media: fs.readFileSync('./media/menu.jpg'),
        footer: "Hello World",
        buttons: [
            {
                name: "cta_copy",
                buttonParamsJson: JSON.stringify({
                    display_text: "Copy Text",
                    id: "1234567",
                    copy_code: "ABCDEFG"
                })
            }
        ]
    }
}, { quoted: m });
```

🔹 Tanpa Header/Title

```js
await sock.sendMessage(jid, {
    interactiveMessage: {
        caption: "Hello World!",
        footer: "Hello World",
        buttons: [
            {
                name: "cta_copy",
                buttonParamsJson: JSON.stringify({
                    display_text: "Copy Text",
                    id: "1234567",
                    copy_code: "ABCDEFG"
                })
            }
        ]
    }
}, { quoted: m });
```

---

📋 Button List Message

🔹 Interactive Button List

```js
await sock.sendMessage(jid, {
    interactiveMessage: {
        header: "Hallo World!",
        title: "Halo World!",
        footer: "Bot WhatsApp 2026",
        buttons: [
            {
                name: "single_select",
                buttonParamsJson: JSON.stringify({
                    title: "Hello World",
                    sections: [
                        {
                            title: "Title",
                            highlight_label: "Label",
                            rows: [
                                { title: "@astracode", description: "Ku Tak Percaya", id: "row_id" }
                            ]
                        }
                    ]
                })
            }
        ]
    }
}, { quoted: m });
```

🔹 Versi Biasa

```js
await sock.sendMessage(jid, {
    text: "Hello World",
    footer: "WhatsApp Bot 2026",
    buttonText: "Button List Message",
    sections: [
        {
            title: "@astracode",
            rows: [{ title: "Hello", description: "hallo", rowId: ".menu" }]
        }
    ]
}, { quoted: m });
```

---

⚡ Button Quick Reply

🔹 Versi Interactive

```js
await sock.sendMessage(jid, {
    interactiveMessage: {
        header: "Hallo World!",
        title: "Halo World!",
        footer: "Bot WhatsApp 2026",
        buttons: [
            {
                name: "quick_reply",
                buttonParamsJson: JSON.stringify({
                    display_text: "Klik Saya",
                    id: "hello_world"
                })
            }
        ]
    }
}, { quoted: m });
```

🔹 Versi Biasa

```js
await sock.sendMessage(jid, {
    text: "Hello World",
    footer: "WhatsApp Bot 2026",
    buttons: [
        { buttonId: ".menu", buttonText: { displayText: "Menu" }, type: 1 }
    ],
    headerType: 1
}, { quoted: m });
```

---

🔗 Button Link

```js
await sock.sendMessage(jid, {
    interactiveMessage: {
        header: "Hello World",
        title: "Halo World!",
        footer: "WhatsApp Bot 2026",
        nativeFlowMessage: {
            buttons: [
                {
                    name: "cta_url",
                    buttonParamsJson: JSON.stringify({
                        display_text: "🌐 Open Website",
                        url: "https://example.com",
                        merchant_url: "https://example.com"
                    })
                }
            ]
        }
    }
}, { quoted: m });
```

---

📊 Polling Result Message

```js
await sock.sendMessage(jid, {
    pollResultMessage: {
        name: "Hello World",
        pollVotes: [
            { optionName: "TEST 1", optionVoteCount: "112233" },
            { optionName: "TEST 2", optionVoteCount: "1" }
        ]
    }
}, { quoted: m });
```

---

🛒 Button Product Message

```js
await sock.sendMessage(jid, {
    productMessage: {
        title: "Produk Contoh",
        description: "Ini adalah deskripsi produk",
        thumbnail: { url: "https://example.com/image.jpg" },
        productId: "PROD001",
        retailerId: "RETAIL001",
        url: "https://example.com/product",
        body: "Detail produk",
        footer: "Harga spesial",
        priceAmount1000: 50000,
        currencyCode: "USD",
        buttons: [
            {
                name: "cta_url",
                buttonParamsJson: JSON.stringify({
                    display_text: "Beli Sekarang",
                    url: "https://example.com/buy"
                })
            }
        ]
    }
}, { quoted: m });
```

---

💬 Contact Support

<div align="center">

[![Telegram](https://img.shields.io/badge/Telegram-@astracodex-blue?style=for-the-badge&logo=telegram)](https://t.me/astracodex)

© 2026 @astracode — All Rights Reserved.

</div>
