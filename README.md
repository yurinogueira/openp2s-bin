# openp2s-bin

Pacote AUR (Arch User Repository) para o **[OpenP2S](https://github.com/wyruweso/openp2s)** — cliente nativo de Linux para VPNs Point-to-Site (P2S) do Azure com autenticação Microsoft Entra ID.

> **Créditos e Autoria / Credits & Upstream Attribution**
>
> O projeto **OpenP2S** foi criado e é mantido por **[Mykhailo Tseluiko (`@wyruweso`)](https://github.com/wyruweso)** (`<tseluiko.m@gmail.com>`).
> Repositório oficial upstream: **[https://github.com/wyruweso/openp2s](https://github.com/wyruweso/openp2s)**.
>
> Este repositório contém exclusivamente o empacotamento para Arch Linux / AUR (`PKGBUILD`, `.SRCINFO`, `.install`), mantido por **Yuri Nogueira** (`<personal@yurinogueira.dev.br>`).

---

## Arquitetura do Pacote no Arch Linux

Este pacote utiliza o release oficial pré-compilado (`openp2s-<pkgver>-linux-amd64.tar.gz`) e a man page oficial (`packaging/openp2s.1`) do projeto upstream:

1. **Preservação de Binários (`options=('!strip' '!debug')`)**:
   - `/usr/bin/openp2s` é uma *Node.js Single Executable Application (SEA)* construída sobre o runtime oficial autocontido do Node.js. Aplicar `strip` corromperia o blob SEA injetado.
   - `/usr/lib/openp2s/openvpn` é o OpenVPN oficial com os patches `long-credentials.patch` e `experimental-azure-compat.patch`, instalado em diretório privado para nunca conflitar com o pacote `openvpn` do sistema (`/usr/bin/openvpn`). Seu hash SHA-256 é registrado em `/usr/lib/openp2s/BUILDINFO` e verificado em tempo de execução pelo `openp2s` — aplicar `strip` quebraria a verificação de proveniência.
2. **Dependências Nativas do Arch Linux**:
   - Declara `openssl-3` (provê `libssl.so.3` e `libcrypto.so.3` no `[core]`), `libcap-ng`, `glibc`, `gcc-libs`, `systemd` (`resolvectl` para Split DNS), `ca-certificates-utils` e `iproute2`.
   - Dependências opcionais (`optdepends`): `sudo` (escalação de privilégios apenas para `openvpn` e `resolvectl`) e `xdg-utils` (`xdg-open` para autenticação interativa no navegador).
3. **Licenças e Créditos Instalados**:
   - Instala `LICENSE` (`GPL-2.0-only`), `NODE_LICENSE`, `openvpn-COPYING`, `THIRD_PARTY_NOTICES` e `AUTHORS` em `/usr/share/licenses/openp2s-bin/`.

---

## Desinstalação de Instalação Manual Prévia

Se você instalou o `openp2s` manualmente em `/usr/local` anteriormente, remova os arquivos manuais antes (ou logo após) instalar o pacote AUR para evitar que `/usr/local/bin/openp2s` tenha precedência no `PATH`:

```bash
sudo rm -rf /usr/local/bin/openp2s /usr/local/lib/openp2s /usr/local/share/doc/openp2s
```

## Instalação

### Via AUR Helper (`paru` ou `yay`)

```bash
paru -S openp2s-bin
# ou
yay -S openp2s-bin
```

### Manualmente via `makepkg`

```bash
git clone https://aur.archlinux.org/openp2s-bin.git
cd openp2s-bin
makepkg -si
```

## Verificação e Uso

```bash
openp2s --version
openp2s doctor
openp2s connect azurevpnconfig.xml
```

---

## English Summary

AUR packaging for **[OpenP2S](https://github.com/wyruweso/openp2s)**, a native Linux client for Azure Point-to-Site VPNs using Microsoft Entra ID authentication, created by **Mykhailo Tseluiko (`@wyruweso`)**.

- Installs `/usr/bin/openp2s`, the private patched OpenVPN binary at `/usr/lib/openp2s/openvpn`, provenance metadata `/usr/lib/openp2s/BUILDINFO`, man page `openp2s(1)`, documentation, and full upstream license/author notices (`/usr/share/licenses/openp2s-bin/`).
- Keeps `options=('!strip' '!debug')` to preserve the Node.js SEA executable and the `BUILDINFO` SHA-256 provenance check.
- Depends on `openssl-3`, `libcap-ng`, `glibc`, `gcc-libs`, `systemd`, `ca-certificates-utils`, and `iproute2` (with `sudo` and `xdg-utils` as `optdepends`).
