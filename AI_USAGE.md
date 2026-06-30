# Uso de Inteligencia Artificial

## Herramienta utilizada
- **Claude (Anthropic)** - Asistente de codificacion con capacidades agénticas

## Generado con asistencia de IA

### Pipeline de datos
- Diagnostico y solucion del error `RuntimeError: Dataset scripts are no longer supported` al cargar `tweet_sentiment_multilingual` y `conll2002`, fijando `datasets<3.0` y reiniciando el runtime
- Implementacion de `emb_corpus`, `buscar` (coseno con embeddings normalizados) y `ndcg_medio` para Sentence-BERT, reutilizando `qrels` y `ndcg_at_k` del Lab 3
- Funcion `tokeniza` (truncation, padding a max_length, max_length=128) y `compute_metrics` (accuracy + F1-macro) para la Parte B
- Funcion `tokeniza_y_alinea`: alineacion de etiquetas BIO a subpalabras via `word_ids`, etiquetando solo la primera subpalabra de cada palabra y `-100` en el resto, para la Parte C


### Documentacion
- `AI_USAGE.md` - registro del uso de IA en este laboratorio

---

## Realizado por el estudiante
- Definición de funciones utilizadas en laboratorios anteriores (`qrels`, `ndcg_at_k`, `_rel` del Lab 3)
- Elección de consultas de prueba (A.3) y de frases propias (B.5)
- Verificación del nombre de columnas tras la carga de los datasets de sentimiento vía JSONL

### Decisiones de diseño
- Cambiar epocas para el Fine Tuning de 2 a 50
- Decision de submuestrear `train` a ~2000 ejemplos en la Parte B para mantener el entrenamiento dentro de los limites de tiempo/VRAM de la T4
- Uso de `save_strategy='no'` en `TrainingArguments` para evitar saturar el disco de la sesion con checkpoints intermedios

### Analisis
- Interpretacion de la matriz de confusion de sentimiento: identificacion de **neutral** como la clase mas dificil (menor recall y F1), con confusion bidireccional hacia negative y positive
- Justificacion de F1-macro como criterio mas robusto que accuracy
- Analisis del domain shift al aplicar el clasificador de sentimiento (entrenado con tuits) sobre noticias del corpus chiapaneco: riesgo de falsos positivos por carga lexica negativa en notas puramente expositivas
- Interpretacion del reporte de `seqeval` en NER: identificacion de **MISC** como la categoria mas dificil (F1 0.61 vs. 0.86-0.94 en LOC/ORG/PER)
- Justificacion de por que `seqeval` es la metrica correcta para NER y no accuracy por token

### Documentacion academica
- Redaccion final de las respuestas justificadas solicitadas en el notebook (A: linea base vs. afinado; B.4: clase mas dificil y criterio de evaluacion; C.4: justificacion de seqeval y tipo de entidad mas costosa)