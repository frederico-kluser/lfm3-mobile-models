# Liquid AI LFM3 + d1 · suite offline móvel

Artefactos verificados por SHA-256 para a app [offline-ai-mobile](https://github.com/frederico-kluser/offline-ai-mobile).

## Incluído
- `manifest.json` — 11 bundles, tamanhos, SHA-256 e origem oficial.
- `SHA256SUMS.txt` — somas de todos os ficheiros conhecidos + licença.
- `LICENSE` — LFM Open License v1.0 (10 574 B; SHA-256 `4d28ca14…0068c`).
- `README.md` — tabela de artefactos, verificação e limitações.

## Licença
Os pesos estão sob a **LFM Open License v1.0** (Apache-2.0-like, com patentes). O §5 limita o uso comercial a organizações com receita anual do grupo **< 10 000 000 USD**; acima disso é necessária licença comercial da Liquid AI. O código da app é MIT.

## Notas
- Nenhum ficheiro recomendado para telemóvel excede o limite de 2 GiB por ativo.
- Os subgrafos ONNX com nomes repetidos entre bundles são publicados com prefixo; a app usa o Hugging Face como origem para ONNX.
- O `d1-omni-600M` é LFM2 legado, não LFM3.
- Pesos completos podem ser obtidos com `tools/publish_lfm3_release.sh` (ver README).
