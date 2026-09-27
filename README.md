# LBF-OS64-BITS_v4.6.5_Kernel_3.1_Runtime_7.4
SISTEMA OPERACIONAL  x86-64 BITS

## 📌 O que é o LBF-OS?

O **LBF-OS** é um sistema operacional experimental de 64 bits desenvolvido **completamente do zero** (*freestanding*, sem dependência de `libc` ou bibliotecas externas). O projeto combina arquitetura de baixa camada clássica com inovação em modo usuário (Ring 3), unindo:

- **Kernel próprio x86-64**: Gerenciamento de memória virtual, IDT/GDT e tarefas.
- **Subsistema Gráfico VESA/LFB**: Interface RAD rica em Ring 3 via `libgui`.
- **Sistema de Arquivos**: VFS com suporte a FAT32.
- **Rede e Segurança Nativa**: Pilha de rede TCP/IP em Ring 3 equipada com TLS 1.2 nativo.
- **Navegador Gráfico**: Renderização visual de páginas Web.
- **Ecossistema Neural (Mixture-of-Experts)**: Motor de IA modular que alimenta a assistente **Existência v10**.

---

## ✨ Destaques desta Versão

### 1. 🔤 Suporte Pleno a PT-BR — *O Contrato do "Byte Empacotado"*
O LBF-OS agora lê, processa e renderiza a língua portuguesa nativamente, do *scancode* do teclado ao pixel na tela:

- **Fonte 8x8 Estendida**: Glifos estáticos alocados diretamente nos slots `0x80–0xFF` (`ç`, `Ç`, `á`, `é`, `í`, `ó`, `ú`, `Á`, `É`, `Í`, `Ó`, `Ú`, `â`, `ê`, `ô`, `ã`, `õ`, `à`, `ü`, `ñ`, `©`…) — eliminando overhead de inicialização em runtime.
- **Teclado ABNT2 com *Dead Keys***: Tratamento completo de acentuação:
  - `´` + vogal = vogais acentuadas.
  - `~` e `^` para composição (`ã`, `õ`, `â`, `ê`, `ô`).
  - `` ` `` para crase.
  - `ç` / `Ç` diretos com sensibilidade ao estado do *Caps Lock*.
- **Controles RAD cientes de Bytes Estendidos**: Componentes como `TEdit`, `TMemo` e o manipulador de eventos central (`libgui.c`) aceitam e validam `(unsigned char) >= 0x80`.
- **LBF Browser — Conversão na Fronteira**: Função `pack_cp()` realiza o parse dinâmico de UTF-8, Latin-1 e entidades HTML (`&ccedil;`, `&copy;`, `&#231;`…) para o mapa de caracteres do sistema durante o processamento do documento.

---

### 2. 🌐 LBF Browser v4.3 — *Navegação Real em Ring 3*
- **Criptografia TLS 1.2 Nativa**: Implementação em C *freestanding* com suporte a `x25519`, `AES-GCM`, `SHA-256/HMAC` e gerador DRBG real.
- **Renderização Visual (`TWebPage`)**: Suporte a links interativos e exibição inline de imagens (`BMP`, `GIF`, `PNG`).
- **Recursos de Rede**: Tratamento de redirecionamentos HTTP (301/302), *Transfer-Encoding Chunked*, histórico de navegação e download de arquivos.
- **Pilha de Rede em Ring 3**: Protocolos DHCP, DNS, ARP e TCP executados integralmente fora do Kernel.

---

### 3. 📂 Gerenciador de Arquivos v0.1.1
- **Operações Seguras**: Manipulação de caminhos com limites rígidos de memória.
- **Integração CLI**: Comandos diretos (`cp`, `mv`, `ren`) e tratamento de caminhos com `/` preservando a estrutura de arquivos.
- **Gate de Foco de Janela**: Isolamento estrito de eventos de teclado, impedindo que janelas em segundo plano consumam entrada indevida.

---

### 4. 🧠 Ecossistema Neural — *Runtime_Neural (16 Núcleos)*
ABI de syscalls estabilizada (IDs `100–119`) dividida em 16 módulos especializados:

`Math` • `Logic` • `Router` • `Synth` • `Memory` • `Knowledge` • `Dialog` • `Emotion` • `Learn` • `Reason` • `Persona` • `Pattern` • `Embed` • `Fichas` • `Social` • `Catalogo`

Esses módulos alimentam a assistente **Existência v10**, provendo:
- Memória contínua e personalidades adaptativas.
- Raciocínio em cadeia (*Chain-of-Thought*).
- Biblioteca de associação semântica e livro de convívio social.

---

## 🧱 Arquitetura do Sistema

| Camada | Escopo | Descrição Téscnica |
| :--- | :--- | :--- |
| **Ring 0** | Kernel | Arquitetura 64 bits, IDT/GDT, VMM, drivers PS/2, VESA LFB, FAT32 volátil. |
| **Ring 3** | Processos | Executáveis ELF, IPC por memória compartilhada, gerenciador de janelas WM. |
| **Gráficos** | GUI Engine | VESA LFB 32 bpp, *Double Buffering*, renderizador de fontes 8x8 próprio. |
| **Rede** | Stack | Pilha TCP/IP + TLS 1.2 desenvolvida em C *freestanding* executada em Ring 3. |
| **IA Engine**| IA Neural | Dispatcher neural com 16 famílias de syscalls e ABI congelada. |

---

# 2. Compile o Kernel 3.1 e os binários ELF de Ring 3
make clean && make

📜 Licença e Filosofia
Este projeto é software livre, licenciado sob a GNU General Public License v3.0 (GPL-3.0).
Permissão concedida para qualquer modificação, estudo, uso e distribuição, sem nenhuma restrição. Todo o conhecimento aqui contido pertence à comunidade.
