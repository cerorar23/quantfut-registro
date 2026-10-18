# QuantFut — registro público de predicciones

Este repositorio es la prueba de que QuantFut no revisa sus predicciones después de los partidos.

## Cómo funciona (comprometer y revelar)

1. **Antes de los partidos** se publica aquí el *compromiso* del día: el recibo `AAAA-MM-DD.receipt.json` y una fila en `LEDGER.md`. Contienen solo hashes SHA-256 y recuentos, no las predicciones. La fecha del commit en GitHub es el sello de tiempo externo.
2. **Cuando terminan todos los partidos del lote** (y pasan al menos 4 horas del último saque) se publica `AAAA-MM-DD.json`, el lote completo en JSON canónico.
3. Cada recibo enlaza con el anterior: `chain_sha256 = sha256(prev_sha256 + payload_sha256)`. Alterar un lote pasado rompe todos los hashes posteriores.

## Cómo verificarlo

Para cada día revelado:

```
sha256(contenido exacto de AAAA-MM-DD.json) == payload_sha256 del recibo
sha256(prev_sha256 + payload_sha256)        == chain_sha256 del recibo
```

y `prev_sha256` debe ser el `chain_sha256` del día anterior (el primero parte de 64 ceros).
Los días aún sin revelar enlazan igualmente por su recibo; su JSON aparecerá al terminar los partidos.

Verificador del proyecto: `python -m qf.publish.hashchain verify` sobre una copia de este repositorio.

Este repositorio lo escribe automáticamente el servidor de QuantFut; no se aceptan contribuciones.
