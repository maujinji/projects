#  Topological Risk Radar: Detección Temprana de Crisis Financieras mediante TDA

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![Ripser](https://img.shields.io/badge/TDA-Ripser-orange.svg)](https://ripser.scikit-tda.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Este repositorio contiene la implementación de un marco cuantitativo avanzado para la **Detección de Riesgo Sistémico en Mercados Financieros** utilizando **Análisis Topológico de Datos (TDA)**. 

El proyecto demuestra empíricamente cómo la geometría subyacente de un mercado multivariado se contrae y pierde dimensionalidad bajo estrés, generando **Señales de Alerta Temprana (EWS)** hasta 250 días antes de colapsos históricos como la Burbuja Dot-com (2000) y la crisis subprime de Lehman Brothers (2008).



##  El Problema: La Ceguera de la Varianza Tradicional
Los modelos clásicos de gestión de riesgos asumen distribuciones normales y estacionariedad. En la práctica, los mercados financieros son sistemas dinámicos complejos. Las métricas basadas en momentos estadísticos unidimensionales (como la volatilidad tradicional) a menudo fracasan en capturar el riesgo de cola, disparando sus alarmas únicamente *después* de que el colapso ya ha ocurrido.

##  La Solución Topológica (TDA)
Basado en la investigación de Gidea & Katz (2018), este modelo abandona la predicción binaria a corto plazo y adopta un enfoque geométrico continuo:

1. **Espacios de Fase Multivariados:** En lugar de usar retrasos temporales (Teorema de Takens), construimos nubes de puntos dinámicas en $\mathbb{R}^4$ utilizando los retornos diarios concurrentes del **S&P 500, DJIA, NASDAQ y Russell 2000**.
2. **Homología Persistente:** Utilizamos complejos de Vietoris-Rips para rastrear la aparición de ciclos unidimensionales ($H_1$) en ventanas móviles de 50 días. El pánico financiero sincroniza los activos (comportamiento de manada), cerrando la topología de la nube de puntos.
3. **Paisajes de Persistencia ($L^1$):** Extraemos la norma integral $L^1$ del paisaje de persistencia, obteniendo un indicador continuo del "estrés topológico" del mercado.

---

##  Resultados y Evidencia Empírica

### 1. El Radar de Inestabilidad (Norma $L^1$)
Mientras el mercado opera con normalidad, la topología es ruido disperso (norma cercana a cero). Sin embargo, meses antes de un colapso, el mercado experimenta una transición de fase, generando picos masivos de correlación topológica.

<img width="1189" height="390" alt="image" src="https://github.com/user-attachments/assets/d77071ba-25cd-4baa-8602-d6b78e08cf2a" />


### 2. Validación Estadística Rigurosa (Mann-Kendall)
Para evitar los falsos positivos visuales, el algoritmo calcula la **varianza móvil de 500 días** sobre la señal topológica y aplica una prueba no paramétrica de tendencia sobre los **250 días previos** a eventos catastróficos conocidos.

Los resultados prueban que el pánico topológico es un precursor estadísticamente significativo:

| Crisis Histórica | Ventana de Prueba (Días Previos) | Coeficiente Kendall-Tau ($\tau$) | Valor-$p$ | Significación |
| :--- | :---: | :---: | :---: | :---: |
| **Burbuja Dot-com** (10/03/2000) | 250 | 0.1757 | $3.54 \times 10^{-5}$ | Altamente Significativa |
| **Quiebra Lehman** (15/09/2008) | 250 | 0.2953 | $3.68 \times 10^{-12}$ | Altamente Significativa |

---

##  Arquitectura del Código

El pipeline está completamente vectorizado y optimizado para ejecutarse en entornos locales sin colapsos de memoria:

* `FASE 1:` Ingesta multivariada concurrente vía `yfinance`.
* `FASE 2:` Ventanas móviles y cálculo de topología $H_1$ usando el motor C++ de `ripser`.
* `FASE 3:` Integración geométrica rápida de la norma $L^1$ a partir de diagramas de persistencia.
* `FASE 4:` Análisis estadístico y pruebas de hipótesis con `scipy.stats`.

---
