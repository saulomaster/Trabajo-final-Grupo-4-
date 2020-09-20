# **INFORME FINAL **

CURSO: Programación 2020              
Profesor: Ing. geógrafo Roy Yali

INTEGRANTES:

| Alumno   | Código  |
| ------------ | ------------ |
|  Bautista Rojas Jhon Edmirando  |  18160045 |
|  Flores Marcos Lucy Angie Sharon |  18160193  |
| Hinostroza Camones Gabriela Isabel  |  18160037  |
|  Pfuturi Huarcaya Saul Oscar | 18160041  |
| Ysuhuaylas Segovia Luis Eduardo  | 18160044  |


**Table of Contents**

[TOCM]

[TOC]

# PORCENTAJE SIN INGRESOS PROPIOS

### 1. Librerias a usar

library(ggplot2)
library(tidyverse)

### 2. Cargamos el archivo csv

data <- read.csv("C:/Users/NEYSSER/Desktop/ciclo _5 _IG/porcentaje_sin_ingresos_propios.csv")

head(data)

### 3. Usamos gather 
Para fundir o agrupar los datos de las zonas rurales y urbanas en comparacion con los años y su porcentaje luego dejamos intacto los datos a nivel nacional, y con head podemos visualizar los datos parcialmente

df <- data %>% 
  gather(key = "Zona", value = "Porcentaje", -Año, -Nac_Mujeres, -Nac_Hombres)
head(df)

### 4. Agrupamos los datos de las zonas rurales y urbanas 
Para colocarlo en el aes y agruparlo, darle color según ello..

ggplot(df, aes(Año, Porcentaje, group = Zona)) +
  geom_line(aes(color = Zona))

------------


##### Puede visualizar el siguiente grafico, dando click [aquí](https://github.com/saulomaster/Trabajo-final-Grupo-3-/blob/master/grafico%201.jpg "Grafico 1")

------------


Como vemos en el grafico nos hes un poco dificil relacionar o encontrar cual linea representa cada valor. Asi que modificaremos el grafico con las distintas posibilidades que nos ofrece ggplot

n <- ggplot(df, aes(Año, Porcentaje, group = Zona)) +

#### # Configuramos la forma de la linea segun la zona
  geom_line(aes(linetype = Zona, color= Zona)) + 
  
#### # Añadimos los puntos en cada año y le damos transparencia
  geom_point(alpha = .2) + 
  
#### # Escribimos el titulo y la descricion de la imagen al pie de pagina
  ggtitle("PERÚ: Mujeres y hombres sin ingresos propios, según ámbito geográfico") + 
  labs(caption="Realizado por: Grupo B - Fuente: INEI (datos abiertos)") +

#### # Los nombres de los ejes (en el caso del eje "x" lo dejamos en blanco porque se sobreentiende)
  xlab("") +
  ylab("Personas sin ingresos propios (porcentaje)") +
  
#### # Configurar el "x" para que se separe cada año
  scale_x_continuous(breaks = seq(2007, 2018, 1)) +
  
#### # De manera similar el eje "y", intervalo considerado sera cada 5
  scale_y_continuous(
    breaks = seq(10, 60, 5)
  ) +
  
#### # Las siguiente lineas son anotaciones hechas a mano para cada linea 
  annotate("text",
           x = 2017,
           y = 45,
           label = "Rural Mujeres",
           color = "#792427",
           fontface = "bold",
           size = 4.5) +
  
  annotate("text",
           x = 2015,
           y = 30,
           label = "Urbano Mujeres",
           color = "#545058",
           fontface = "bold",
           size = 4.5) +
  
  annotate("text",
           x = 2016,
           y = 14,
           label = "Rural Hombres",
           color = "#D73815",
           fontface = "bold",
           size = 4.5) +
  
  annotate("text",
           x = 2017.8,
           y = 11,
           label = "Urbano Hombres",
           color = "#2B818E",
           fontface = "bold",
           size = 4.5) +
  
####  # Selecionamos un tema de fondo  # Selecionamos un tema de fondo
  theme_bw() +
  
#### # Eliminamos la leyenda  # Eliminamos la leyenda
  theme(legend.position = "none")

n

------------


##### ## Puede visualizar el siguiente gráfico dando click [aquí](https://github.com/saulomaster/Trabajo-final-Grupo-3-/blob/master/grafico%202.jpg "Grafico 2")

------------


### 5 Conclusión

Podria ser que las mujeres de area rural son mayor porcentaje en cuanto a no contar con ingresos propios aunque en los ultimos años tiene tendendia a bajar. Los hombres de las zonas rurales y urbanas tienen un aproximado porcentaje en los ultimos años

#Relación Temperatura-Salinidad en el mar peruano

### 1. Cargamos las librerias que usaremos en este ejercicio.

