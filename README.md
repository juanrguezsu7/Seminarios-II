# Seminario Mundos Virtuales. Introducción a la programación de gráficos 3D.
En este seminario vamos a responder a 15 preguntas, relacionando en los casos que sea posible con los contenidos que se han explicado en la sesión de mundos virtuales.

## Integrantes del grupo
- Igor Dragone - alu0101469652@ull.edu.es 
- Álvaro Fontenla León - alu0101437989@ull.edu.es
- Sergio Pérez Lozano - alu0101473260@ull.edu.es
- Juan Rodríguez Suárez - alu0101477596@ull.edu.es

## Preguntas

### 1. Qué funciones se pueden usar en los scripts de Unity para llevar a cabo traslaciones, rotaciones y escalados.
Se usan métodos y funciones de las clases Transform y RigidBody:
- Para traslaciones tenemos `transform.position`, que permite acceder o asignar directamente una nueva posición en el espacio 3D, `transform.Translate(Vector3)`, que mueve el objeto una distancia específica en la dirección indicada por el vector y finalmente `Rigidbody.MovePosition(Vector3)`, para objetos físicos.
- Para rotaciones tenemos `transform.rotation`, que permite establecer la rotación del objeto directamente mediante un Quaternion, `transform.Rotate(Vector3)`, que aplica una rotación adicional en los ejes X, Y y Z relativos al objeto y `Rigidbody.MoveRotation(Quaternion)`, para objetos físicos. También existe `transform.RotateAround(Vector3 point, Vector3 axis, float angle)`, donde el objeto rota alrededor de un punto específico.
- Para escalados tenemos `transform.localScale`, que permite ajustar la escala del objeto en los ejes X, Y y Z

Todos estos métodos modifican el sistema de referencia del objeto al que se le aplican, multiplicando cada vértice por la matriz de transformación.
### 2. Como trasladarías la cámara 2 metros en cada uno de los ejes y luego la rotas 30º alrededor del eje Y?. Rota la cámara alrededor del eje Y 30ª y desplázala 2 metros en cada uno de los ejes. ¿Obtendrías el mismo resultado en ambos casos?. Justifica el resultado
En primer lugar vamos a mover la cámara:
- Para trasladarla 2 metros en cada uno de los ejes usamos transform.Translate(2,2,2)
- Para rotar 30 grados alrededor del eje Y usamos transform.Rotate(0,30,0)

En el segundo caso realizamos las mismas operaciones pero en orden invertido. Sin embargo, el resultado que obtendríamos no sería el mismo: al rotar primero, los ejes locales del objeto cambiarán, luego al desplazar el objeto, la posición final no coincidirá con la del primer caso.
### 3. Sitúa la esfera de radio 1 en el campo de visión de la cámara y configura un volumen de vista que la recorte parcialmente.
Para realizar este ejercicio, primero creamos una esfera de radio 1 (desde GameObject). En segundo lugar, nos colocamos en la cámara y modificamos el ángulo de apertura o FOV, pasando del valor 60 a 12. De esta forma, la esfera se ve parcialmente recortada:

![3](./img/3.png)
### 4.Sitúa la esfera de radio 1 en el campo de visión de la cámara y configura el volumen de vista para que la deje fuera de la vista.
Esta vez, para que la esfera se quede fuera de la vista, modificamos el atributo far de la cámara, pasando de 1000 a 9. Dado que la esfera está a una distancia de 10, esta no aparecerá en la vitsa aún estando en el campo de visión de la cámara:

![4](./img/4.png)
### 5. Como puedes aumentar el ángulo de la cámara. Qué efecto tiene disminuir el ángulo de la cámara.
Para modificar el ángulo de visión de la cámara en Unity, podemos ajustar el campo de visión (Field of View o FOV) en la propiedad `Camera.fieldOfView` si la cámara está en modo de proyección en perspectiva. Al aumentar el valor de `fieldOfView`, ampliamos el ángulo de visión, lo que genera un efecto de "alejamiento" en la imagen, permitiendo ver más contenido de la escena. Al disminuir el valor, el ángulo se reduce, generando un efecto de "zoom in", que hace que los objetos ocupen más espacio en la pantalla.

