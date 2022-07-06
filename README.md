Para poder utilizar estos codigos ya se tiene que tener un video previamente grabado en donde se muestre la cara de la persona 

Para realizar el reconocimiento facial necesitaremos de los rostros de las personas que deseemos reconocer. Estos rostros deberán denotar variedad de expresiones como: felicidad, tristeza, aburrimiento, sorpresa, entre otros. Otro aspecto que deben tener estas imágenes es la variación de condiciones de luz, que las personas lleven lentes o que no los lleven, incluso que cierren los ojos o guiñen uno.

Es recomendable que se realice la recolección de estas imágenes en el escenario o ambiente en donde se vaya a aplicar el reconocimiento facial. Toda esta variedad de imágenes que se obtenga de los rostros contribuirá al desempeño de los algoritmos que usemos el día de hoy (y claro, según vayas experimentado podrás añadir o quitar algunas de estas condiciones que he nombrado).

Lo siguiente que haremos es preparar los datos para entrenar 
Antes de proceder con el entrenamiento es necesario tener cada una de las imágenes con una etiqueta asociada a la persona a la que pertenecen los rostros. Por ello, por ejemplo, cuando leamos la carpeta ‘Persona 1’ todas esas imágenes se les asignará etiqueta 0, luego a todas imágenes de los rostros de ‘Persona 2’ se asignará 1, de ‘Persona 3’ se asignará 2, y así sucesivamente. Con cada una de estas etiquetas le haremos saber al computador que las imágenes les corresponden a personas diferentes.

Entonces en otro script llamado entrenandoRF.py prepararemos este proceso, es decir las imágenes y etiquetas para poder entrenarlos empleando: Eigenfaces, fisherfaces y Local Binary Patterns Histograms. 

Entrenado (Código)
Ahora que tenemos una idea de estos métodos para reconocimiento facial usando OpenCV, pasaremos al código. Podremos elegir entonces de los siguientes métodos (recuerda comentar los 2 que no vayas a usar):

face_recognizer = cv2.face.EigenFaceRecognizer_create()
face_recognizer = cv2.face.FisherFaceRecognizer_create()
face_recognizer = cv2.face.LBPHFaceRecognizer_create()

Una vez que hayas elegido uno de estos métodos es hora de entrenar, y descuida que solo necesitarás una línea de código.

# Entrenando el reconocedor de rostros
print("Entrenando...")
face_recognizer.train(facesData, np.array(labels))

Guardando el modelo obtenido

Una vez entrenado el modelo que nos servirá para reconocer rostros es posible almacenarlo. ¿Para qué nos serviría esto?, pues el entrenamiento toma tiempo, y más si son varias personas que se busca reconocer, es por eso que es mejor guardar el modelo obtenido para luego leerlo en otro script, así nos ahorramos el volverlo a entrenar.
Para almacenar el modelo usaremos write, dentro de los paréntesis deberemos especificar el nombre que deseamos asignarle, junto con la extensión XML o YAML.

# Almacenando el modelo obtenido
#face_recognizer.write('modeloEigenFace.xml')
#face_recognizer.write('modeloFisherFace.xml')
face_recognizer.write('modeloLBPHFace.xml')
print("Modelo almacenado...")

NOTA 1: Algo que hay que tener en cuenta es que al aplicar predict, obtendremos una etiqueta y su valor de confianza asociado. Vamos a tener muy en cuenta este segundo valor ya que dependiendo este podremos aceptar o descartar el reconocimiento.

NOTA 2: Los valores (valor de confianza) más bajos o cercanos a cero querrá decir que el rostro a identificar tiene mucha similitud al de los entrenados, mientras que al tener valores muchos más altos el grado de similitud será más pobre por lo que podremos descartarlos. Cabe destacar que dependiendo de cada método obtendremos distintos valores de confianza que podremos comparar.

Si descomentamos las líneas 8 y 13 correspondientes al método EigenFaces, vamos a obtener una etiqueta con el rostro más parecido a los entrenados junto con su valor de confianza. Tendremos que realizar algunas pruebas para determinar un umbral para que aparezca el nombre de la persona identificada y para valores mayores a este umbral será considerado como rostro desconocido.
Una vez que hemos probado el código y hemos experimentado, procederemos a descomentar las líneas 37 a 43.


# EigenFaces
if result[1] < 5700:
    cv2.putText(frame,'{}'.format(imagePaths[result[0]]),(x,y-25),2,1.1,(0,255,0),1,cv2.LINE_AA)
    cv2.rectangle(frame, (x,y),(x+w,y+h),(0,255,0),2)
else:
    cv2.putText(frame,'Desconocido',(x,y-20),2,0.8,(0,0,255),1,cv2.LINE_AA)
    cv2.rectangle(frame, (x,y),(x+w,y+h),(0,0,255),2)
    
    
Línea 38: Comparo el valor de confianza que está en result[1] con 5700, de tal modo que valores menores a este se etiquete con el nombre de la persona y valores mayores a este se etiquete como desconocido. El umbral 5700 que he escogido ha sido a través de prueba y error, por lo que tu debes cambiar este con la experimentación que realices con respecto a tus resultados.

Línea 39 y 40: En la línea 39 vamos a visualizar el nombre de la persona correspondiente al valor de la etiqueta almacenada en result[0], para ello usaremos imagePaths para obtener los nombres. En la línea 40 dibujaremos un rectángulo de color verde alrededor del rostro detectado.
Y con esto hemos logrado tener el reconocimiento facial terminado en video 

Por último, para poder hacer el reconocimiento facial en video streaming lo único que le tenemos que agregar a nuestro código es una simple línea y descomentar la de video, la cual nos va permitir el reconocimiento en vivo 

#cap = cv2.VideoCapture(0)
#cap = cv2.VideoCapture(0,cv2.CAP_DSHOW)
#cap = cv2.VideoCapture('Video.mp4')

Ahora si con esto terminado ya podemos tener el reconocimiento en video streaming 
