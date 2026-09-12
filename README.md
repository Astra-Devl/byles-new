<div align="center">

# 🌱 `@astracode/byles-new`

### ⚡ Simple · Fast · Powerful WhatsApp Bot Library ⚡

> *Underrated Baileys Fork — Built for Modern Developers*

<br />

<img src="https://i.ibb.co.com/xSwJ0hgs/Wallpaper-Alchemy-Wallpaper-Naruto-Pedesaan-Tenang-4-K.jpg" alt="Banner" width="100%" style="border-radius: 20px;" />

<br /><br />

<a href="https://www.npmjs.com/package/@astracode/byles-new">
  <img src="https://img.shields.io/npm/v/@astracode/byles-new?color=00c853&style=for-the-badge&logo=npm&labelColor=1a1a1a" />
</a>
<a href="https://www.npmjs.com/package/@astracode/byles-new">
  <img src="https://img.shields.io/npm/dm/@astracode/byles-new?color=2962ff&style=for-the-badge&logo=npm&labelColor=1a1a1a" />
</a>
<a href="https://github.com/Astra-Devl/byles-new">
  <img src="https://img.shields.io/github/stars/Astra-Devl/byles-new?color=ffab00&style=for-the-badge&logo=github&labelColor=1a1a1a" />
</a>
<a href="https://github.com/Astra-Devl/byles-new/blob/main/LICENSE">
  <img src="https://img.shields.io/github/license/Astra-Devl/byles-new?color=d500f9&style=for-the-badge&labelColor=1a1a1a" />
</a>

<br /><br />

<a href="https://github.com/Astra-Devl">
  <img src="https://img.shields.io/badge/GitHub-Astra--Devl-181717?style=for-the-badge&logo=github&logoColor=white" />
</a>
<a href="https://whatsapp.com/channel/0029VbCsoB0H5JLxG7J1CU31">
  <img src="https://img.shields.io/badge/WhatsApp-Channel-25D366?style=for-the-badge&logo=whatsapp&logoColor=white" />
</a>
<a href="https://www.npmjs.com/package/@astracode/byles-new">
  <img src="https://img.shields.io/badge/NPM-Package-cb3837?style=for-the-badge&logo=npm&logoColor=white" />
</a>

</div>

---

<div align="center">

## ✨ **Tentang Library**

**`@astracode/byles-new`** adalah library bot WhatsApp yang **simpel**, **cepat**, dan **kaya fitur**.
Dibuat dengan ❤️ oleh **AstraCode** — fork dari Baileys dengan berbagai peningkatan modern,
dukungan **ESM & CJS**, serta fitur-fitur canggih seperti **Rich Response**, **Native Flow**,
**Carousel**, **Album**, **Sticker Pack**, dan masih banyak lagi! 🚀

</div>

---

## 🎯 **Fitur Unggulan**

<div align="center">

| 🚀 Fitur | 📝 Deskripsi |
| :---: | :--- |
| ⚡ **Fast & Lightweight** | Performa tinggi, hemat resource |
| 🧩 **ESM & CJS Support** | Mendukung `import` dan `require` |
| 🎨 **Rich Response** | Pesan kaya dengan kode, tabel & link |
| 🔘 **Interactive Buttons** | Button, List, Native Flow, Carousel |
| 🖼️ **Media Lengkap** | Image, Video, Audio, Sticker, Album |
| 👥 **Group & Community** | Manajemen lengkap grup & komunitas |
| 📣 **Newsletter** | Kelola channel WhatsApp dengan mudah |
| 🛒 **Business Tools** | Produk, katalog, dan profil bisnis |
| 🔐 **Privacy Control** | Atur semua privasi akun |
| 📡 **Event System** | Event lengkap & terstruktur |

</div>

---

## 📥 **Installation**

<details>
<summary><b>📄 Via package.json — Klik untuk lihat</b></summary>

```json
{
  "dependencies": {
    "@astracode/byles-new": "latest"
  }
}
```

**Atau via GitHub:**

```json
{
  "dependencies": {
    "@astracode/byles-new": "github:Astra-Devl/byles-new"
  }
}
```

</details>

<details>
<summary><b>⌨️ Via Terminal — Klik untuk lihat</b></summary>

```bash
# NPM
npm i @astracode/byles-new

# GitHub
npm i github:Astra-Devl/byles-new
```

</details>

<details>
<summary><b>🧩 Import (ESM & CJS) — Klik untuk lihat</b></summary>

```javascript
// --- ESM
import { makeWASocket } from '@astracode/byles-new'

// --- CJS (tested and working on Node.js 24 ✅)
const { makeWASocket } = require('@astracode/byles-new')
```

</details>

---

## 🌐 **Connect to WhatsApp (Quick Step)**

<details>
<summary><b>⚡ Klik untuk lihat kode koneksi WhatsApp</b></summary>

```javascript
import { makeWASocket, delay, DisconnectReason, useMultiFileAuthState } from '@astracode/byles-new'
import { Boom } from '@hapi/boom'
import pino from 'pino'

// --- Connect with pairing code
const myPhoneNumber = '6288888888888'

const logger = pino({ level: 'silent' })

const connectToWhatsApp = async () => {
   const { state, saveCreds } = await useMultiFileAuthState('session')
    
   const sock = makeWASocket({
      logger,
      auth: state
   })

   sock.ev.on('creds.update', saveCreds)

   sock.ev.on('connection.update', (update) => {
      const { connection, lastDisconnect } = update
      if (connection === 'connecting' && !sock.authState.creds.registered) {
         await delay(1500)
         const code = await sock.requestPairingCode(myPhoneNumber)
         console.log('🔗 Pairing code', ':', code)
      }
      else if (connection === 'close') {
         const shouldReconnect = new Boom(connection?.lastDisconnect?.error)?.output?.statusCode !== DisconnectReason.loggedOut
         console.log('⚠️ Connection closed because', lastDisconnect.error, ', reconnecting ', shouldReconnect)
         if (shouldReconnect) {
            connectToWhatsApp()
         }
      }
      else if (connection === 'open') {
         console.log('✅ Successfully connected to WhatsApp')
      }
   })

   sock.ev.on('messages.upsert', async ({ messages }) => {
      for (const message of messages) {
         if (!message.message) continue

         console.log('🔔 Got new message', ':', message)
         await sock.sendMessage(message.key.remoteJid, {
            text: '👋🏻 Hello world'
         })
      }
   })
}

connectToWhatsApp()
```

</details>

### 🔐 Auth State

