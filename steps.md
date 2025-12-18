# Introducción a spaCy

Entramos en **spacy.io/usage** y nos encontramos con los primeros pasos para instalar y usar spaCy correctamente.

---

## 1️⃣ Actualización de herramientas básicas

```bash
pip install -U pip setuptools wheel
```

### ¿Qué hace?

Actualiza herramientas básicas del ecosistema Python.

### Herramientas

* **pip**
  Gestor de paquetes. Sirve para instalar librerías:

  ```bash
  pip install X
  ```

* **setuptools**
  Permite que muchos paquetes se *construyan* e instalen correctamente.

* **wheel**
  Define el formato `.whl`, que son paquetes ya compilados → instalación más rápida y menos errores.

---

## 2️⃣ Instalación de spaCy

```bash
pip install -U spacy
```

📦 Instala o actualiza la librería **spaCy**.

---

## 3️⃣ Descarga de un modelo de lenguaje

```bash
python -m spacy download en_core_web_sm
```

### `python -m spacy`

Ejecuta spaCy como un **módulo**, no como un script cualquiera.

Esto garantiza que se usa el spaCy del **entorno activo** (muy importante cuando trabajamos con `venv`).

### `download`

Comando interno de spaCy para descargar modelos.

### `en_core_web_sm`

Modelo de lenguaje:

* **en** → inglés
* **core** → modelo general
* **web** → entrenado con texto web
* **sm** → *small* (pequeño, rápido, menos preciso)

### Incluye

* Tokenizador
* POS tagging
* Named Entity Recognition (NER)
* Dependencias sintácticas

---

## 4️⃣ Cargando el modelo

```python
nlp = spacy.load("en_core_web_sm")
```

Esto crea el objeto principal de trabajo en spaCy: `nlp`.

---

## 5️⃣ Containers en spaCy

Los **containers** son objetos de spaCy que contienen gran cantidad de información sobre un texto y permiten trabajar con NLP de forma estructurada.

---

### `Language`

Es el **objeto principal de spaCy**.

* Representa el pipeline completo (tokenizer + componentes NLP)
* Normalmente lo llamamos `nlp`

```python
import spacy
nlp = spacy.load("en_core_web_sm")
```

👉 A partir de `Language` se crean todos los demás objetos.

---

### `Doc`

Representa un **texto procesado** por spaCy.

* Contiene tokens, frases, entidades, dependencias, etc.
* Es el objeto central del análisis NLP

```python
doc = nlp("This is a sentence")
```

👉 Un `Doc` es **inmutable** (no se pueden añadir tokens después).

---

### `Token`

Representa una **palabra o símbolo individual** dentro de un `Doc`.

```python
token = doc[0]
print(token.text)
```

Incluye información como:

* Forma original
* Lema
* POS tag
* Dependencias sintácticas

---

### `Span`

Representa un **fragmento continuo de texto** dentro de un `Doc`.

```python
span = doc[0:2]
```

Usos típicos:

* Entidades nombradas
* Frases
* Sub-secciones del texto

👉 Un `Span` **no copia texto**, solo referencia al `Doc` original.

---

### `SpanGroup`

Agrupa múltiples `Span` relacionados dentro de un `Doc`.

Ejemplo:

* Todas las entidades detectadas
* Spans creados por un componente custom

```python
doc.spans["my_group"] = [span1, span2]
```

---

### `Lexeme`

Representa una **entrada del vocabulario**, no una aparición concreta.

* Vive en `nlp.vocab`
* Comparte información entre tokens iguales

```python
lexeme = nlp.vocab["apple"]
```

Incluye:

* Forma normalizada
* Flags (is_alpha, is_stop, etc.)

👉 Un `Lexeme` **no pertenece a un texto específico**.

---

### `DocBin`

Contenedor eficiente para **serializar y guardar muchos `Doc`**.

* Muy usado en entrenamiento
* Más rápido y compacto que guardar texto plano

```python
doc_bin = DocBin()
doc_bin.add(doc)
```

---

### `Example`

Representa un **ejemplo de entrenamiento**.

Contiene:

* Un `Doc` predicho
* Un `Doc` con la anotación correcta

Se usa para:

* Entrenar
* Evaluar modelos

```python
from spacy.training import Example
example = Example.from_dict(doc, annotations)
```

---

## 🧠 Resumen mental rápido

* `Language` → el motor (pipeline)
* `Doc` → texto procesado
* `Token` → palabra individual
* `Span` → trozo de texto
* `SpanGroup` → grupo de spans
* `Lexeme` → vocabulario global
* `DocBin` → muchos docs empaquetados
* `Example
