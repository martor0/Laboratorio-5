# Laboratorio-5 Variabilidad de la Frecuencia Cardiaca usando la Transformada Wavelet
# Introducción 

La frecuencia cardíaca es un parámetro fisiológico fundamental que refleja la actividad del sistema nervioso autónomo (SNA), el cual regula el equilibrio entre la acción simpática —asociada con la respuesta de alerta y el aumento de la frecuencia cardíaca y la acción parasimpática relacionada con el reposo y la disminución de la misma. Las variaciones naturales que se presentan entre los intervalos consecutivos R–R del electrocardiograma (ECG) son conocidas como variabilidad de la frecuencia cardíaca (HRV, Heart Rate Variability), constituyen un indicador no invasivo de la función autonómica del corazón.

El análisis de la HRV permite identificar alteraciones en la modulación cardíaca y se ha convertido en una herramienta diagnóstica y de investigación en áreas como la fisiología, la cardiología y la ingeniería biomédica. Tradicionalmente, este análisis se realiza en el dominio del tiempo o de la frecuencia mediante la Transformada de Fourier; sin embargo, dichas técnicas no capturan adecuadamente la evolución temporal de los componentes de frecuencia. Por ello, se empleara métodos tiempo–frecuencia como la Transformada Wavelet Continua (CWT), que nos ayudara a estudiar la distribución espectral de la señal cardíaca con alta resolución temporal y frecuencial.

En la presente práctica de laboratorio se implementara un procesamiento digital completo de una señal ECG utilizando Python, con el fin de analizar la HRV a partir de los intervalos R–R y evaluar las variaciones en las bandas de baja (0.04–0.15 Hz) y alta frecuencia (0.15–0.4 Hz). Para ello, se aplicaran tecnicas de filtrado digital, detección de picos R y análisis tiempo–frecuencia mediante la transformada Wavelet. Los resultados obtenidos permiten observar cómo la actividad simpática y parasimpática se manifiestan en diferentes regiones espectrales, evidenciando la utilidad de las herramientas de procesamiento digital de señales para el estudio del control autonómico del corazón.

# Fundamento teorico 
1. Sistema Nervioso Autónomo (SNA) y control de la frecuencia cardíaca

El sistema nervioso autónomo (SNA) es el encargado de regular de manera involuntaria las funciones fisiológicas esenciales del organismo, entre ellas la actividad cardíaca. Este sistema se divide en dos ramas principales: el sistema simpático y el sistema parasimpático.

El sistema simpático se activa en situaciones de estrés o alerta, liberando catecolaminas (como la adrenalina), lo que incrementa la frecuencia cardíaca, la contractilidad del miocardio y la presión arterial. Por el contrario, el sistema parasimpático, mediado por el nervio vago y la liberación de acetilcolina, promueve estados de reposo y recuperación, reduciendo la frecuencia cardíaca y favoreciendo la estabilidad del ritmo sinusal.

El balance dinámico entre ambas ramas determina el comportamiento del ritmo cardíaco en reposo y durante estímulos externos. Un predominio simpático se asocia con menor variabilidad entre los intervalos cardíacos, mientras que un predominio parasimpático genera mayor variabilidad. Por ello, la evaluación de la variabilidad de la frecuencia cardíaca (HRV) se ha convertido en un indicador indirecto del estado funcional del SNA.

<img width="1080" height="1350" alt="image" src="https://github.com/user-attachments/assets/dd9a31d8-5494-4d88-aebb-b05050810eea" />

2. Variabilidad de la Frecuencia Cardíaca (HRV)

La variabilidad de la frecuencia cardíaca (HRV) se define como la fluctuación temporal entre intervalos consecutivos R–R del electrocardiograma (ECG). Dichas variaciones reflejan la capacidad del sistema cardiovascular para adaptarse a cambios fisiológicos y ambientales.

El análisis de la HRV puede realizarse en distintos dominios:

Dominio del tiempo: Se calculan parámetros estadísticos a partir de los intervalos R–R, como la media (mean RR) y la desviación estándar (SDRR o SDNN). Un valor alto de desviación estándar indica una mayor modulación vagal y, por tanto, un buen equilibrio autonómico.

