# TESSERA — SCHEME

## access flow

```
index.mu
├── [chatroom] ──────────────────→ chatroom.mu (fake — ASCII art + "awaiting authentication")
│                                        └── [enter] → chatroom-auth.mu (erro: "LXMF identity not recognised")
│
├── [cypherpunktalks] ───────────→ cypherpunktalks.mu (fake — ASCII art + "awaiting authentication")
│                                        └── [enter] → cypherpunktalks-auth.mu (erro: "LXMF identity not recognised")
│
└── (link privado) ──────────────→ chatroom-index.mu (real — dashboard noduscypher)
                                         ├── [NOMADNET INTERFACE] → chatroom-nomadnet.mu
                                         └── [MESHcHAT INTERFACE] → chatroom-meshchat.mu
```

---

## entry process

```
  PESSOA                              OPERADOR (tu)
  ──────                              ─────────────
  1. vê a página fake
  2. contacta-te via LXMF        →    3. recebes o pedido
     hash: 0a1b2c3d4e5f6789             Field Signal:
           0abcdef012345678              0a1b2c3d4e5f67890abcdef012345678

                                    4. decides se aceitas
                                    5. partilhas o link privado  →  pessoa recebe
                                       chatroom-index.mu              o link
                                       (via mensagem LXMF)

  6. abre o link no NomadNet
     e entra diretamente
```

---

## notes

- no whitelist — access is by secret link only
- whoever has the link can enter — never share publicly
- to revoke access: rename the file on the node, share new link only with trusted contacts
- security through silence

---

## dead drop (planned)

- leave an encrypted message on the node — no name, no sender
- receive a random 8-character token
- share the token via LXMF
- recipient enters token in tessera
- message appears once, then is destroyed
- zero trace. zero log.
