# tessera

invite-only mesh chatroom scheme for NomadNet / Reticulum

---

        "Non nobis Domine, non nobis,
         sed nomini tuo da gloriam."

        — Knights Templar, XII century
          carved in stone. Monsanto. Beira Baixa. Portugal.

---

eight centuries ago, the Knights Templar built their stronghold
on this rock and used substitution ciphers to communicate
across thousands of kilometres — not with the neighbour,
but with the world.

the cipher was the key.
only those who carried the method could read the message.

tessera continues that tradition.

no accounts. no servers. no tracking.
two rooms. public decoys. access by private signal only.
the link is the cipher. the node is the rock.

---

## what is tessera

tessera is not just a chatroom.

it is a **secure async communication scheme** for offline mesh networks,
built on NomadNet / Reticulum, inspired by the Templar cipher tradition
of Monsanto, Beira Baixa, Portugal — anno domini mcxviii.

**two channels:**
- `chatroom` — public. open to all. the town square.
- `cypherpunktalks` — private. invite only. access by secret link via LXMF.

**dead drop** *(coming soon)*
leave an encrypted message. receive a token.
the message exists once. for one person. then it is gone.

---

## how it works

```
index.mu
├── [chatroom] ────────────→ public chatroom (anyone enters)
│
├── [cypherpunktalks] ─────→ decoy page (fake auth error)
│                                  └── real access: secret link via LXMF only
│
└── dead drop (coming soon)
```

**entry process for private channel:**
```
person                         operator
──────                         ────────
sees decoy page
contacts operator via LXMF  →  receives request
                               decides to accept
                               shares private link via LXMF  →  person enters
```

**to revoke access:** rename the private file on the node.
share new link only with trusted contacts.

---

## install

### requirements
- Reticulum network stack
- NomadNet node
- Python 3
- based on: noduscypher/thechatroom

### steps

```bash
# 1. clone tessera
git clone https://github.com/YOUR_USERNAME/tessera
cd tessera

# 2. copy files to your NomadNet pages directory
cp pages/*.mu ~/.nomadnetwork/storage/pages/

# 3. fix internal links (replace YOUR_SECRET_PAGE with your chosen filename)
sed -i 's|YOUR_SECRET_PAGE|your-actual-secret-name.mu|g' \
  ~/.nomadnetwork/storage/pages/cypherpunktalks.mu

# 4. create data directory and files
mkdir -p ~/.nomadnetwork/storage/pages/chatroom
echo '[]' > ~/.nomadnetwork/storage/pages/chatroom/chat_log.json
echo '{"topic": "your topic", "author": "your name", "time": "2026-01-01 00:00"}' \
  > ~/.nomadnetwork/storage/pages/chatroom/topic.json
cp emoticon.txt ~/.nomadnetwork/storage/pages/chatroom/emoticons.txt

# 5. fix file paths in scripts
sed -i 's|os.path.dirname(__file__)|os.path.expanduser("~/.nomadnetwork/storage/pages/chatroom")|g' \
  ~/.nomadnetwork/storage/pages/chatroom-nomadnet.mu

# 6. make executable
chmod +x ~/.nomadnetwork/storage/pages/chatroom-nomadnet.mu
chmod +x ~/.nomadnetwork/storage/pages/chatroom-fullchat.mu
chmod +x ~/.nomadnetwork/storage/pages/chatroom-last100.mu

# 7. restart NomadNet
sudo systemctl restart nomadnet.service

# 8. update index.mu with your node hash
# replace YOUR_NODE_HASH with your actual node hash
```

### update your index.mu

add these two links to your main index page:

```
`[☍ chatroom`:/page/chatroom-nomadnet.mu]
  mesh chat · local signal · open

`[☍ cypherpunktalks`:/page/cypherpunktalks.mu]
  cypherpunk channel · invite only
```

---

## security notes

- access to private channel is by secret link only — choose a filename that is hard to guess
- never publish your secret filename publicly
- whoever has the link can enter — share only via LXMF
- to revoke: rename the file, share new link with trusted contacts only
- chat log is stored unencrypted on disk — full disk encryption recommended

---

## dead drop *(coming soon)*

a one-time encrypted message system for async offline communication.

```
leave message → receive 12-char token → share token via LXMF
recipient enters token → message appears once → message is destroyed
zero trace. zero log.
```

like the Templars did — leave the message. give the code. disappear.

---

## reading list

Freddy Silva
- **The First Templar Nation** — how eleven knights created a new country and a refuge for the Grail
- **Legacy of the Gods** — the origin of sacred sites and the Sion revelation
- **The Divine Blueprint** — temples, power places and the global plan to shape the human soul

---

## credits

based on [noduscypher/thechatroom](https://github.com/noduscypher/thechatroom)

tessera scheme, manifesto and dead drop concept:
original work — Monsanto · Beira Baixa · Portugal

---

        walk quietly, ₿ut keep the signal alive.
        built with love in cooperation with nature.

        Reticulum Network · LoRa 868MHz · anno domini mcxviii