> [!NOTE]
> You can use the experimental `useSingleFileAuthState` and `useSqliteAuthState` as an alternative to `useMultiFileAuthState`. However, `useSingleFileAuthState` already includes an internal caching mechanism, so there is no need to wrap `state.keys` with `makeCacheableSignalKeyStore`.

---

## 🗄️ **Implementing Data Store**

> [!CAUTION]
> I highly recommend building your own data store, as keeping an entire chat history in memory can lead to excessive RAM usage.

<details>
<summary><b>📦 Klik untuk lihat kode Data Store</b></summary>

```javascript
import { makeWASocket, makeInMemoryStore, delay, DisconnectReason, useMultiFileAuthState } from '@astracode/byles-new'
import { Boom } from '@hapi/boom'
import pino from 'pino'

const myPhoneNumber = '6288888888888'

// --- Create your store path
const storePath = './store.json'

const logger = pino({ level: 'silent' })

const connectToWhatsApp = async () => {
   const { state, saveCreds } = await useMultiFileAuthState('session')
    
   const sock = makeWASocket({
      logger,
      auth: state
   })

   const store = makeInMemoryStore({
      logger,
      socket: sock
   })

   store.bind(sock.ev)

   sock.ev.on('creds.update', saveCreds)

   sock.ev.on('connection.update', (update) => {
      const { connection, lastDisconnect } = update
      if (connection === 'connecting' && !sock.authState.creds.registered) {
         await delay(1500)
         const code = await sock.requestPairingCode(myPhoneNumber)
         console.log('🔗 Pairing code', ':', code)
      }
      else if (connection === 'close') {
         const shouldReconnect = new Boom(connection?.lastDisconnect?.error)?.output?.statusCode !== DisconnectReason.loggedOut
         console.log('⚠️ Connection closed because', lastDisconnect.error, ', reconnecting ', shouldReconnect)
         if (shouldReconnect) {
            connectToWhatsApp()
         }
      }
      else if (connection === 'open') {
         console.log('✅ Successfully connected to WhatsApp')
      }
   })

   sock.ev.on('chats.upsert', () => {
      console.log('✉️ Got chats', store.chats.all())
   })

   sock.ev.on('contacts.upsert', () => {
      console.log('👥 Got contacts', Object.values(store.contacts))
   })

   // --- Read store from file
   store.readFromFile(storePath)

   // --- Save store every 3 minutes
   setInterval(() => {
      store.writeToFile(storePath)
   }, 180000)
}

connectToWhatsApp()
```

</details>

---

## 🪪 **WhatsApp IDs Explain**

<details>
<summary><b>📖 Klik untuk lihat penjelasan WhatsApp IDs</b></summary>

`id` is the WhatsApp ID, called `jid` and `lid` too, of the person or group you're sending the message to.

- It must be in the format `[country code][phone number]@s.whatsapp.net`
   - Example for people: `19999999999@s.whatsapp.net` and `12699999999@lid`.
   - For groups, it must be in the format `123456789-123345@g.us`.
- For Meta AI, it's `11111111111@bot`.
- For broadcast lists, it's `[timestamp of creation]@broadcast`.
- For stories, the ID is `status@broadcast`.

</details>

---

## ✉️ **Sending Messages**

> [!NOTE]
> You can get the `jid` from `message.key.remoteJid` in the first example.

<details>
<summary><b>🔠 Text — Klik untuk lihat kode</b></summary>

```javascript
// --- Send a regular text message
sock.sendMessage(jid, {
   text: '👋🏻 Hello'
}, {
   quoted: message
})

// --- Send a text message with a link preview
const urlA = 'https://www.npmjs.com/package/@astracode/byles-new'

sock.sendMessage(jid, {
   text: urlA + ' 👆🏻 Check it out!',
   linkPreview: {
      'matched-text': urlA,
      title: '🌱 @astracode/byles-new',
      description: 'Underrated Baileys Fork',
      previewType: 0,
      jpegThumbnail: fs.readFileSync('./path/to/image.jpg')
   }
})

// --- Send a text message with a large link preview and favicon
import { prepareWAMessageMedia } from '@astracode/byles-new'

const urlB = 'https://www.npmjs.com/package/@astracode/byles-new#readme'

const { imageMessage: image } = await prepareWAMessageMedia({
   image: {
      url: './path/to/image.jpg'
   }
}, {
   upload: sock.waUploadToServer,
   mediaTypeOverride: 'thumbnail-link'
})

// --- Set the thumbnail display size
image.height = 720
image.width = 480

sock.sendMessage(jid, {
   text: urlB + ' 👆🏻 Check it out!',
   linkPreview: {
      'matched-text': urlB,
      title: '🌱 @astracode/byles-new',
      description: 'Underrated Baileys Fork',
      previewType: 0,
      jpegThumbnail: fs.readFileSync('./path/to/image.jpg'),
      highQualityThumbnail: image,
      linkPreviewMetadata: {
         linkMediaDuration: 0,
         socialMediaPostType: 1,
      }
   },
   favicon: {
      url: './path/to/tiny-image.ico'
   }
})
```

</details>

<details>
<summary><b>🔔 Mention — Klik untuk lihat kode</b></summary>

```javascript
// --- Regular mention
sock.sendMessage(jid, {
   text: '👋🏻 Hello @628123456789',
   mentions: ['628123456789@s.whatsapp.net']
}, {
   quoted: message
})

// --- Mention all
sock.sendMessage(jid, {
   text: '👋🏻 Hello @all',
   mentionAll: true
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>😁 Reaction — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   react: {
      key: message.key,
      text: '✨'
   }
})
```

</details>

<details>
<summary><b>📌 Pin Message — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   pin: message.key,
   time: 86400, // --- Set the value in seconds: 86400 (1d), 604800 (7d), or 2592000 (30d)
   type: 1 // --- Or 2 to remove
})
```

</details>

<details>
<summary><b>🔖 Keep Chat — Klik untuk lihat kode</b></summary>

> [!NOTE]
> Keep Chat can only be used in chats or groups with disappearing messages enabled.

```javascript
sock.sendMessage(jid, {
   keep: message.key,
   type: 1 // --- Or 2 to remove
})
```

</details>

<details>
<summary><b>➡️ Forward Message — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   forward: message,
   force: true // --- Optional
})
```

</details>

<details>
<summary><b>👤 Contact — Klik untuk lihat kode</b></summary>