Dominio de la frecuencia: Se estudia la distribución de energía en bandas espectrales específicas:

Baja frecuencia (LF, 0.04–0.15 Hz): relacionada con la actividad simpática y parasimpática combinada.

Alta frecuencia (HF, 0.15–0.4 Hz): asociada principalmente a la modulación parasimpática (vagal).

La razón LF/HF es usada como indicador del balance autonómico.

El análisis de HRV proporciona información valiosa sobre la regulación cardiovascular, pudiendo emplearse en el diagnóstico de estrés, fatiga, patologías cardíacas o alteraciones del sistema nervioso autónomo.

<img width="750" height="337" alt="image" src="https://github.com/user-attachments/assets/35e33912-f2bf-426f-9942-3341f9d4d58d" />

3. Transformada Wavelet

La transformada Wavelet es una herramienta matemática utilizada para analizar señales no estacionarias, es decir, aquellas cuyos componentes de frecuencia cambian con el tiempo, como las señales biológicas.
A diferencia de la Transformada de Fourier, la Wavelet permite obtener información simultáneamente en el dominio del tiempo y de la frecuencia, mediante el uso de funciones base denominadas wavelets madre.

En el análisis de la HRV, la transformada Wavelet permite:

Detectar variaciones transitorias en las bandas de baja frecuencia (LF) y alta frecuencia (HF).

Evaluar la evolución temporal de la actividad simpática y parasimpática.

Visualizar la potencia espectral mediante un escalograma o espectrograma Wavelet, que representa cómo cambia la energía de la señal en diferentes frecuencias a lo largo del tiempo.

El uso de la Transformada Wavelet mejora la interpretación fisiológica de la HRV y complementa los análisis tradicionales en los dominios del tiempo y la frecuencia.

# Adquisición de la señal ECG

Para el desarrollo del laboratorio se realizó la adquisición de la señal electrocardiográfica (ECG) de un compañero en condiciones de reposo, con el fin de analizar la variabilidad de la frecuencia cardíaca (HRV).

1. Instrumentación utilizada

Electrodos de superficie: 3 electrodos colocados según la configuración estándar para derivación DI (brazo derecho, brazo izquierdo y pierna derecha como referencia).

Sistema de adquisición: módulo de adquisición de datos compatible (BiTalino).

Software de captura: aplicación de registro o script en Python.

Frecuencia de muestreo: entre 250 Hz y 500 Hz, suficiente para capturar las componentes relevantes del ECG.

Duración del registro: aproximadamente 5 minutos, en estado de reposo.

2. Procedimiento experimental

# Preparación del sujeto:

Se limpió la piel con alcohol isopropílico para reducir la impedancia.

Se colocaron los electrodos en las posiciones anatómicas correspondientes.

Se solicitó al sujeto permanecer sentado, relajado y sin hablar durante el registro.

# Configuración del sistema de adquisición:

Se verificó la conexión de los cables y la polaridad de los electrodos.

Se seleccionó la frecuencia de muestreo y el canal de entrada.

Se inició la captura continua de la señal ECG por un periodo de 5 minutos.

 # Registro de la señal:

Se almacenaron los valores del voltaje en función del tiempo en un archivo para su posterior procesamiento digital.

3. Consideraciones de calidad de señal

Para garantizar una señal ECG confiable y de buena calidad se tuvieron en cuenta las siguientes precauciones:

Utilizar electrodos bien adheridos y cables de baja impedancia.

Mantener un entorno sin movimiento y con mínimas interferencias eléctricas.

Asegurar una correcta referencia a tierra del sistema de adquisición.

Verificar que no existan pérdidas de conexión o ruido por mala colocación de los electrodos.

4. Resultado esperado de la adquisición

El resultado fue una señal ECG con complejos P-QRS-T claramente visibles, lo que permitió aplicar posteriormente el proceso de filtrado digital y la detección de picos R para calcular los intervalos R-R y analizar la HRV.

