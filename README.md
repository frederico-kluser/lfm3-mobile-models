# Liquid AI LFM3 · mirror móvel

Artefactos **verificados por SHA-256** da família
[Liquid AI LFM3](https://www.liquid.ai/blog/lfm3) e dos decisores
[d1](https://huggingface.co/LiquidAI/d1-2B) para a app
[`offline-ai-mobile`](https://github.com/frederico-kluser/offline-ai-mobile).

- **Release:** `models-lfm3-suite-v1`
- **Manifesto:** [`manifest.json`](manifest.json)
- **Somas:** [`SHA256SUMS.txt`](SHA256SUMS.txt)
- **Licença dos pesos:** [`LICENSE`](LICENSE) — **LFM Open License v1.0**

> **Licença (importante):** o código da app é MIT, mas os pesos LFM3/d1 estão
> sob a LFM Open License v1.0. É Apache-2.0-like, com patentes, mas o §5 limita
> o uso comercial a organizações cujo grupo tenha receita anual **inferior a
> 10 000 000 USD**. Acima desse limiar é necessária licença comercial da
> Liquid AI. O ficheiro `LICENSE` é a cópia integral (10 574 bytes, SHA-256
> `4d28ca14…0068c`) distribuída com os modelos.

## Artefactos

| Bundle | Ficheiro principal | Projetor / sidecar | Tamanho aprox. |
|---|---|---|---|
| `lfm3-1.7b-q4_0` | `LFM3-1.7B-QAD-Q4_0.gguf` | — | 1,05 GB |
| `lfm3-1.7b-q4_k_m` | `LFM3-1.7B-Q4_K_M.gguf` | — | 1,10 GB |
| `lfm3-1.7b-q8_0` | `LFM3-1.7B-Q8_0.gguf` | — | 1,85 GB |
| `lfm3-vl-2b-q4_k_m` | `LFM3-VL-2B-Q4_K_M.gguf` | `mmproj-LFM3-VL-2B-Q8_0.gguf` | 1,30 GB |
| `lfm3-audio-2b-q4_k_m` | `LFM3-Audio-2B-Q4_K_M.gguf` | `mmproj-LFM3-Audio-2B-Q8_0.gguf` | 1,40 GB |
| `lfm3-omni-2b-q4_k_m` | `LFM3-Omni-2B-Q4_K_M.gguf` | `mmproj-LFM3-Omni-2B-Q4_K_M.gguf` | 1,39 GB |
| `d1-2b-q4_k_m` | `d1-2B-Q4_K_M.gguf` | `mmproj-d1-2B-Q8_0.gguf` | 1,30 GB |
| `d1-omni-2b-q8_0` | `d1-Omni-2B-Q8_0.gguf` | `mmproj-d1-Omni-2B-Q8_0.gguf` | 2,17 GB |
| `d1-omni-600m-legacy-q8_0` | `d1-omni-600M-Q8_0.gguf` | `mmproj-d1-omni-600M-Q8_0.gguf` | 0,67 GB |
| `d1-2b-onnx-q4f16` | `decoder_q4f16.onnx` (+ `.onnx_data`) | `vision_encoder_fp16.onnx` (+ `.onnx_data`) | 1,94 GB |
| `d1-omni-2b-onnx-q4f16` | `decoder_q4f16.onnx` (+ `.onnx_data`) | `vision_encoder_fp16.onnx`, `audio_encoder_fp16.onnx` (+ `.onnx_data`) | 2,45 GB |

Os GGUF e os `mmproj` oficiais são anexados com o **nome original**. Os
subgrafos ONNX com nomes repetidos entre bundles são anexados com prefixo
(`d1-2b-onnx-q4f16_…`, `d1-omni-2b-onnx-q4f16_…`) para não colidirem; a app
descarrega os ONNX diretamente do Hugging Face e usa o release para os GGUF.

## Descarregar e verificar

```bash
# Um ficheiro (exemplo)
curl -L -O https://github.com/frederico-kluser/lfm3-mobile-models/releases/download/models-lfm3-suite-v1/LFM3-1.7B-QAD-Q4_0.gguf

# Verificar todas as somas
sha256sum -c SHA256SUMS.txt
```

No macOS: `shasum -a 256 -c SHA256SUMS.txt`.

O GitHub Releases impõe **2 GiB por ativo**, mas nenhum ficheiro recomendado
para telemóvel chega a esse limite; não há splits artificiais. O limite de
2 GiB foi verificado por
[`tools/publish_lfm3_release.sh`](tools/publish_lfm3_release.sh).

## Publicar / atualizar o release

Requisitos: [`huggingface-hub`](https://huggingface.co/docs/huggingface_hub)
(`hf`), [GitHub CLI](https://cli.github.com/) autenticado, `sha256sum`/`shasum`.

```bash
# 1. Descarrega de LiquidAI/*, verifica tamanhos/hashes e publica no release
bash tools/publish_lfm3_release.sh

# 2. Só valida o manifesto e os limites, sem publicar
bash tools/publish_lfm3_release.sh --dry-run
```

O script:

1. descarrega os ficheiros do Hugging Face para `staging/`;
2. calcula/confirma o SHA-256 e compara com `manifest.json`/`SHA256SUMS.txt`;
3. recusa qualquer ficheiro ≥ 2 GiB;
4. cria/atualiza o release `models-lfm3-suite-v1` com `gh release upload`;
5. anexa `manifest.json`, `SHA256SUMS.txt`, `LICENSE` e `README.md`.

## Limitações conhecidas

- O `d1-omni-600M` **não é LFM3**: o GGUF declara `architecture=lfm2` e foi
  lançado com a família d1-3B. Fica aqui apenas como fallback de baixa RAM.
- A via GGUF do d1 (`/v1/systemone`) só expõe `noul`, `choice` e `score`;
  spans, `range`, `box` e `boxes` exigem o bundle ONNX.
- O `chat_template.jinja` (13 363 B) e o `config.json` (5 802 B) do
  `d1-Omni-2B-ONNX` ainda não têm SHA-256 publicado; ficam sem linha em
  `SHA256SUMS.txt`.
- O desempenho real em Android/iOS varia com o SoC, o thermal throttling e a
  versão do llama.cpp; mede com o perfilador da app.

## Upstream

- <https://huggingface.co/LiquidAI/LFM3-1.7B-GGUF>
- <https://huggingface.co/LiquidAI/LFM3-VL-2B-GGUF>
- <https://huggingface.co/LiquidAI/LFM3-Audio-2B-GGUF>
- <https://huggingface.co/LiquidAI/LFM3-Omni-2B-GGUF>
- <https://huggingface.co/LiquidAI/d1-2B-GGUF>
- <https://huggingface.co/LiquidAI/d1-Omni-2B-GGUF>
- <https://huggingface.co/LiquidAI/d1-2B-ONNX>
- <https://huggingface.co/LiquidAI/d1-Omni-2B-ONNX>
- <https://github.com/ggml-org/llama.cpp/releases/tag/b12088>