```javascript
const vcard = 'BEGIN:VCARD\n'
            + 'VERSION:3.0\n'
            + 'FN:Lia Wynn\n'
            + 'ORG:Waitress;\n'
            + 'TEL;type=CELL;type=VOICE;waid=628123456789:+62 8123 4567 89\n'
            + 'END:VCARD'

sock.sendMessage(jid, {
   contacts: {
      displayName: 'Lia Wynn',
      contacts: [
         { vcard }
      ]
   }
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>📍 Location — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   location: {
      degreesLatitude: 24.121231,
      degreesLongitude: 55.1121221,
      name: '👋🏻 I am here'
   }
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>🗓️ Event — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   event: {
      name: '🎶 Meet & Mingle Party',
      description: 'Meet & Mingle Party is a fun, casual gathering to connect, chat, and build new relationships within the community.',
      call: 'audio', // --- Or "video", this field is optional
      startDate: new Date(Date.now() + 3600000),
      endDate: new Date(Date.now() + 28800000),
      isCancelled: false,
      isScheduleCall: false,
      extraGuestsAllowed: false,
      location: {
         name: 'Jakarta',
         degreesLatitude: -6.2,
         degreesLongitude: 106.8
      }
   }
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>👥 Group Invite — Klik untuk lihat kode</b></summary>

```javascript
const inviteCode = groupUrl
   .split('chat.whatsapp.com/')[1]
   ?.split('?')[0]

const groupJid = '1201111111111@g.us'
const groupName = '@astracode/byles-new'

sock.sendMessage(jid, {
   groupInvite: {
      inviteCode,
      inviteExpiration: Date.now() + 86400000,
      text: '👋🏻 Hello, we invite you to join our group.',
      jid: groupJid,
      subject: groupName,
   }
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>🛍️ Product — Klik untuk lihat kode</b></summary>

```javascript
import { randomUUID } from 'crypto'

sock.sendMessage(jid, {
   image: {
      url: './path/to/image.jpg'
   },
   body: '👋🏻 Check my product here!',
   footer: '@astracode/byles-new',
   product: {
      currencyCode: 'IDR',
      description: '🛍️ Interesting product!',
      priceAmount1000: 70_000_000,
      productId: randomUUID(),
      productImageCount: 1,
      salePriceAmount1000: 65_000_000,
      signedUrl: 'https://www.npmjs.com/package/@astracode/byles-new',
      title: '📦 Starseed (Premium)',
      url: 'https://www.npmjs.com/package/@astracode/byles-new'
   },
   businessOwnerJid: '0@s.whatsapp.net'
})
```

</details>

<details>
<summary><b>📊 Poll — Klik untuk lihat kode</b></summary>

```javascript
// --- Regular poll message
sock.sendMessage(jid, {
   poll: {
      name: '🔥 Voting time',
      values: ['Yes', 'No'],
      selectableCount: 1,
      toAnnouncementGroup: false,
      endDate: new Date(Date.now() + 28800000),
      hideVoter: false,
      canAddOption: false
   }
}, {
   quoted: message
})

// --- Quiz (only for newsletter)
sock.sendMessage('1211111111111@newsletter', {
   poll: {
      name: '🔥 Quiz',
      values: ['Yes', 'No'],
      correctAnswer: 'Yes',
      pollType: 1
   }
}, {
   quoted: message
})

// --- Poll result
sock.sendMessage(jid, {
   pollResult: {
      name: '📝 Poll Result',
      votes: [{
         name: 'Nice',
         voteCount: 10
      }, {
         name: 'Nah',
         voteCount: 2
      }],
      pollType: 0
   }
}, {
   quoted: message
})

// --- Poll update
sock.sendMessage(jid, {
   pollUpdate: {
      metadata: {},
      key: message.key,
      vote: {
         enclv: /* <Buffer> */,
         encPayload: /* <Buffer> */
      }
   }
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>💭 Button Response — Klik untuk lihat kode</b></summary>

```javascript
// --- Using buttonsResponseMessage
sock.sendMessage(jid, {
   type: 'plain',
   buttonReply: {
      id: '#Menu',
      displayText: '✨ Interesting Menu'
   }
}, {
   quoted: message
})

// --- Using interactiveResponseMessage
sock.sendMessage(jid, {
   flowReply: {
      format: 0,
      text: '💭 Response',
      name: 'menu_options',
      paramsJson: JSON.stringify({
         id: '#Menu',
         description: '✨ Interesting Menu'
      })
   }
}, {
   quoted: message
})

// --- Using listResponseMessage
sock.sendMessage(jid, {
   listReply: {
      title: '📄 See More',
      description: '✨ Interesting Menu',
      id: '#Menu'
   }
}, {
   quoted: message
})

// --- Using templateButtonReplyMessage
sock.sendMessage(jid, {
   type: 'template',
   buttonReply: {
      id: '#Menu',
      displayText: '✨ Interesting Menu',
      index: 1
   }
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>✨ Rich Response — Klik untuk lihat kode</b></summary>

> [!NOTE]
> `richResponse[]` is a representation of `submessages[]` inside `richResponseMessage`.

```javascript
sock.sendMessage(jid, {
   disclaimerText: 'RAW submessages structure example',
   richResponse: [{
      text: 'Example Usage',
   }, {
      language: 'javascript',
      code: [{
         highlightType: 0,
         codeContent: 'console.log("Hello, World!")'
      }]
   }, {
      text: 'Pretty simple, right?\n'
   }, {
      text: 'Comparison between Node.js, Bun, and Deno',
   }, {
      title: 'Runtime Comparison',
      table: [{
         isHeading: true,
         items: ['', 'Node.js', 'Bun', 'Deno']
      }, {
         isHeading: false,
         items: ['Engine', 'V8 (C++)', 'JavaScriptCore (C++)', 'V8 (C++)']
      }, {
         isHeading: false,
         items: ['Performance', '4/5', '5/5', '4/5']
      }]
   }, {
      text: 'Does this help clarify the differences?'
   }]
})
```

> [!TIP]
> You can easily add syntax highlighting by importing `tokenizeCode` directly from Baileys.

```javascript
import { tokenizeCode } from '@astracode/byles-new'

const language = 'javascript'
const code = 'console.log("Hello, World!")'

sock.sendMessage(jid, {
   disclaimerText: 'Example of tokenizing Code Block',
   richResponse: [{
      text: 'Example Usage',
   }, {
      language,
      code: tokenizeCode(code, language)
   }, {
      text: 'Pretty simple, right?'
   }]
})
```

> 💡 Supported Languages: `css`, `html`, `javascript`, `typescript`, `python`, `golang`, `rust`, `c`, `c#`, `c++`, `bash`, `bat`, `powershell`.

</details>

<details>
<summary><b>🧾 Message with Code Block — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   disclaimerText: 'Code Block',
   headerText: '## Example Usage',
   contentText: '---',
   code: 'console.log("Hello, World!")',
   language: 'javascript',
   footerText: 'Pretty simple, right?'
})
```

</details>

<details>
<summary><b>🌏 Message with Inline Entities — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   disclaimerText: 'Inline Entities',
   headerText: '## Check Out!',
   contentText: '---',
   links: [{
      text: '1. Google',
      title: 'Popular Search Engine',
      url: 'https://www.google.com/'
   }, {
      text: '2. YouTube',
      title: 'Popular Streaming Platform',
      url: 'https://www.youtube.com/'
   }, {
      text: '3. Modded Baileys',
      title: 'Underrated Baileys Fork',
      url: 'https://www.npmjs.com/package/@astracode/byles-new'
   }],
   footerText: '---'
})
```

</details>

<details>
<summary><b>📋 Message with Table — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   disclaimerText: 'Table',
   headerText: '## Comparison between Node.js, Bun, and Deno',
   contentText: '---',
   title: 'Runtime Comparison',
   table: [
      ['', 'Node.js', 'Bun', 'Deno'],
      ['Engine', 'V8 (C++)', 'JavaScriptCore (C++)', 'V8 (C++)'],
      ['Performance', '4/5', '5/5', '4/5']
   ],
   noHeading: false,
   footerText: 'Does this help clarify the differences?'
})
```

</details>

<details>
<summary><b>🎞️ Status Mention — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage([jidA, jidB, jidC], {
   text: 'Hello! 👋🏻'
})
```

