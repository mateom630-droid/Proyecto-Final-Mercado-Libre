# 📊 Proyecto RappiPlus: de datos a decisiones de negocio

Análisis de extremo a extremo del servicio **RappiPlus** para apoyar decisiones de negocio basadas en datos. El proyecto cubre calidad de datos, rentabilidad, funnel de conversión, retención por cohortes y un experimento A/B sobre el checkout.

---

## 🎯 Objetivo

Responder, con datos, seis preguntas clave del negocio:

1. 🔍 ¿Podemos confiar en los datos? (calidad de datos)
2. 💰 ¿El negocio es rentable? (revenue, costos y profit)
3. 🛒 ¿Dónde se pierden los usuarios? (funnel de conversión)
4. 🔁 ¿Los usuarios regresan? (retención por cohortes)
5. 🧪 ¿Los cambios generan impacto? (test estadístico)
6. 📊 ¿Cómo comunicar los resultados? (dashboard en BI)

---

## 🗂️ Datos utilizados

| Fuente | Contenido |
|--------|-----------|
| `rappiplus_orders_raw.csv` | Pedidos, precios, descuentos y revenue |
| `rappiplus_catalog.csv` | Costos de productos, categorías y proveedores |
| `rappiplus_marketing_spend.csv` | Inversión en marketing por canal y país |
| `events` (SQL) | Eventos de los usuarios dentro de la plataforma |
| `users` y `user_activity` (SQL) | Registro y actividad de usuarios |
| `experiment_checkout_ui.csv` | Resultados del experimento A/B en el checkout |

---

## 🛠️ Herramientas

- **Python**: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`
- **SQL (PostgreSQL)** con `SQLAlchemy` para consultar `events`, `users` y `user_activity`
- **BI** para el dashboard final
- **Jupyter Notebook**

---

## 🔬 Metodología y hallazgos

### 1. Calidad de datos
- Conversión de fechas a formato `datetime`.
- Eliminación de pedidos con cantidad no válida (≤ 0).
- Verificación de consistencia de montos: la diferencia máxima entre `monto_total` y el monto recalculado fue de 0.01 (redondeo), por lo que se consideró aceptable.
- Eliminación de **100 pedidos duplicados** (25,046 filas → 24,946 `id_pedido` únicos).
- Estandarización de nombres de país (`mexico` → `Mexico`, etc.).
- Valores nulos menores al 2% por columna: se conservaron por no afectar el análisis.

### 2. Rentabilidad

| Indicador | Valor |
|-----------|-------|
| Ingreso total | 51,966,981.56 |
| Costo total | 43,124,069.01 |
| Inversión en marketing | 2,871,843.53 |
| **Profit** | **5,971,069.02** |
| Ticket promedio | 2,083.18 |
| Cantidad promedio por orden | 7.12 |

- ✅ El negocio es **rentable**.
- Producto más vendido: `Laptop-Gaming-16GB`.
- Inversión de marketing repartida de forma pareja entre canales: social (~918 mil), organic (~914 mil) y paid_search (~863 mil).

### 3. Funnel de conversión (SQL)

| Etapa | Usuarios únicos |
|-------|-----------------|
| first_visit | 7,796 |
| select_item | 7,582 |
| add_to_cart | 7,634 |
| begin_checkout | 7,208 |
| add_payment_info | 6,250 |
| purchase | 6,240 |

- Mayor pérdida: de **begin_checkout a add_payment_info** (conversión de 86.71%).
- Tasa de conversión final: **80.04%**.

### 4. Retención por cohortes (SQL)
- Cohortes mensuales de enero a mayo de 2025, con retención en las semanas 1, 2 y 3.
- La retención es **estable, alrededor de 40%–44%**, sin una caída marcada entre cohortes ni entre semanas.

### 5. Experimento A/B del checkout
- **H₀:** no hay diferencia en la tasa de conversión entre la UI original y la nueva.
- **H₁:** la nueva UI genera una mayor tasa de conversión.
- Test: **Chi-cuadrado** de independencia, con α = 0.05.

| Variante | Usuarios | Conversión |
|----------|----------|------------|
| Control | 4,965 | 15.69% |
| Tratamiento | 5,035 | 16.29% |

- χ² = 0.6178, **p = 0.4319** → **no se rechaza H₀**.
- La diferencia observada (+0.60 pp) puede deberse al azar.

---

## ✅ Recomendaciones

1. **No implementar todavía la nueva UI del checkout**: el experimento no muestra un efecto significativo.
2. Continuar el experimento con más usuarios para obtener resultados más concluyentes.
3. Analizar segmentos (país, dispositivo) donde la nueva UI pueda tener impacto.
4. Considerar un rediseño con un cambio más pronunciado.
5. Priorizar la mejora del paso **begin_checkout → add_payment_info**, donde se concentra la mayor pérdida de usuarios.

---

## 📁 Estructura del repositorio

```
├── S12_Estudiante_Proyecto_Final.ipynb   # Notebook con todo el análisis
├── README.md
└── data/                                 # (opcional) datasets limpios exportados
    ├── orders_clean.csv
    ├── catalog_clean.csv
    └── marketing_clean.csv
```

## ▶️ Cómo ejecutarlo

1. Clona el repositorio:
   ```bash
   git clone https://github.com/TU_USUARIO/TU_REPOSITORIO.git
   ```
2. Instala las dependencias:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy sqlalchemy psycopg2-binary jupyter
   ```
3. Abre el notebook:
   ```bash
   jupyter notebook
   ```

> **Nota:** las consultas SQL (pasos 3 y 4) requieren acceso a la base de datos del curso. Configura las credenciales mediante variables de entorno; no las incluyas en el código.

---

## 👤 Autor

**Tu nombre** · [LinkedIn](https://www.linkedin.com/) · [GitHub](https://github.com/)