library(rgee)
library(mapview)
library(mapedit)
library(tidyverse)
library(sf)
library(raster)

###2. Iniciamos nuestra cuenta

ee_Initialize()

###3. Creamos un area de interés 
Mediante el uso de mapview y editmap, luego selecionamos todos sus atributos

area <- mapview() %>% editMap()
area_sf <- area$all

------------


Para ver el area seleccionada puede dar click [aquí](https://github.com/saulomaster/Trabajo-final-Grupo-3-/blob/master/imagen_1.jpeg)

------------


###4. Convertimos el área a un objeto Earth Engine
Exportamos como un objeto Earth Engine

area_ee <- sf_as_ee(area_sf)

###5. Llamamos a la colección de imagen de la base de datos de Google Earth Engine 
Para extraer la temperatura y salinidad a nivel de la superficie del mar. Filtramos la media de los datos.

imagen <- ee$ImageCollection("HYCOM/sea_temp_salinity")$
  filterDate(ee$Date("2018-01-01"), ee$Date("2018-01-31"))$
  mean()

###6. Descargamos las imagenes a nuestro directorio local mediante ee_as_raster

area_stack <- ee_as_raster(image = imagen,
                           region = area_ee$geometry())


###7. Selecionamos las bandas que usaremos; en este caso la temperatura y salinidad superficial

area_temp <- area_stack[["water_temp_0"]]
mar_salinidad <- area_stack[["salinity_0"]]

Aunque las unidades de la temperatura esten en grados centrigrados podemos obtener la escala apropiada con los valores que nos ofrece la tabla de las bandas del dataset en GEE

Puede visualizar el area seleccionada dando click [aquí](https://github.com/saulomaster/Trabajo-final-Grupo-3-/blob/master/imagen_5.jpeg)

###8. Tanto para la temperatura como la salinidad y lo guardamos con los mismos nombres

puntos <- mapview(area_sf) %>% editMap()
puntos_sf <- punto$all

------------


**Puede ver los puntos seleccionados dando click [aquí](https://github.com/saulomaster/Trabajo-final-Grupo-3-/blob/master/imagen_2.jpeg)**

------------


###9. Con estas lineas codigo extraemos los valores de latitud y longitud del "sf" y guardamos con el mismo nombre

puntos_sf <- puntos_sf %>%
  mutate(lon = unlist(map(puntos_sf$geometry,1)),
         lat = unlist(map(puntos_sf$geometry,2)))

###10. Extraemos los datos de temperaturas y salinidad 
Con el archivo raster descargado de GEE a los puntos seleccionados anteriormente. Los nombres de las columnas lo definimos añadiendole despues del simbolo del dolar

puntos_sf$temp <- raster::extract(mar_temperatura, puntos_sf)
puntos_sf$sal <- raster::extract(mar_salinidad, puntos_sf)

###11. Revisamos los datos creados anteriormente

puntos_sf <- st_read("C:/Users/NEYSSER/Desktop/trabajo final progra/puntos_sf.shp")

puntos_sf

###12. Convertimos a un tibble y seleccionamos las colummnas que usaremos

puntos_data <- puntos_sf %>%
  as_tibble() %>%
  dplyr::select(lat, lon, temp, sal)
head(puntos_data)

###13. Podemos comparar graficamente la relacion entre las variables

pairs(puntos_data)

cor(puntos_data)

###14. Realizamos una regresion lineal comparando la latitud con la salinidad

regresion_salinidad <- lm(lat ~ sal, data = puntos_data)
summary(regresion_salinidad)

###15. Ploteamos la temperatura vs latitud y añadimos la linea de regresion 

plot(puntos_data$sal, puntos_data$lat,
     main = "Latitud vs Salinidad superficial",
     ylab = "Latitud",
     xlab = "Salinidad (psu)",
     col = "blue")
abline(regresion_salinidad)

plot(salinidad)

------------


puede visualizar el siguiente grafico dando click [aquí](https://github.com/saulomaster/Trabajo-final-Grupo-3-/blob/master/imagen_4.jpeg)

------------


###16. Ahora para la latitud con la temperatura

regresion_temperatura <- lm(lat ~ temp, data = puntos_data)
summary(regresion_temperatura)

###17. Ploteamos la temperatura y añadimos la linea de regresion 

plot(puntos_data$temp, puntos_data$lat,
     main = "Latitud vs Temperatura superficial",
     ylab = "Latitud",
     xlab = "Temperatura (°C)",
     col = "blue")
abline(regresion_temperatura, col = "red")

plot(temperatura)

------------


**Puede visualizar el siguiente gráfico dando click [aquí](https://github.com/saulomaster/Trabajo-final-Grupo-3-/blob/master/imagen_3.jpeg)
**
------------





###End