Se evitó la proximidad de fuentes de ruido electromagnético (celulares, monitores, cables de corriente).

<img width="365" height="645" alt="image" src="https://github.com/user-attachments/assets/7484a5ef-f936-40e2-979d-6ba0a00c5816" />

<img width="358" height="356" alt="image" src="https://github.com/user-attachments/assets/6c3ababc-1268-4077-b8ed-0a32175046b7" />

<img width="243" height="177" alt="image" src="https://github.com/user-attachments/assets/9731ac26-1883-4908-804c-07ae5483e291" />

## Diagrama de flujo

![image](https://github.com/user-attachments/assets/4580bdea-f575-46fa-8ea0-8af3424bc487)

# Preprocesamiento de la señal 

1. Filtrado digital paso banda

La señal ECG original fue obtenida de un archivo de texto (señal ecg 1 Gaby.txt) y muestreada a 1000 Hz.
Posteriormente, se aplicó un filtro paso banda Butterworth de cuarto orden (IIR) con frecuencias de corte en 0.5 Hz y 40 Hz, diseñado con la función butter() de la librería scipy.signal.

El filtro atenúa:

Componentes de baja frecuencia (< 0.5 Hz), asociadas a la deriva de línea base.

Componentes de alta frecuencia (> 40 Hz), debidas al ruido eléctrico y muscular (EMG).

<img width="625" height="348" alt="image" src="https://github.com/user-attachments/assets/6e574c2d-3f09-4ae6-8019-316bb7ebbd14" />

El filtrado permitió obtener una señal ECG limpia, conservando las morfologías P, QRS y T con mínima distorsión.

2. Detección de picos R

Una vez filtrada la señal, se aplicó un algoritmo de detección de picos basado en la función find_peaks() de scipy.signal, la cual permite localizar máximos locales cumpliendo condiciones de amplitud mínima y distancia temporal mínima entre picos.

<img width="982" height="143" alt="image" src="https://github.com/user-attachments/assets/c02781bf-79c9-4351-9721-504f325f3d0f" />

El parámetro distance=FS*0.6 asegura que no se detecten picos falsos dentro del mismo complejo QRS (equivale a un máximo fisiológico de 100 BPM).

El height se calcula dinámicamente a partir de la media y desviación estándar de la señal, para adaptarse a distintos niveles de amplitud.

3. Cálculo de los intervalos R-R

Con las posiciones de los picos R detectados, se calcularon los intervalos R-R en segundos como la diferencia temporal entre picos consecutivos:

<img width="976" height="134" alt="image" src="https://github.com/user-attachments/assets/d55fc458-82b3-452a-a9b7-f499306ac7ae" />

Estos intervalos representan la variabilidad instantánea de la frecuencia cardíaca.
A partir de ellos, se obtiene una serie temporal irregular que luego será interpolada para el análisis con la transformada Wavelet.

4. Resultados del preprocesamiento

El resultado del preprocesamiento fue una señal ECG filtrada, sin ruido de línea base ni interferencias, con los picos R correctamente identificados y listos para el análisis de HRV.

La siguiente figura ilustra el proceso completo:

ECG original (ruidoso)

ECG filtrado con picos R detectados

Serie de intervalos R-R

RR interpolado para análisis de frecuencia

# Resultados e interpretación 

<img width="1919" height="1018" alt="image" src="https://github.com/user-attachments/assets/e427a328-be0f-4d40-b088-8b9321215251" />

* En la primera grafica vemos co,o se ve nuestra señal ECG original, a penas obtenida del sensor donde vemos un ritmo cardiaco normal y con la cual empezaremos a hacer el analisis siguientes.
* Para la segunda grafica teemos La deteccion de los picos R de la señal filtrada donde se calcula los intervalos de RR que nos muestra el tiempo entre dos latidos consecutivos, y con esta grafica podemos obtener tambien el analisis de la variabilidad del ritmo cardiaco.
* Seguimos con la serie de intervalos de RR enfrentando la parte original (linea verde ) con la interpolada (linea roja), en donde vemos que a los valores altos corresponden los latidos mas separados que nos dice que hay una frecuencia cardiaca baja y a los valores bajos son los latidos mas cercanos que nos muestran una frecuencia cardiaca alta, esto nos muestra cómo varía el ritmo cardíaco a lo largo del tiempo y la interpolación nos sirve para preparar los datos para el análisis espectral.
* Espectograma wavelet de (RR interpolado) este nos muestra mediante una colorimetria la potencia de la señal en cada instante y frecuencia, donde las zonas rojas y amarillas nos muestran que hay mas potencia y una mayor influencia de frecuencia en ese instante, donde vemos que las frecuencias bajas se asocian con la actividad simpática y parasimpática del sistema nervioso, y las frecuencias altas están relacionadas con la respiración y el control parasimpático, concuyendo en que el espectro nos permite visualizar cómo las componentes de baja y alta frecuencia del ritmo cardíaco varían con el tiempo, mostrando la dinámica del sistema nervioso autónomo.

4. Análisis de la Variabilidad de la Frecuencia Cardíaca (HRV)

A partir de la detección de los picos R en la señal ECG filtrada, se calcularon los intervalos R–R (RR), los cuales representan el tiempo entre latidos consecutivos.
Estos intervalos permiten analizar la variabilidad de la frecuencia cardíaca (HRV) en el dominio del tiempo, un indicador del equilibrio entre las ramas simpática y parasimpática del Sistema Nervioso Autónomo (SNA).

La media de los intervalos RR refleja el ritmo basal del corazón, mientras que la desviación estándar (SDNN) indica el grado de variabilidad de los latidos.
En la gráfica de intervalos RR (serie verde), se observa una modulación suave y periódica que corresponde a una actividad autonómica normal.
La serie interpolada (línea roja discontinua) conserva la tendencia de la señal original, validando el proceso de interpolación empleado.

<img width="651" height="229" alt="image" src="https://github.com/user-attachments/assets/81fa6c33-48c8-4107-8a75-7604c36862b1" />

<img width="476" height="160" alt="image" src="https://github.com/user-attachments/assets/44c6d1ac-52f9-4737-a7a2-3305e1a7a76a" />

5. Análisis en el Dominio Tiempo-Frecuencia (Transformada Wavelet)
   
<img width="872" height="528" alt="image" src="https://github.com/user-attachments/assets/e7a2ad49-9808-407b-8b73-a2257204bb49" />

La Transformada Wavelet se aplicó a la serie de intervalos R–R interpolada para analizar la variabilidad de la frecuencia cardiaca en el dominio tiempo-frecuencia. Esta herramienta permite observar cómo cambian las componentes de baja frecuencia (LF: 0.04–0.15 Hz), asociadas a la actividad simpática, y las de alta frecuencia (HF: 0.15–0.4 Hz), relacionadas con la modulación parasimpática.
En el espectrograma se aprecia mayor potencia en la banda LF, lo que indica una ligera predominancia simpática, mientras que las componentes HF reflejan la influencia respiratoria normal. La relación LF/HF ≈ 1.6 sugiere un equilibrio autonómico estable, mostrando que la Transformada Wavelet describe de forma efectiva las variaciones dinámicas del sistema nervioso autónomo a lo largo del tiempo.

# Conclusion
El análisis realizado permitió caracterizar la variabilidad de la frecuencia cardíaca (HRV) a partir de una señal ECG real mediante filtrado, detección de picos R y aplicación de la Transformada Wavelet. Los resultados mostraron una media RR de 0.913 s, SDNN de 0.068 s y una frecuencia cardíaca promedio de 66.1 bpm, indicando un ritmo sinusal estable y una variabilidad moderada.
El análisis tiempo–frecuencia evidenció una mayor potencia en la banda LF (0.8111) frente a la HF (0.0802), con una relación LF/HF ≈ 10.1, lo que refleja un predominio simpático y una posible respuesta de alerta del sistema nervioso autónomo.
En conclusión, la Transformada Wavelet permitió observar de manera dinámica la interacción entre las ramas simpática y parasimpática, demostrando su eficacia como herramienta para el estudio del control autonómico del corazón.