</details>

---

## 📁 **Sending Media Messages**

> [!NOTE]
> For media messages, you can pass a `Buffer` directly, or an object with either `{ stream: Readable }` or `{ url: string }` (local file path or HTTP/HTTPS URL).

<details>
<summary><b>🖼️ Image — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   image: {
      url: './path/to/image.jpg'
   },
   caption: '🔥 Superb'
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>🎥 Video — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   video: {
      url: './path/to/video.mp4'
   },
   gifPlayback: false,
   ptv: false,
   caption: '🔥 Superb'
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>📃 Sticker — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   sticker: {
      url: './path/to/sticker.webp'
   }
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>💽 Audio — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   audio: {
      url: './path/to/audio.mp3'
   },
   ptt: false
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>🗂️ Document — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   document: {
      url: './path/to/document.pdf'
   },
   mimetype: 'application/pdf',
   caption: '✨ My work!'
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>🖼️ Album (Image & Video) — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   album: [{
      image: {
         url: './path/to/image.jpg'
      },
      caption: '1st image'
   }, {
      video: {
         url: './path/to/video.mp4'
      },
      caption: '1st video'
   }, {
      image: {
         url: './path/to/image.jpg'
      },
      caption: '2nd image'
   }, {
      video: {
         url: './path/to/video.mp4'
      },
      caption: '2nd video'
   }]
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>📦 Sticker Pack — Klik untuk lihat kode</b></summary>

> [!IMPORTANT]
> If `sharp` or `@napi-rs/image` is not installed, the `cover` and `stickers` must already be in WebP format.

```javascript
sock.sendMessage(jid, {
   cover: {
      url: './path/to/image.webp'
   },
   stickers: [{
      data: {
         url: './path/to/image.webp'
      }
   }, {
      data: {
         url: './path/to/image.webp'
      }
   }, {
      data: {
         url: './path/to/image.webp'
      }
   }],
   name: '📦 My Sticker Pack',
   publisher: '🌟 Lia Wynn',
   description: '@astracode/byles-new'
}, {
   quoted: message
})
```

</details>

---

## 👉🏻 **Sending Interactive Messages**

<details>
<summary><b>🔘 Buttons — Klik untuk lihat kode</b></summary>

