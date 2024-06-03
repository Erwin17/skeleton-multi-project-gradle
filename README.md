# Configuracion Multiples Proyecto

1. Creamos la carppeta que contendra todos nuestros proyecto o en la carpeta principal del repositorio:
 [composition-microservice]
2. Creamos las carpetas de utilidad [api, util] y la carpeta principla que contendra todos los microservicios:
   
> mkdir util
   
> mkdir api

> mkdir mmicroservices

3. Entando en la raiz de nuestra carpeta, creamos un archivo settings.gradle el cual apunta a la estructura de los microservicios que se crearon en el paso anterior

```
---settings.gradle---
include ':api'
include ':util'
include ':microservices:product-service'
include ':microservices:review-service'
include ':microservices:recommendation-service'
include ':microservices:product-composite-service'
```
4.  Luego entando en la raiz, copiamos los archivos ejecutable de Gradle que se generaron a partir de los proyectos para que podamos reutilizarlos para compilaciones de multiples proyectos:
```
cp -r microservices/product-service/gradle .
cp microservices/product-service/gardlew .
cp microservice/product-service/gardlew.bat .
cp microservices/product-service/.igignore .
```
5. Procedemos a eliminar los gradle en los proyectos de los microservicios ya que no lo necesitamos, la compilacion se hara desde la raiz:

```
find microservices -depth -name "gradle" -exec rm -rfv "{}" \; 
find microservices -depth -name "gradlew*" -exec rm -fv "{}" \; 
```

## NOTA
En este paso se elimina de todos los microservicios los siguientes directorios:
gradle: Contiene el wrapper.


6. Procedemos a ejecutar la compilacion de los microservicios:
```
./gradlew build
```
## NOTA
Si queremos incluir el proyecto de manera local en un microservicio, tenemos que hacerlo en el archivo build.gradle en la seccion de dependencias, como se muestra a continuacion:
```
dependencies {
implementation project (':api')
...
...
}
```

7. ejecutar todos los microservicios:
```
java -jar microservices/product-composite-service/build/libs/*.jar &
java -jar microservices/product-service/build/libs/*.jar &
java -jar microservices/recommendation-service/build/libs/*.jar &
java -jar microservices/review-service/build/libs/*.jar &
```
