# 📦 Configuração de Partições NTFS no Fedora com Montagem Automática

Este guia explica como habilitar leitura/escrita em partições NTFS no Fedora e configurar montagem automática no boot.

---

## ✅ 1. Instalar suporte NTFS

```bash
sudo dnf install ntfs-3g
```

---

## ✅ 2. Identificar a partição

Liste os discos e identifique sua partição NTFS (ex: `/dev/sdb1`):

```bash
lsblk
```

Anote o nome da partição (ex: `/sdb1`).

---

## ✅ 3. Criar ponto de montagem

Crie uma pasta no "/media" para montar a partição, pois, assim aparecerá no Nautilus (exemplo: `/media/<seu_usuario>/<nome_da_partição>`):

```bash
sudo mkdir -p /media/<seu_usuario>/<nome_da_partição>
```

---

## ✅ 4. Verificar o UUID

```bash
sudo blkid /dev/<partição>
```

Anote o valor do `UUID`.

---

## ✅ 6. Adicionar ao `/etc/fstab` para montar no boot

Abra o arquivo:

```bash
sudo nano /etc/fstab
```

Adicione a seguinte linha, substituindo o UUID:

```
UUID=<UUID>  /media/<seu_usuario>/<nome_da_partição>  ntfs-3g  defaults,uid=1000,gid=1000,umask=0022  0  0
```

- `uid=1000` e `gid=1000` assumem que seu usuário tem ID 1000 (padrão no Fedora)
- `umask=0022` deixa somente o dono com permissão de escrita

---

## ✅ 7. Aplicar mudanças

```bash
sudo systemctl daemon-reexec
sudo mount -a
```

---

## ✅ Pronto!

Sua partição NTFS agora monta automaticamente com leitura e escrita, e está visível no Nautilus.