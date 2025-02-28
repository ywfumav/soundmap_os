# soundmap_os
This repository helps automate "soundwalk" process by integrating gps data (tcx file) and sound logging file (txt file).

## Hardware Dependencies
1. GPS module (Google Pixel Watch 2)
2. Sound Level Meter (NTi XL2 / Pixel 9 Pro Fold)

## Software Dependencies

### Python Packages
Refer to [**requirements**](/requirement.txt)
```
pip install folium
pip install pandas
pip install matplotlib
```

## Viewing the Interactive Soundwalk Map (HTML)

To view the interactive soundwalk map (html file), please download the html files from the repository. 
The files can be rendered using a Google Chrome or Microsoft Edge browser. 

## Operation

### main_average.py

This python file import `Logging File` and averages the following measurement readings for the entire logging duration.
1. dBA 
2. dBC 

```mermaid 
graph LR

A[XL2 Logging File] --> B[main_average.py]
B --> C[averaged dBA]
B --> E[averaged dBC]

```

### main_plot.py 

This python file import `Logging file` and plot the following measurement readings into line graphs (`png` format) for the entire logging duration using `matplotlib`.
1. dBA 
3. dBC 

```mermaid 
graph LR

A[XL2 Logging File] --> B[main_plot.py]
B --> C[dBA time log plot]
B --> E[dBC time log plot]


```

### main_rta.py

This python file import `RTA file` and plot the following averaged measurement readings into bar graphs (`png` format). The dataset is recorded in `dBZ` and the python file is using a band weighting offset scale to calculate the corresponding `dBA` and `dBC` RTA graphs. 

**For offset values less than 0, the bar will not show any data**

```mermaid 
graph LR

A[XL2 RTA File] --> B[main_rta.py]
B --> C[dBA]
B --> D[dBA Max]
B --> E[dBC]
B --> F[dBC Max]

```

### main_soundwalk.py

This python file will import both `Logging File` and `GPS file`. It will retrieve `dBA_dt` value from logging file. The program will then compare the corresponding dBA's timestamp from Logging File to GPS file. If there are corresponding timestamp, the program will map the value into `openmap`. The program will output an `html` file to illustrate the sound mapping over a district.

Below details the colour scale for the Noise Level Mapping

![alt text](media/colourmap.png)

```mermaid 
graph LR

A[XL2 Logging File] --> B[main_rta.py]
C[GPS File] --> B
B --> D[Retrieve dBA data]
D --> E[Check timestamp between logging and gps files]
E --> F[if timestamp = True, save dBA]
F --> G[colour scale]
G --> F
F --> H[Create Map] 

```

### dBA and dBC offset_values

The offset tables below are used derive SPL dBA and dBC values from recorded dBZ

```
A_WEIGHT_OFFSETS = {
    6.3:  -86.4,  8.0:  -77.8,  10.0: -70.4,  12.5: -63.4,
    16.0: -56.7,  20.0: -50.5,  25.0: -44.7,  31.5: -39.4,
    40.0: -34.6,  50.0: -30.2,  63.0: -26.2,  80.0: -22.5,
    100.0:-19.1,  125.0:-16.1,  160.0:-13.4,  200.0:-10.9,
    250.0:-8.6,   315.0:-6.6,   400.0:-4.8,   500.0:-3.2,
    630.0:-1.9,   800.0:-0.8,   1000.0:0.0,   1250.0:0.6,
    1600.0:1.0,   2000.0:1.2,   2500.0:1.3,   3150.0:1.2,
    4000.0:1.0,   5000.0:0.5,   6300.0:-0.1,  8000.0:-1.1,
    10000.0:-2.5, 12500.0:-3.8, 16000.0:-6.6, 20000.0:-9.3
}

C_WEIGHT_OFFSETS = {
    6.3:  -21.3,  8.0:  -17.7,  10.0: -14.3,  12.5: -11.2,
    16.0: -8.5,   20.0: -6.7,   25.0: -5.0,   31.5: -3.0,
    40.0: -2.0,   50.0: -0.5,   63.0: -0.8,   80.0: -0.2,
    100.0:-0.3,   125.0:-0.2,   160.0:-0.1,   200.0: 0.0,
    250.0: 0.0,   315.0: 0.0,   400.0: 0.0,   500.0: 0.0,
    630.0: 0.0,   800.0: 0.0,   1000.0:0.0,   1250.0:0.0,
    1600.0:-0.1,  2000.0:-0.2,  2500.0:-0.3,  3150.0:-0.5,
    4000.0:-0.8,  5000.0:-1.3,  6300.0:-2.0,  8000.0:-3.0,
    10000.0:-4.4, 12500.0:-6.2, 16000.0:-8.5, 20000.0:-11.2
}
```