```csharp
Camera.main.fieldOfView = 60; // Ejemplo para establecer el ángulo en 60º
```
### 6. Es correcta la siguiente afirmación: Para realizar la proyección al espacio 2D, en el inspector de la cámara, cambiaremos el valor de projection, asignándole el valor de orthographic
La afirmación es falsa. Esto se debe a que la proyección al espacio 2D se puede realizar sin depender del tipo de proyección utilizada. No es estrictamente necesario cambiar la configuración de la cámara ya que la proyección 2D puede lograrse utilizando una cámara en modo `Perspective`.
### 7. Especifica las rotaciones que se han indicado en los ejercicios previos con la utilidad quaternion.
Para realizar rotaciones en Unity utilizando cuaterniones se modifica la propiedad `rotation` en `transform`, usando la función `Euler` de la clase `Quaternion`. Esta función 
devuelve la rotación especificada mediante los parámetros evitando problemas como el 'Gimbal lock'.
Para rotar un objeto 30º alrededor del eje Y:
```csharp
transform.rotation = Quaternion.Euler(0, 30, 0);
```
### 8. ¿Como puedes averiguar la matriz de proyección en perspectiva que se ha usado para proyectar la escena al último frame renderizado?.
En Unity, se puede acceder a la matriz de proyección en perspectiva de una cámara a través de la propiedad `Camera.projectionMatrix`. Esta matriz define cómo se proyectan los objetos 3D en la pantalla en modo perspectiva.
```csharp
Matrix4x4 perspectiveMatrix = Camera.main.projectionMatrix;
```
Para la visualización de la matriz basta con imprimirla en consola.
### 9. ¿Como puedes averiguar la matriz de proyección en perspectiva ortográfica que se ha usado para proyectar la escena al último frame renderizado?.
La matriz de proyección utilizada por la cámara puede accederse mediante la propiedad Camera.projectionMatrix. Si la cámara está configurada en modo ortográfico (es decir, sin perspectiva), esta propiedad contiene la matriz de proyección ortográfica. Para verificar el tipo de proyección, se puede consultar la propiedad Camera.orthographic, que devuelve true si la cámara está en modo ortográfico y false si está en modo perspectiva.
### 10. ¿Cómo puedes obtener la matriz de transformación entre el sistema de coordenadas local y el mundial?.
El componente Transform permite acceder a la matriz de transformación entre el sistema de coordenadas local y el sistema mundial. Esto se puede lograr mediante la propiedad Transform.localToWorldMatrix. Esta matriz permite transformar posiciones y vectores de las coordenadas locales del objeto a las coordenadas del mundo.
### 11. Cómo puedes obtener la matriz para cambiar al sistema de referencia de vista
Para cambiar al sistema de referencia de la vista se utiliza la matriz de vista de la cámara, que convierte las coordenadas del mundo en coordenadas de vista. En Unity, esta matriz es accesible a través de Camera.worldToCameraMatrix.
### 12. Especifica la matriz de la proyección usado en un instante de la ejecución del ejercicio 1 de la práctica 1.
Gracias al siguiente código empleado, pudimos obtener la matriz de proyección usado en un instante de la ejecución.
```csharp
using UnityEngine;


public class seminario : MonoBehaviour
{
    void Start()
    {
        // Obtener la matriz de proyección de la cámara
        Matrix4x4 projectionMatrix = Camera.main.projectionMatrix;
       
        // Imprimir la matriz en la consola
        Debug.Log("Matriz de Proyección en ejecución:");
        for (int row = 0; row < 4; row++)
        {
            string rowValues = "";
            for (int col = 0; col < 4; col++)
            {
                rowValues += projectionMatrix[row, col].ToString("F4") + "\t";
            }
            Debug.Log(rowValues);
        }
    }
}

```
![12](./img/Captura12.PNG)