```javascript
// --- Regular buttons message
sock.sendMessage(jid, {
   text: '👆🏻 Buttons!',
   footer: '@astracode/byles-new',
   buttons: [{
      text: '👋🏻 SignUp',
      id: '#SignUp'
   }]
}, {
   quoted: message
})

// --- Buttons with Media & Native Flow
sock.sendMessage(jid, {
   image: {
      url: './path/to/image.jpg'
   },
   caption: '👆🏻 Buttons and Native Flow!',
   footer: '@astracode/byles-new',
   buttons: [{
      text: '👋🏻 Rating',
      id: '#Rating'
   }, {
      text: '📋 Select',
      sections: [{
         title: '✨ Section 1',
         rows: [{
            header: '',
            title: '💭 Secret Ingredient',
            description: '',
            id: '#SecretIngredient'
         }]
      }, {
         title: '✨ Section 2',
         highlight_label: '🔥 Popular',
         rows: [{
            header: '',
            title: '🏷️ Coupon',
            description: '',
            id: '#CouponCode'
         }]
      }]
   }]
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>📋 List — Klik untuk lihat kode</b></summary>

> [!NOTE]
> It only works in private chat (`@s.whatsapp.net`).

```javascript
sock.sendMessage(jid, {
   text: '📋 List!',
   footer: '@astracode/byles-new',
   buttonText: '📋 Select',
   title: '👋🏻 Hello',
   sections: [{
      title: '🚀 Menu 1',
      rows: [{
         title: '✨ AI',
         description: '',
         rowId: '#AI'
      }]
   }, {
      title: '🌱 Menu 2',
      rows: [{
         title: '🔍 Search',
         description: '',
         rowId: '#Search'
      }]
   }]
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>🗄️ Interactive — Klik untuk lihat kode</b></summary>

```javascript
// --- Native Flow
sock.sendMessage(jid, {
   image: {
      url: './path/to/image.jpg'
   },
   caption: '🗄️️ Interactive!',
   footer: '@astracode/byles-new',
   optionText: '👉🏻 Select Options',
   optionTitle: '📄 Select Options',
   offerText: '🏷️ Newest Coupon!',
   offerCode: '@astracode/byles-new',
   offerUrl: 'https://www.npmjs.com/package/@astracode/byles-new',
   offerExpiration: Date.now() + 3_600_000,
   nativeFlow: [{
      text: '👋🏻 Greeting',
      id: '#Greeting',
      icon: 'review'
   }, {
      text: '📞 Call',
      call: '628123456789'
   }, {
      text: '📋 Copy',
      copy: '@astracode/byles-new'
   }, {
      text: '🌐 Source',
      url: 'https://www.npmjs.com/package/@astracode/byles-new',
      useWebview: true
   }, {
      text: '📋 Select',
      sections: [{
         title: '✨ Section 1',
         rows: [{
            header: '',
            title: '🏷️ Coupon',
            description: '',
            id: '#CouponCode'
         }]
      }, {
         title: '✨ Section 2',
         highlight_label: '🔥 Popular',
         rows: [{
            header: '',
            title: '💭 Secret Ingredient',
            description: '',
            id: '#SecretIngredient'
         }]
      }],
      icon: 'default'
   }],
   interactiveAsTemplate: false,
}, {
   quoted: message
})

// --- Carousel & Native Flow
sock.sendMessage(jid, {
   text: '🗂️ Interactive with Carousel!',
   footer: '@astracode/byles-new',
   cards: [{
      image: {
         url: './path/to/image.jpg'
      },
      caption: '🖼️ Image 1',
      footer: '🏷️️ Pinterest',
      nativeFlow: [{
         text: '🌐 Source',
         url: 'https://www.npmjs.com/package/@astracode/byles-new',
         useWebview: true
      }]
   }, {
      image: {
         url: './path/to/image.jpg'
      },
      caption: '🖼️ Image 2',
      footer: '🏷️ Pinterest',
      offerText: '🏷️ New Coupon!',
      offerCode: '@astracode/byles-new',
      offerUrl: 'https://www.npmjs.com/package/@astracode/byles-new',
      offerExpiration: Date.now() + 3_600_000,
      nativeFlow: [{
         text: '🌐 Source',
         url: 'https://www.npmjs.com/package/@astracode/byles-new'
      }]
   }, {
      image: {
         url: './path/to/image.jpg'
      },
      caption: '🖼️ Image 3',
      footer: '🏷️ Pinterest',
      optionText: '👉🏻 Select Options',
      optionTitle: '👉🏻 Select Options',
      offerText: '🏷️ New Coupon!',
      offerCode: '@astracode/byles-new',
      offerUrl: 'https://www.npmjs.com/package/@astracode/byles-new',
      offerExpiration: Date.now() + 3_600_000,
      nativeFlow: [{
         text: '🛒 Product',
         id: '#Product',
         icon: 'default'
      }, {
         text: '🌐 Source',
         url: 'https://www.npmjs.com/package/@astracode/byles-new'
      }]
   }]
}, {
   quoted: message
})

// --- Native Flow with Audio in the Footer
sock.sendMessage(jid, {
   text: '🔈 Music in the footer!',
   audioFooter: {
      url: './path/to/audio.mp3'
   },
   nativeFlow: [{
      text: '👍🏻 Good, next',
      id: '#Next',
      icon: 'review'
   }, {
      text: '👎🏻 Skip',
      id: '#Skip',
      icon: 'default'
   }]
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>🫙 Hydrated Template — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   title: '👋🏻 Hello',
   image: {
      url: './path/to/image.jpg'
   },
   caption: '🫙 Template!',
   footer: '@astracode/byles-new',
   templateButtons: [{
      text: '👉🏻 Tap Here',
      id: '#Order'
   }, {
      text: '🌐 Source',
      url: 'https://www.npmjs.com/package/@astracode/byles-new'
   }, {
      text: '📞 Call',
      call: '628123456789'
   }]
}, {
   quoted: message
})
```

</details>

---

## 💳 **Sending Payment Messages**

<details>
<summary><b>➕ Invite Payment — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   paymentInviteServiceType: 3 // 1, 2, or 3
})
```

</details>

<details>
<summary><b>🧾 Invoice — Klik untuk lihat kode</b></summary>

> [!NOTE]
> Invoice message are not supported yet.

```javascript
sock.sendMessage(jid, {
   image: {
      url: './path/to/image.jpg'
   },
   invoiceNote: '🏷️ Invoice'
})
```

</details>

<details>
<summary><b>🛍️ Order — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(chat, {
   orderText: '🛍️ Order',
   thumbnail: fs.readFileSync('./path/to/image.jpg')
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>💳 Request Payment — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   text: '💳 Request Payment',
   requestPaymentFrom: '0@s.whatsapp.net'
})
```

</details>

---

## 👁️ **Other Message Options**

<details>
<summary><b>🤖 AI Icon — Klik untuk lihat kode</b></summary>

> [!NOTE]
> It only works in private chat (`@s.whatsapp.net`).

```javascript
sock.sendMessage(jid, {
   image: {
      url: './path/to/image.jpg'
   },
   caption: '🤖 With AI icon!',
   ai: true
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>🕒 Ephemeral — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   image: {
      url: './path/to/image.jpg'
   },
   caption: '👁️ Ephemeral',
   ephemeral: true
})
```

</details>

<details>
<summary><b>📰 External Ad Reply — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   text: '📰 External Ad Reply',
   externalAdReply: {
      title: '📝 Did you know?',
      body: '❓ I dont know',
      thumbnail: fs.readFileSync('./path/to/image.jpg'),
      largeThumbnail: false,
      url: 'https://www.npmjs.com/package/@astracode/byles-new'
   }
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>🧑‍🧑‍🧒 Group Status — Klik untuk lihat kode</b></summary>

> [!NOTE]
> It only works in group chat (`@g.us`)

```javascript
sock.sendMessage(jid, {
   image: {
      url: './path/to/image.jpg'
   },
   caption: '👥 Group Status!',
   groupStatus: true
})
```

</details>

<details>
<summary><b>🐱 Lottie Sticker — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   sticker: {
      url: './path/to/sticker.webp'
   },
   isLottie: true
})
```

</details>

<details>
<summary><b>🧩 Raw — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   extendedTextMessage: {
      text: '📃 Built manually from scratch using the raw WhatsApp proto structure',
      contextInfo: {
         externalAdReply: {
            title: '@astracode/byles-new',
            thumbnail: fs.readFileSync('./path/to/image.jpg'),
            sourceApp: 'whatsapp',
            showAdAttribution: true,
            mediaType: 1
         }
      }
   },
   raw: true
}, {
   quoted: message
})
```

</details>

<details>
<summary><b>🏷️ Secure Meta Service Label — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   text: '🏷️ Just a label!',
   secureMetaServiceLabel: true
})
```

</details>

<details>
<summary><b>📑 Spoiler — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   image: {
      url: './path/to/image.jpg'
   },
   caption: '❔ Spoiler',
   spoiler: true
})
```

</details>

<details>
<summary><b>👁️ View Once — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   image: {
      url: './path/to/image.jpg'
   },
   caption: '👁️ View Once',
   viewOnce: true
})
```

</details>

<details>
<summary><b>👁️ View Once V2 — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   image: {
      url: './path/to/image.jpg'
   },
   caption: '👁️ View Once V2',
   viewOnceV2: true
})
```

</details>

