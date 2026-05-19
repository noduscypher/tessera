# TESSERA — INSTALL

## requirements

- Raspberry Pi or any Linux node
- Reticulum network stack
- NomadNet installed and running
- Python 3
- git

---

## step 1 — clone tessera

```bash
git clone https://github.com/YOUR_USERNAME/tessera
cd tessera
```

---

## step 2 — clone the base chatroom

tessera builds on top of noduscypher/thechatroom.
clone it and copy files to your NomadNet pages root with `chatroom-` prefix:

```bash
cd /tmp && git clone https://github.com/noduscypher/thechatroom chatroom_base

cp /tmp/chatroom_base/nomadnet.mu ~/.nomadnetwork/storage/pages/chatroom-nomadnet.mu
cp /tmp/chatroom_base/meshchat.mu ~/.nomadnetwork/storage/pages/chatroom-meshchat.mu
cp /tmp/chatroom_base/fullchat.mu ~/.nomadnetwork/storage/pages/chatroom-fullchat.mu
cp /tmp/chatroom_base/last100.mu  ~/.nomadnetwork/storage/pages/chatroom-last100.mu
cp /tmp/chatroom_base/index.mu    ~/.nomadnetwork/storage/pages/chatroom-index.mu

chmod +x ~/.nomadnetwork/storage/pages/chatroom-nomadnet.mu
chmod +x ~/.nomadnetwork/storage/pages/chatroom-meshchat.mu
chmod +x ~/.nomadnetwork/storage/pages/chatroom-fullchat.mu
chmod +x ~/.nomadnetwork/storage/pages/chatroom-last100.mu
```

---

## step 3 — fix internal links

NomadNet does not serve pages from subdirectories.
all files must be in the pages root with the `chatroom-` prefix:

```bash
sed -i 's|:/page/nomadnet\.mu|:/page/chatroom-nomadnet.mu|g' ~/.nomadnetwork/storage/pages/chatroom-nomadnet.mu
sed -i 's|:/page/fullchat\.mu|:/page/chatroom-fullchat.mu|g' ~/.nomadnetwork/storage/pages/chatroom-nomadnet.mu
sed -i 's|:/page/last100\.mu|:/page/chatroom-last100.mu|g'   ~/.nomadnetwork/storage/pages/chatroom-nomadnet.mu
sed -i 's|:/page/nomadnet\.mu|:/page/chatroom-nomadnet.mu|g' ~/.nomadnetwork/storage/pages/chatroom-fullchat.mu
sed -i 's|:/page/nomadnet\.mu|:/page/chatroom-nomadnet.mu|g' ~/.nomadnetwork/storage/pages/chatroom-last100.mu
sed -i 's|:/page/nomadnet\.mu|:/page/chatroom-nomadnet.mu|g' ~/.nomadnetwork/storage/pages/chatroom-index.mu
sed -i 's|:/page/meshchat\.mu|:/page/chatroom-meshchat.mu|g' ~/.nomadnetwork/storage/pages/chatroom-index.mu
```

---

## step 4 — fix file paths

the scripts use `os.path.dirname(__file__)` to find data files.
after copying to pages root this breaks — fix it:

```bash
sed -i 's|os\.path\.dirname(__file__)|os.path.expanduser("~/.nomadnetwork/storage/pages/chatroom")|g' \
  ~/.nomadnetwork/storage/pages/chatroom-nomadnet.mu

sed -i 's|os\.path\.dirname(__file__)|os.path.expanduser("~/.nomadnetwork/storage/pages/chatroom")|g' \
  ~/.nomadnetwork/storage/pages/chatroom-meshchat.mu

sed -i 's|os\.path\.dirname(__file__)|os.path.expanduser("~/.nomadnetwork/storage/pages/chatroom")|g' \
  ~/.nomadnetwork/storage/pages/chatroom-fullchat.mu

sed -i 's|os\.path\.dirname(__file__)|os.path.expanduser("~/.nomadnetwork/storage/pages/chatroom")|g' \
  ~/.nomadnetwork/storage/pages/chatroom-last100.mu
```

---

## step 5 — create data directory and files

```bash
mkdir -p ~/.nomadnetwork/storage/pages/chatroom

echo '[]' > ~/.nomadnetwork/storage/pages/chatroom/chat_log.json

echo '{"topic": "your topic here", "author": "your name", "time": "2026-01-01 00:00"}' \
  > ~/.nomadnetwork/storage/pages/chatroom/topic.json

cp /tmp/chatroom_base/emoticon.txt ~/.nomadnetwork/storage/pages/chatroom/emoticons.txt
```

---

## step 6 — copy tessera pages

```bash
cp pages/chatroom.mu          ~/.nomadnetwork/storage/pages/
cp pages/chatroom-auth.mu     ~/.nomadnetwork/storage/pages/
cp pages/cypherpunktalks.mu   ~/.nomadnetwork/storage/pages/
cp pages/cypherpunktalks-auth.mu ~/.nomadnetwork/storage/pages/
```

---

## step 7 — configure your personal data

replace placeholders with your actual values:

```bash
# in chatroom-auth.mu and cypherpunktalks-auth.mu
# replace YOUR_LXMF_HASH with your actual LXMF identity hash
sed -i 's|YOUR_LXMF_HASH|your_actual_hash_here|g' \
  ~/.nomadnetwork/storage/pages/chatroom-auth.mu

sed -i 's|YOUR_LXMF_HASH|your_actual_hash_here|g' \
  ~/.nomadnetwork/storage/pages/cypherpunktalks-auth.mu
```

---

## step 8 — choose your secret filename

rename the private chatroom entry point to something only you know:

```bash
mv ~/.nomadnetwork/storage/pages/chatroom-index.mu \
   ~/.nomadnetwork/storage/pages/YOUR_SECRET_NAME.mu
```

share this secret name only via LXMF with trusted contacts.
never publish it.

---

## step 9 — test

```bash
python3 ~/.nomadnetwork/storage/pages/chatroom-nomadnet.mu 2>&1 | head -5
```

should output the first lines of the chatroom page without errors.

---

## step 10 — restart NomadNet

```bash
sudo systemctl restart nomadnet.service
```

---

## step 11 — update your index.mu

add to your main index page:

```
`[☍ chatroom`:/page/chatroom.mu]
  mesh chat · local signal · open

`[☍ cypherpunktalks`:/page/cypherpunktalks.mu]
  cypherpunk channel · invite only
```

---

## done

```
public  → YOUR_NODE_HASH:/page/chatroom.mu
private → YOUR_NODE_HASH:/page/YOUR_SECRET_NAME.mu  (share via LXMF only)
```

---

walk quietly, ₿ut keep the signal alive.