### 13. Especifica la matriz de modelo y vista de la escena del ejercicio 1 de la práctica 1.
Se calcula la matriz de modelo (para pasar del sistema local al mundial) y la matriz de vista (para pasar del sistema mundial al de vista) de la escena del ejercicio 1 de la práctica 1. Para ello, se ha creado un script que imprime ambas matrices en la consola.
```csharp
public class e1 : MonoBehaviour
{
  // Start is called before the first frame update
  void Start()
  {
    Transform objetoTransform = GetComponent<Transform>();
    Matrix4x4 modelMatrix = objetoTransform.localToWorldMatrix;

    Camera camara = Camera.main;
    Matrix4x4 viewMatrix = camara.worldToCameraMatrix;

    Debug.Log("-----Model Matrix-----");
    PrintMatrix(modelMatrix);
    Debug.Log("-----View Matrix-----");
    PrintMatrix(viewMatrix);
  }

  void PrintMatrix(Matrix4x4 matrix)
  {
    for (int i = 0; i < 4; i++)
    {
      Debug.Log(matrix.GetRow(i));
    }
  }
}
```
![13](./img/13.png)
### 14. Aplica una rotación en el start de uno de los objetos de la escena y muestra la matriz de cambio al sistema de referencias mundial.
```csharp
public class e1 : MonoBehaviour
{
  // Start is called before the first frame update
  void Start()
  {
    Transform objetoTransform = GetComponent<Transform>();
    objetoTransform.Rotate(0, 90, 0);
    Matrix4x4 modelMatrix = objetoTransform.localToWorldMatrix;

    Camera camara = Camera.main;
    Matrix4x4 viewMatrix = camara.worldToCameraMatrix;

    Debug.Log("-----Model Matrix-----");
    PrintMatrix(modelMatrix);
    Debug.Log("-----View Matrix-----");
    PrintMatrix(viewMatrix);
  }

  void PrintMatrix(Matrix4x4 matrix)
  {
    for (int i = 0; i < 4; i++)
    {
      Debug.Log(matrix.GetRow(i));
    }
  }
}
```
Se observa que, al contrario de la anterior (donde no había rotación y por tanto los ejes estaban alineados con el sistema mundial), ahora las columnas que corresponden a cómo serían los ejes del objeto en el sistema mundial están cambiadas. Esto se debe a que la rotación ha cambiado el sistema de referencia del objeto (rotación de 90º en el eje Y).
![14](./img/14.png)
### 15. ¿Como puedes calcular las coordenadas del sistema de referencia de un objeto con las siguientes propiedades del Transform:?: Position (3, 1, 1), Rotation (45, 0, 45)
Teniendo ese sistema de referencia (cilindro), vamos a calcular cuáles serían las coordenadas para ese sistema de un punto arbitrario ((1, 2, 3) en el mundo). Para ello primero obtenemos la matriz de transformación al sistema mundial del cilindro y luego multiplicamos el punto por la matriz, obteniendo las coordenadas del punto en el sistema de referencia del cilindro.
```csharp
public class e2 : MonoBehaviour
{
  // Start is called before the first frame update
  void Start()
  {
    Transform objetoTransform = GetComponent<Transform>();
    Matrix4x4 worldToLocalMatrix = objetoTransform.worldToLocalMatrix;

    Vector3 point = new Vector3(1, 2, 3);
    
    Debug.Log("Old position: " + point);
    Debug.Log("New position: " + worldToLocalMatrix.MultiplyPoint(point));
  }
}
```
![15](./img/15.png)
### 16. Investiga sobre los modelos de iluminación que aplica Unity y resume las relaciones existentes con el modelo explicado en clase.

Unity utiliza modelos de iluminación similares a los estudiados en clase:

1. **Iluminación Local**: Unity emplea el modelo de Lambert para la luz difusa y el modelo visto en clase para la luz especular. Estos modelos calculan la luz que afecta directamente a un objeto sin considerar reflejos de otros objetos, como se explicó en clase.

2. **Iluminación Global**: Unity aplica una iluminación global que simula la luz reflejada entre superficies, parecido a la radiosidad. Esto permite que los objetos se iluminen tanto por fuentes directas como por la luz rebotada en el entorno.

3. **Materiales**: Los materiales en Unity especifican cómo un objeto refleja la luz, permitiendo configuraciones de brillo y rugosidad.

### 17. Indica las funciones de la API de Unity más importantes respecto a la iluminación

1. **Componente `Light`**:
   - `Light.type`: Define el tipo de luz (Directional, Point, Spot, Area), lo que determina cómo se emite y afecta la iluminación en la escena.
   - `Light.intensity`: Controla la intensidad o brillo de la luz. Permite ajustar cómo de intensa es la luz emitida.
   - `Light.range`: Para luces puntuales y focos (`spot lights`), define el alcance de la luz.
   - `Light.color`: Establece el color de la luz.

2. **Iluminación Global y Ambiente**:
   - `RenderSettings.ambientMode`: Permite seleccionar el modo de iluminación ambiental (Skybox, Color, Gradient).
   - `RenderSettings.ambientIntensity`: Controla la intensidad de la luz ambiental aplicada en la escena, afectando el tono y la luz difusa general.
   - `RenderSettings.ambientLight`: Define el color de la luz ambiental cuando `ambientMode` está en Color.

3. **Sombras**:
   - `Light.shadows`: Activa o desactiva las sombras para una fuente de luz. También permite seleccionar el tipo de sombra (None, Hard, Soft).
   - `Light.shadowStrength`: Ajusta la opacidad de las sombras.

4. **Materiales y Shaders**:
   - `Material.SetFloat`, `Material.SetColor`: Permiten modificar propiedades del material, como el brillo especular, el color de la emisión y la rugosidad.
   - `Shader.globalIlluminationFlags`: Controla cómo un material interactúa con la iluminación global.