<details>
<summary><b>👁️ View Once V2 Extension — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   image: {
      url: './path/to/image.jpg'
   },
   caption: '👁️ View Once V2 Extension',
   viewOnceV2Extension: true
})
```

</details>

---

## ♻️ **Modify Messages**

<details>
<summary><b>🗑️ Delete Messages — Klik untuk lihat kode</b></summary>

```javascript
sock.sendMessage(jid, {
   delete: message.key
})
```

</details>

<details>
<summary><b>✏️ Edit Messages — Klik untuk lihat kode</b></summary>

```javascript
// --- Edit plain text
sock.sendMessage(jid, {
   text: '✨ I mean, nice!',
   edit: message.key
})

// --- Edit media messages caption
sock.sendMessage(jid, {
   caption: '✨ I mean, here is the image!',
   edit: message.key
})
```

</details>

---

## 🧰 **Additional Contents**

<details>
<summary><b>🏷️ Find User ID (JID|PN/LID) — Klik untuk lihat kode</b></summary>

```javascript
// --- PN (Phone Number)
const phoneNumber = '6281111111111@s.whatsapp.net'

const ids = await sock.findUserId(phoneNumber)

console.log('🏷️ Got user ID', ':', ids)

// --- LID (Local Identifier)
const lid = '43411111111111@lid'

const ids = await sock.findUserId(lid)

console.log('🏷️ Got user ID', ':', ids)
```

</details>

<details>
<summary><b>🔑 Request Custom Pairing Code — Klik untuk lihat kode</b></summary>

```javascript
const phoneNumber = '6281111111111'
const customPairingCode = 'STARFALL'

await sock.requestPairingCode(phoneNumber, customPairingCode)

console.log('🔗 Pairing code', ':', customPairingCode)
```

</details>

<details>
<summary><b>🖼️ Image Processing — Klik untuk lihat kode</b></summary>

```javascript
import { getImageProcessingLibrary } from '@astracode/byles-new'
import { readFile } from 'fs/promises'

const lib = await getImageProcessingLibrary()

const bufferOrFilePath = './path/to/image.jpg'
const width = 512

let output

// --- If sharp installed
if (lib.sharp?.default) {
   const img = lib.sharp.default(bufferOrFilePath)

   output = await img.resize(width)
      .jpeg({ quality: 80 })
      .toBuffer()
}

// --- If @napi-rs/image installed
else if (lib.image?.Transformer) {
   const inputBuffer = Buffer.isBuffer(bufferOrFilePath)
      ? bufferOrFilePath
      : await readFile(bufferOrFilePath)

   const img = new lib.image.Transformer(inputBuffer)

   output = await img.resize(width, undefined, 0)
      .jpeg(50)
}

// --- If jimp installed
else if (lib.jimp?.Jimp) {
   const img = await lib.jimp.Jimp.read(bufferOrFilePath)

   output = await img
      .resize({ w: width, mode: lib.jimp.ResizeStrategy.BILINEAR })
      .getBuffer('image/jpeg', { quality: 50 })
}

// --- Fallback
else {
   throw new Error('No image processing available')
}

console.log('✅ Process completed!')
console.dir(output, { depth: null })
```

</details>

<details>
<summary><b>📣 Newsletter Management — Klik untuk lihat kode</b></summary>

```javascript
// --- Create a new one
sock.newsletterCreate('@astracode/byles-new', '📣 Fresh updates weekly')

// --- Get info
const metadata = sock.newsletterMetadata('1231111111111@newsletter')
console.dir(metadata, { depth: null })

// --- Get subscribers count
const subscribers = await sock.newsletterSubscribers('1231111111111@newsletter')
console.dir(subscribers, { depth: null })

// --- Follow and Unfollow
sock.newsletterFollow('1231111111111@newsletter')
sock.newsletterUnfollow('1231111111111@newsletter')

// --- Mute and Unmute
sock.newsletterMute('1231111111111@newsletter')
sock.newsletterUnmute('1231111111111@newsletter')

// --- Demote admin
sock.newsletterDemote('1231111111111@newsletter', '6281111111111@s.whatsapp.net')

// --- Change owner
sock.newsletterChangeOwner('1231111111111@newsletter', '6281111111111@s.whatsapp.net')

// --- Update newsletter
sock.newsletterUpdate('1231111111111@newsletter', { name: '@astracode/byles-new' })

// --- Change name
sock.newsletterUpdateName('1231111111111@newsletter', '📦 @astracode/byles-new')

// --- Change description
sock.newsletterUpdateDescription('1231111111111@newsletter', '📣 Fresh updates weekly')

// --- Change photo
sock.newsletterUpdatePicture('1231111111111@newsletter', {
   url: 'path/to/image.jpg'
})

// --- Remove photo
sock.newsletterRemovePicture('1231111111111@newsletter')

// --- React to a message
sock.newsletterReactMessage('1231111111111@newsletter', '100', '💛')

// --- Get admin count
const count = await sock.newsletterAdminCount('1231111111111@newsletter')

// --- Get all subscribed newsletters
const newsletters = await sock.newsletterSubscribed()
console.dir(newsletters, { depth: null })

// --- Fetch newsletter messages
const messages = sock.newsletterFetchMessages('jid', '1231111111111@newsletter', 50, 0, 0)
console.dir(messages, { depth: null })

// --- Delete newsletter
sock.newsletterDelete('1231111111111@newsletter')
```

</details>

<details>
<summary><b>👥 Group Management — Klik untuk lihat kode</b></summary>

```javascript
// --- Create a new one and add participants using their JIDs
const group = sock.groupCreate('@astracode/byles-new', ['628123456789@s.whatsapp.net'])
console.dir(group, { depth: null })

// --- Get info
const metadata = await sock.groupMetadata(jid)
console.dir(metadata, { depth: null })

// --- Get group invite code
const inviteCode = await sock.groupInviteCode(jid)
console.dir(inviteCode, { depth: null })

// --- Revoke invite link
sock.groupRevokeInvite(jid)

// --- Accept group invite
sock.groupAcceptInvite(inviteCode)

// --- Leave group
sock.groupLeave(jid)

// --- Add participants
sock.groupParticipantsUpdate(jid, ['628123456789@s.whatsapp.net'], 'add')

// --- Remove participants
sock.groupParticipantsUpdate(jid, ['628123456789@s.whatsapp.net'], 'remove')

// --- Promote to admin
sock.groupParticipantsUpdate(jid, ['628123456789@s.whatsapp.net'], 'promote')

// --- Demote from admin
sock.groupParticipantsUpdate(jid, ['628123456789@s.whatsapp.net'], 'demote')

// --- Accept join requests
sock.groupRequestParticipantsUpdate(jid, ['628123456789@s.whatsapp.net'], 'approve')

