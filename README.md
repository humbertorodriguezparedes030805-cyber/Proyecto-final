# Sistema de Biofeedback para el Control del Estrés (EEG + ECG)

## Descripción

Este proyecto implementa un sistema de biofeedback en tiempo real capaz de estimar el nivel de relajación o carga cognitiva de un usuario mediante el análisis simultáneo de señales electroencefalográficas (EEG) y electrocardiográficas (ECG).

El sistema utiliza la potencia de la banda Alfa del EEG y métricas de Variabilidad de la Frecuencia Cardíaca (HRV) obtenidas a partir del ECG para generar un indicador visual tipo semáforo.

* 🟢 Verde: Estado relajado.
* 🟡 Amarillo: Estado de transición.
* 🔴 Rojo: Carga cognitiva o estrés.

---

## Objetivos

* Adquirir señales EEG y ECG utilizando un sistema BIOPAC MP36.
* Procesar señales biomédicas en ventanas deslizantes.
* Detectar picos R en tiempo real.
* Calcular métricas HRV (RMSSD y LF/HF).
* Calcular potencia espectral de la banda Alfa (8–13 Hz).
* Fusionar características fisiológicas provenientes de EEG y ECG.
* Implementar una interfaz gráfica para biofeedback.

---

## Hardware Utilizado

* BIOPAC MP36
* Electrodos desechables Ag/AgCl
* Computadora con Python 3.x

### Configuración EEG

* Derivación: Fz – Cz
* Frecuencia de muestreo: 1000 Hz

### Configuración ECG

* Derivación II
* Frecuencia de muestreo: 1000 Hz

---

## Procesamiento de EEG

La señal EEG es filtrada mediante un filtro Butterworth pasabanda de cuarto orden entre 1 Hz y 40 Hz.

Posteriormente se calcula la Densidad Espectral de Potencia (PSD) utilizando el método de Welch.

La potencia Alfa se obtiene integrando la PSD entre:

8 Hz ≤ f ≤ 13 Hz

La potencia Alfa se utiliza como indicador principal de relajación.

---

## Procesamiento de ECG

La señal ECG es filtrada entre 5 Hz y 20 Hz para mejorar la detección de los complejos QRS.

Posteriormente se detectan los picos R utilizando un umbral adaptativo.

A partir de los intervalos RR se calculan:

* Frecuencia cardíaca (BPM)
* RMSSD
* Relación LF/HF

Estas métricas permiten estimar la actividad del sistema nervioso autónomo.

---

## Índice de Relajación

El sistema combina información del EEG y ECG mediante:

Score = 0.7(Alpha / Alpha_ref) + 0.3(RMSSD / RMSSD_ref)

donde:

* Alpha_ref corresponde a la potencia Alfa obtenida durante la calibración.
* RMSSD_ref corresponde al RMSSD obtenido durante la calibración.

---

## Calibración

Antes de utilizar el sistema se realiza una fase de calibración de 60 segundos.

Condiciones recomendadas:

* Usuario sentado.
* Ambiente silencioso.
* Ojos cerrados.
* Sin movimientos bruscos.
* Respiración normal.

Durante esta fase se obtienen:

* Alpha_ref
* Alpha_std
* RMSSD_ref
* LFHF_ref

Estos parámetros son utilizados para personalizar los umbrales del semáforo.

---

## Interfaz Gráfica

La GUI muestra:

* Señal EEG filtrada.
* Señal ECG filtrada.
* Potencia Alfa en tiempo real.
* BPM.
* RMSSD.
* Score de relajación.
* Indicador visual tipo semáforo.

---

## Estructura del Proyecto

```text
Proyecto/
│
├── calibracion.py
├── semaforo.py
├── datos/
│   ├── eeg_reposo.acq
│   ├── eeg_estres.acq
│   └── prueba_60s.acq
│
├── figuras/
│   ├── gui.png
│   ├── eeg.png
│   └── ecg.png
│
└── README.md
```

## Librerías Utilizadas

```bash
pip install numpy
pip install scipy
pip install matplotlib
pip install bioread
```

## Ejecución

Modificar el nombre del archivo:

```python
archivo = "mi_archivo.acq"
```

y ejecutar:

```bash
python semaforo.py
```

## Resultados Esperados

Durante estados de relajación:

* Incremento de potencia Alfa.
* Mayor estabilidad cardíaca.
* Semáforo en verde.

Durante tareas cognitivas:

* Disminución de potencia Alfa.
* Cambios en HRV.
* Semáforo en rojo.

---

## Autor

Humberto Alonso Rodríguez Paredes

Ingeniería Biomédica

Universidad Veracruzana

---

## Referencias

1. Malik et al., Heart Rate Variability: Standards of Measurement, Physiological Interpretation and Clinical Use, 1996.

2. Welch, The Use of Fast Fourier Transform for the Estimation of Power Spectra, 1967.

3. Klimesch, EEG Alpha and Theta Oscillations Reflect Cognitive and Memory Performance, 1999.

4. Niedermeyer & Lopes da Silva, Electroencephalography: Basic Principles, Clinical Applications and Related Fields, 2005.
