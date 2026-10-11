<div align="center">

# OpenCore EFI · Lenovo ThinkPad E14 Gen 1 · macOS Tahoe 26

![OpenCore](https://img.shields.io/badge/OpenCore-1.0.8-1f6feb?style=for-the-badge)
![macOS](https://img.shields.io/badge/macOS-Tahoe_26.7.1-000000?style=for-the-badge&logo=apple&logoColor=white)
![ThinkPad](https://img.shields.io/badge/ThinkPad-E14_Gen_1-E2231A?style=for-the-badge&logo=lenovo&logoColor=white)
![CPU](https://img.shields.io/badge/CPU-i5--10210U_Comet_Lake-0071C5?style=for-the-badge&logo=intel&logoColor=white)

![Status](https://img.shields.io/badge/status-testado_no_hardware_real-2ea44f?style=flat-square)
![Release](https://img.shields.io/github/v/release/gabrielevanger/ThinkPad-E14-Gen1-Hackintosh-Tahoe?style=flat-square&label=release)
![Downloads](https://img.shields.io/github/downloads/gabrielevanger/ThinkPad-E14-Gen1-Hackintosh-Tahoe/total?style=flat-square&label=downloads)
![Last commit](https://img.shields.io/github/last-commit/gabrielevanger/ThinkPad-E14-Gen1-Hackintosh-Tahoe?style=flat-square&label=atualizado)

**[⬇️ Baixar a EFI (última release)](https://github.com/gabrielevanger/ThinkPad-E14-Gen1-Hackintosh-Tahoe/releases/latest)**

[Hardware](#hardware) · [Screenshots](#screenshots) · [Configuração](#configuração-e-por-quê) · [Antes de usar](#antes-de-usar) · [Depois de instalar](#depois-de-instalar) · [Créditos](#créditos)

</div>

EFI do OpenCore para o **Lenovo ThinkPad E14 Gen 1 (20RA/20RB)** rodando **macOS Tahoe 26.7.1**,
montada para fins educacionais. Foi feita a partir do `Sample.plist` oficial do OpenCore e de
componentes baixados direto dos repositórios originais.

![macOS Tahoe 26.7.1 no ThinkPad E14 Gen 1](screenshots/sobre-fastfetch.png)

> ⚠️ **Não use sem antes gerar o seu próprio SMBIOS** (veja abaixo). Hackintosh não é suportado
> pela Apple: use por sua conta e risco e mantenha backup dos seus dados.

## Hardware

| Componente | Modelo | Status |
|---|---|---|
| CPU | Intel Core i5-10210U (Comet Lake) | ✅ |
| iGPU | Intel UHD 620 | ✅ aceleração, brilho, HDMI |
| Áudio | Realtek ALC257 | ✅ requer root patch no Tahoe (veja abaixo) |
| Ethernet | Realtek RTL8111 | ✅ |
| Wi-Fi | Intel AX201 | ✅ via itlwm + [HeliPort](https://github.com/OpenIntelWireless/HeliPort) |
| Bluetooth | Intel AX201 | ✅ |
| Touchpad | ELAN | ✅ via VoodooPS2 |
| TrackPoint | PS/2 | ✅ |
| Teclado | ABNT2 | ✅ inclusive a tecla `/?°` |
| Câmera | Integrada (USB) | ✅ |
| Bateria | — | ✅ porcentagem e carga |
| Suspensão | — | ✅ |
| Leitor de digitais | Goodix | ❌ sem driver para macOS |

## Screenshots

Capturas do sistema rodando com esta EFI (serial, IP, endereços MAC/Bluetooth, localização e redes de terceiros ocultados).

**Sobre Este Mac + fastfetch** — macOS Tahoe 26.7.1 (25G241), MacBookPro16,1, i5-10210U

![Sobre Este Mac e fastfetch](screenshots/sobre-fastfetch.png)

| | |
|---|---|
| **Gráficos** — UHD 620 com Metal 3 | **Áudio** — entrada e saída integradas |
| ![Gráficos](screenshots/graficos.png) | ![Áudio](screenshots/audio.png) |
| **Bluetooth** — ativado, firmware carregado | **USB** — câmera, Bluetooth e leitor de digitais |
| ![Bluetooth](screenshots/bluetooth.png) | ![USB](screenshots/usb.png) |
| **Ethernet** — RTL8111 a 1 Gb/s | **Wi-Fi** — conectado via itlwm/HeliPort |
| ![Ethernet](screenshots/ethernet.png) | ![Wi-Fi](screenshots/wifi-ethernet.png) |
| **Bateria** — leitura e histórico de carga | |
| ![Bateria](screenshots/bateria.png) | |

## Versões

- **OpenCore:** 1.0.8 (RELEASE)
- **SMBIOS:** MacBookPro16,1

| Kext | Versão | Função |
|---|---|---|
| Lilu | 1.7.2 | Base para os plugins |
| VirtualSMC + SMCProcessor + SMCBatteryManager | 1.3.8 | SMC, sensores e bateria |
| WhateverGreen | 1.7.1 | iGPU |
| AppleALC | 1.9.8 | Áudio (`layout-id 15`) |
| ECEnabler | 1.0.6 | Leitura da bateria |
| VoodooPS2Controller | 2.3.7 | Teclado, touchpad ELAN e TrackPoint |
| BrightnessKeys | 1.0.3 | Teclas de brilho |
| RealtekRTL8111 | 2.5.0 | Ethernet |
| itlwm | 2.3.0 | Wi-Fi Intel |
| IntelBluetoothFirmware + IntelBTPatcher | 2.5.1 (fork lshbluesky, suporte ao Tahoe) | Bluetooth |
| BlueToolFixup | 2.7.2 | Bluetooth |
| NVMeFix | 1.1.3 | NVMe |
| USBToolBox + UTBMap | 1.2.0 | Mapa de USB |
| CPUFriend | 1.3.0 | Energia (**desativado**, veja abaixo) |

**ACPI** (prebuilts da Dortania): SSDT-PLUG-DRTNIA, SSDT-EC-USBX-LAPTOP, SSDT-AWAC, SSDT-PNLF.

## Configuração e por quê

| Item | Valor | Motivo |
|---|---|---|
| iGPU | `AAPL,ig-platform-id 00009B3E`, `device-id 9B3E0000`, stolenmem/fbmem | UHD 620 (Comet Lake, `0x9B41`) com spoof para `0x3E9B` |
| HDMI | con1/con2 tipo HDMI + `hda-gfx onboard-1` | Imagem e áudio pelo HDMI |
| SMBIOS | MacBookPro16,1 + `agdpmod=vit9696` | Com o 16,2 (Ice Lake), a decodificação de vídeo e o YouTube falhavam |
| Ethernet | RealtekRTL8111 **2.5.0** com EEE/TSO4/TSO6/CSO6 desligados | A versão 3.0.0 e o EEE derrubavam o link e causavam `PKDownloadError 8` na instalação |
| Touchpad | Só VoodooPS2, sem VoodooI2C/VoodooSMBus | O VoodooPS2 já tem suporte ao ELAN. O VoodooI2C deixava o touchpad parcial, e o VoodooSMBus 2.2 nem carrega no Comet Lake (não reconhece o SMBus `8086:02a3`) |
| Teclado | `Use ISO layout keyboard = true` + `Custom ADB Map e01d=5e` | No ABNT2 do ThinkPad, a tecla `/?°` envia o scancode do Ctrl direito (`e01d`) |
| USB | UTBMap com HS01–HS10 + SS01–SS04 (14 portas) | Sem o mapa, a câmera e o Bluetooth (porta interna 10) ficavam de fora do limite de 15 portas |
| Bluetooth | `-btlfxallowanyaddr` + NVRAM `bluetoothExternalDongleFailed=00` e `bluetoothInternalControllerInfo=00…` (Add + Delete) | Corrige o `Address: NULL` / `BCM_4350C2` do Tahoe ([issue #513](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/issues/513)) |
| CFG Lock | `AppleXcpmCfgLock = true` | A BIOS da Lenovo não deixa desligar o CFG Lock |
| SIP | `csr-active-config = 03080000` | Necessário para o root patch de áudio |
| Segurança | `SecureBootModel = Disabled` | Necessário para instalar o Tahoe |
| Boot | `ShowPicker = false` | Liga direto no macOS. Segure **Esc** ao ligar para ver o menu |

## Antes de usar

### 1. SMBIOS (obrigatório)
Os campos `SystemSerialNumber`, `MLB`, `SystemUUID` e `ROM` estão **vazios**. Para preenchê-los:
1. Gere com o [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) ou com o `macserial` do OpenCore, usando o modelo **MacBookPro16,1**.
2. Confira o serial em [checkcoverage.apple.com](https://checkcoverage.apple.com). Ele precisa aparecer como **inválido**. Se aparecer um Mac real, gere outro.
3. Em `ROM`, use o MAC da sua placa Ethernet.

### 2. BIOS
- Secure Boot: Disabled
- SGX: Disabled
- Startup: UEFI Only
- CSM: Disabled
- Fast Boot: Disabled

### 3. Instalação
- Use **cabo de rede**, porque o Wi-Fi não funciona no recovery.
- Se der `PKDownloadError 8`, confira a hora no Terminal do recovery com `date`. Ela precisa estar em UTC. Ajuste com `date MMDDhhmmAA`.

## Depois de instalar

### Áudio no Tahoe
O macOS 26 não tem mais o AppleHDA. Para ter som:
1. Instale o **KDK** exato da sua build, de [dortania/KdkSupportPkg](https://github.com/dortania/KdkSupportPkg/releases).
2. Aplique o root patch de áudio com uma ferramenta de root patch. Este repositório **não** inclui nenhuma.
3. **Repita isso a cada atualização do macOS.**

### Wi-Fi
Instale o [HeliPort 1.5.0](https://github.com/OpenIntelWireless/HeliPort/releases). Para ele abrir sozinho, use um LaunchAgent (`open -a HeliPort` com `RunAtLoad`).

### CPUFriend (opcional, recomendado)
O `CPUFriendDataProvider` é gerado **por máquina**:
1. Rode o [one-key-cpufriend](https://github.com/stevezhengshiqi/one-key-cpufriend) no próprio Mac. Usei LFM 800 MHz e EPP Balance Power.
2. Coloque o `CPUFriendDataProvider.kext` em `OC/Kexts`.
3. Habilite as duas entradas no `config.plist`.

### Energia
```
sudo pmset -a hibernatemode 0 standby 0 autopoweroff 0 proximitywake 0 powernap 0
```

## Créditos

- [Acidanthera](https://github.com/acidanthera): OpenCore, Lilu, WhateverGreen, AppleALC, VirtualSMC, VoodooPS2, NVMeFix, BrightnessKeys, CPUFriend, BrcmPatchRAM
- [Dortania](https://dortania.github.io): guias e SSDTs pré-compiladas
- [OpenIntelWireless](https://github.com/OpenIntelWireless): itlwm, HeliPort, IntelBluetoothFirmware
- [lshbluesky](https://github.com/lshbluesky/IntelBluetoothFirmware): fork do IntelBluetoothFirmware com suporte ao Tahoe
- [Mieze](https://github.com/Mieze/RTL8111_driver_for_OS_X): RealtekRTL8111
- [1Revenger1](https://github.com/1Revenger1/ECEnabler): ECEnabler
- [USBToolBox](https://github.com/USBToolBox/kext)
- [gdantas04/ThinkPad-E14-macOS-OpenCore](https://github.com/gdantas04/ThinkPad-E14-macOS-OpenCore) (MIT): referência do mesmo modelo, incluindo o `layout-id` 15, os conectores HDMI e o mapa de portas USB

Cada componente mantém a licença do seu projeto original. O OpenCore é distribuído sob BSD-3-Clause
(veja `LICENSE-OpenCore.txt`).