// --- Change name
sock.groupUpdateSubject(jid, '📦 @astracode/byles-new')

// --- Change description
sock.groupUpdateDescription(jid, 'Updated description')

// --- Change photo
sock.updateProfilePicture(jid, {
   url: 'path/to/image.jpg'
})

// --- Remove photo
sock.removeProfilePicture(jid)

// --- Set group as admin only for chatting
sock.groupSettingUpdate(jid, 'announcement')

// --- Set group as open to all for chatting
sock.groupSettingUpdate(jid, 'not_announcement')

// --- Set admin only can edit group info
sock.groupSettingUpdate(jid, 'locked')

// --- Set all participants can edit group info
sock.groupSettingUpdate(jid, 'unlocked')

// --- Set admin only can add participants
sock.groupMemberAddMode(jid, 'admin_add')

// --- Set all participants can add participants
sock.groupMemberAddMode(jid, 'all_member_add')

// --- Enable or disable temporary messages with seconds format
sock.groupToggleEphemeral(jid, 86400)

// --- Disable temporary messages
sock.groupToggleEphemeral(jid, 0)

// --- Enable or disable membership approval mode
sock.groupJoinApprovalMode(jid, 'on')
sock.groupJoinApprovalMode(jid, 'off')

// --- Get all groups metadata
const groups = await sock.groupFetchAllParticipating()
console.dir(groups, { depth: null })

// --- Get pending join requests
const requests = await sock.groupRequestParticipantsList(jid)
console.dir(requests, { depth: null })

// --- Get group info from link
const group = await sock.groupGetInviteInfo('ABC123456789')
console.log('👥 Got group info from invite code', ':', group)

// --- Update bot member label
sock.updateMemberLabel(jid, '@astracode/byles-new')
```

</details>

<details>
<summary><b>👥 Community Management — Klik untuk lihat kode</b></summary>

```javascript
// --- Create a new one and add description
const community = await sock.communityCreate('@astracode/byles-new', '📣 Fresh updates weekly')
console.dir(community, { depth: null })

// --- Create a subgroup for community and add participants using their JIDs
const group = await sock.communityCreateGroup('📢 Announcements', ['628123456789@s.whatsapp.net'], communityJid)

// --- Link an existing group
sock.communityLinkGroup(groupJid, communityJid)

// --- Unlink an existing group
sock.communityUnlinkGroup(groupJid, communityJid)

// --- Get info
const metadata = await sock.communityMetadata(jid)
console.dir(metadata, { depth: null })

// --- Get community invite code
const inviteCode = await sock.communityInviteCode(jid)
console.dir(inviteCode, { depth: null })

// --- Revoke invite link
sock.communityRevokeInvite(jid)

// --- Accept community invite
sock.communityAcceptInvite(inviteCode)

// --- Leave community
sock.communityLeave(jid)

// --- Accept join requests
sock.communityRequestParticipantsUpdate(jid, ['628123456789@s.whatsapp.net'], 'approve')

// --- Change name
sock.communityUpdateSubject(jid, '📦 @astracode/byles-new')

// --- Change description
sock.communityUpdateDescription(jid, 'Updated description')

// --- Set community as admin only for chatting
sock.communitySettingUpdate(jid, 'announcement')

// --- Set community as open to all for chatting
sock.communitySettingUpdate(jid, 'not_announcement')

// --- Set admin only can edit community info
sock.communitySettingUpdate(jid, 'locked')

// --- Set all participants can edit community info
sock.communitySettingUpdate(jid, 'unlocked')

// --- Set admin only can add participants
sock.communityMemberAddMode(jid, 'admin_add')

// --- Set all participants can add participants
sock.communityMemberAddMode(jid, 'all_member_add')

// --- Enable or disable temporary messages with seconds format
sock.communityToggleEphemeral(jid, 86400)

// --- Disable temporary messages
sock.communityToggleEphemeral(jid, 0)

// --- Enable or disable membership approval mode
sock.communityJoinApprovalMode(jid, 'on')
sock.communityJoinApprovalMode(jid, 'off')

// --- Get all communities metadata
const communities = await sock.communityFetchAllParticipating()
console.dir(communities, { depth: null })

// --- Get all community linked groups
const linked = await sock.communityFetchLinkedGroups(jid)
console.dir(linked, { depth: null })

// --- Get pending join requests
const requests = await sock.communityRequestParticipantsList(jid)
console.dir(requests, { depth: null })

// --- Get community info from link
const community = await sock.communityGetInviteInfo('ABC123456789')
console.log('👥 Got community info from invite code', ':', community)
```

</details>

<details>
<summary><b>👤 Profile Management — Klik untuk lihat kode</b></summary>

```javascript
// --- Get user profile picture
const url = await sock.profilePictureUrl(jid, 'image')
console.log('🖼️ Got user profile url', url)

// --- Update profile picture
sock.updateProfilePicture(jid, buffer)
sock.updateProfilePicture(jid, { url })

// --- Remove profile picture
sock.removeProfilePicture(jid)

// --- Update profile name
sock.updateProfileName('My Name')

// --- Update profile status
sock.updateProfileStatus('Available')

// --- Presence
sock.sendPresenceUpdate('available', jid)
sock.presenceSubscribe(jid)

// --- Read receipts
sock.readMessages([message.key])
sock.sendReceipt(jid, participant, [messageId], 'read')

// --- Block user
sock.updateBlockStatus(jid, 'block')

// --- Unblock user
sock.updateBlockStatus(jid, 'unblock')

// --- Fetch blocklist
const blocked = await sock.fetchBlocklist()
console.dir(blocked, { depth: null })

// --- Modify chats
sock.chatModify({
   archive: true,
   lastMessageOrig: message,
   lastMessage: message
}, jid)

// --- Star messages
sock.star(jid, [{ id: messageId, fromMe: true }], true)

// --- Contact
sock.addOrEditContact(jid, { displayName: 'Starseed' })
sock.removeContact(jid)

// --- Label
sock.addChatLabel(jid, labelId)
sock.removeChatLabel(jid, labelId)
sock.addMessageLabel(jid, messageId, labelId)

// --- App state sync
sock.resyncAppState(['regular', 'critical_block'], true)

// --- Get business profile
const profile = await sock.getBusinessProfile(jid)
console.dir(profile, { depth: null })
```

</details>

<details>
<summary><b>🛒 Business Management — Klik untuk lihat kode</b></summary>

```javascript
// --- Create a new product
const product = await sock.productCreate({
   name: '🧩 Starseed (Premium)',
   description: 'Get a full version of Starseed!',
   price: 100000,
   currency: 'IDR',
   originCountryCode: 'ID',
   images: [
      bufferImage,
      {
         url: './path/to/image.jpg'
      }
   ]
})
console.dir(product, { depth: null })

// --- Update product
await sock.productUpdate(productId, {
   name: '🧩 Starseed (Premium)',
   description: 'Get a full version of Starseed with more features!',
   price: 75000,
   currency: 'IDR',
   images: [
      {
         url: './path/to/image.jpg'
      }
   ]
})

// --- Delete product
sock.productDelete([productId])

// --- Get catalog info
const { products, nextPageCursor } = await sock.getCatalog({
  jid: '628123456789@s.whatsapp.net',
  limit: 10
})

// --- Get collections
const collections = await sock.getCollections('628123456789@s.whatsapp.net', 10)
console.dir(collections, { depth: null })

// --- Get order info
const order = await sock.getOrderDetails(orderId, tokenBase64)
console.dir(order, { depth: null })

// --- Update business profile
await sock.updateBusinessProfile({
   address: 'Jakarta, Indonesia',
   description: '🛒 Official Starseed Store',
   websites: ['https://www.npmjs.com/package/@astracode/byles-new'],
   email: 'more-more@gmail.com',
   hours: {
      timezone: 'Asia/Jakarta',
      days: [{ day: 'mon', mode: 'open_24h' }]
   }
})

// --- Update cover
sock.updateCoverPhoto({
   url: './path/to/image.jpg'
})

// --- Remove cover
sock.removeCoverPhoto(coverId)

// --- Update quick replies
sock.addOrEditQuickReply({
  shortcut: 'hello',
  message: 'Hello from business account',
})

// --- Remove quick reply
sock.removeQuickReply(timestamp)
```

</details>

<details>
<summary><b>🔐 Privacy Management — Klik untuk lihat kode</b></summary>

```javascript
// --- Update last seen privacy
sock.updateLastSeenPrivacy('all')
sock.updateLastSeenPrivacy('contacts')
sock.updateLastSeenPrivacy('contact_blacklist')
sock.updateLastSeenPrivacy('nobody')

// --- Update online privacy
sock.updateOnlinePrivacy('all')
sock.updateOnlinePrivacy('match_last_seen')

// --- Update profile picture privacy
sock.updateProfilePicturePrivacy('contacts')

// --- Update status privacy
sock.updateStatusPrivacy('contacts')

// --- Update read receipts privacy
sock.updateReadReceiptsPrivacy('all')
sock.updateReadReceiptsPrivacy('none')

// --- Update groups add privacy
sock.updateGroupsAddPrivacy('all')
sock.updateGroupsAddPrivacy('contacts')

// --- Update messages privacy
sock.updateMessagesPrivacy('all')
sock.updateMessagesPrivacy('contacts')
sock.updateMessagesPrivacy('nobody')

// --- Update call privacy
sock.updateCallPrivacy('everyone')

// --- Update default disappearing mode
sock.updateDefaultDisappearingMode(86400)

// --- Update link previews privacy
sock.updateDisableLinkPreviewsPrivacy(true)
```

</details>

<details>
<summary><b>📡 Events — Klik untuk lihat kode</b></summary>

```javascript
sock.ev.on('connection.update', (update) => {})
sock.ev.on('creds.update', (update) => {})
sock.ev.on('messaging-history.set', (update) => {})
sock.ev.on('messaging-history.status', (update) => {})
sock.ev.on('chats.upsert', (update) => {})
sock.ev.on('chats.update', (update) => {})
sock.ev.on('chats.delete', (update) => {})
sock.ev.on('chats.lock', (update) => {})
sock.ev.on('lid-mapping.update', (update) => {})
sock.ev.on('presence.update', (update) => {})
sock.ev.on('contacts.upsert', (update) => {})
sock.ev.on('contacts.update', (update) => {})
sock.ev.on('messages.delete', (update) => {})
sock.ev.on('messages.update', (update) => {})
sock.ev.on('messages.media-update', (update) => {})
sock.ev.on('messages.upsert', (update) => {})
sock.ev.on('messages.reaction', (update) => {})
sock.ev.on('message-receipt.update', (update) => {})
sock.ev.on('groups.upsert', (update) => {})
sock.ev.on('groups.update', (update) => {})
sock.ev.on('group-participants.update', (update) => {})
sock.ev.on('group.join-request', (update) => {})
sock.ev.on('group.member-tag.update', (update) => {})
sock.ev.on('blocklist.set', (update) => {})
sock.ev.on('blocklist.update', (update) => {})
sock.ev.on('call', (update) => {})
sock.ev.on('labels.edit', (update) => {})
sock.ev.on('labels.association', (update) => {})
sock.ev.on('newsletter.reaction', (update) => {})
sock.ev.on('newsletter.view', (update) => {})
sock.ev.on('newsletter-participants.update', (update) => {})
sock.ev.on('newsletter-settings.update', (update) => {})
sock.ev.on('settings.update', (update) => {})
```

</details>

---

## 🔗 **Links & Community**

<div align="center">

| 🌐 Platform | 🔗 Link |
| :---: | :--- |
| 🐙 **GitHub** | [Astra-Devl](https://github.com/Astra-Devl) |
| 📦 **NPM Package** | [@astracode/byles-new](https://www.npmjs.com/package/@astracode/byles-new) |
| 💬 **WhatsApp Channel** | [Join Channel](https://whatsapp.com/channel/0029VbCsoB0H5JLxG7J1CU31) |
| 🐛 **Issues** | [Report Bug](https://github.com/Astra-Devl/byles-new/issues) |
| ⭐ **Repository** | [byles-new](https://github.com/Astra-Devl/byles-new) |

</div>

---

<div align="center">

## 💖 **Made with Love by AstraCode**

<p>
  <a href="https://github.com/Astra-Devl/byles-new/stargazers">
    <img src="https://img.shields.io/badge/⭐_Star_this_repo-ffab00?style=for-the-badge&logo=github&logoColor=black" />
  </a>
  <a href="https://www.npmjs.com/package/@astracode/byles-new">
    <img src="https://img.shields.io/badge/📦_Install_from_NPM-cb3837?style=for-the-badge&logo=npm&logoColor=white" />
  </a>
  <a href="https://whatsapp.com/channel/0029VbCsoB0H5JLxG7J1CU31">
    <img src="https://img.shields.io/badge/💬_Join_Channel-25D366?style=for-the-badge&logo=whatsapp&logoColor=white" />
  </a>
</p>

**© 2025 AstraCode — All Rights Reserved**

</div>
