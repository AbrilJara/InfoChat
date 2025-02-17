# InfoChat - chat interactivo

Esta aplicación fue desarrollada para la materia Introducción a la Ciberseguridad de la Facultad de Informática en la UNLP como trabajo final. La misma tiene como objetivo crear un chatbot vulnerable con el propósito de enseñar, de manera didáctica, cómo se pueden explotar las vulnerabilidades en los modelos de lenguaje. La idea surgió a partir de la aplicación [Prompt Airlines](https://promptairlines.com/), un ejemplo práctico de un chatbot vulnerable en forma de un ejercicio de CTF el cual sirvió como inspiración para desarrollar una versión más sencilla y adaptada a la comunidad hispanohablante.

# Arquitectura de la Aplicación

La arquitectura de la aplicación está diseñada para ser sencilla y fácil de entender, lo que facilita su uso en un entorno de aprendizaje. Está fue construida utilizando tecnologías modernas y ampliamente utilizadas en el desarrollo de chatbots, lo que la hace adaptable a diferentes escenarios en caso de quererse expandir su funcionalidad.

## LangChain
LangChain es un framework de Python que facilita el desarrollo de aplicaciones basadas en modelos de lenguaje. Permite la integración de diferentes componentes, como modelos de lenguaje, prompts, y funciones de procesamiento de texto. En esta aplicación, LangChain se utiliza para manejar la interacción entre el usuario y el modelo de lenguaje, así como para procesar y formatear las respuestas del chatbot. Su simplicidad permite que cada parte de la construcción de la App sea sencillo desde el desarrollo hasta la ejecución.

## Endpoints de Hugging Face
Hugging Face es una plataforma líder en el desarrollo y despliegue de modelos de AI open source. Esta plataforma permite usar endpoints los cuales proporcionan una API RESTful para acceder al modelo de lenguaje que querramos usar. Estos endpoints permiten enviar solicitudes de texto y recibir respuestas generadas por el modelo. La integración con Hugging Face nos permite aprovechar un modelo de lenguaje de alta calidad sin necesidad de entrenarlo localmente.

## Modelo de Lenguaje: mistralai/Mistral-7B-Instruct-v0.2

El modelo mistralai/Mistral-7B-Instruct-v0.2 es un modelo de lenguaje generacional, lo cual permite que a partir de un texto inicial se genere respuestas acorde a lo pedido por un contexto inicial. Este modelo es particularmente adecuado para nuestro chatbot porque puede generar respuestas detalladas y coherentes, sin ser de última generación lo que lo hace ideal para crear un entorno de aprendizaje interactivo ya que posee vulnerabilidades sensillas de comprender.

## Prompt especializado
El prompt especializado es un componente crucial de la aplicación el cual rige el actuar del modelo utilizado. Se ha diseñado de manera cuidadosa para cumplir con el objetivo de enseñar sobre inyecciones de prompt. El mismo lo podemos dividir en cuatro secciones, cada una con un propósito específico:

- La primera sección indica al modelo quien es y su fucnión, En este caso se indica que debe ayudar a aprender sobre las vulnerabilidades de las AI garantizando que sea dificil poder revelar sis datos y funcionamiento interno. Esto hace que su funcionalidad esté limitada a lo pedido y garantice que su función principal sea ayudar a entender sobre vulnerabilidades en AI. En esta sección se presenta el primer flag que la persona debe descubrir.
- La segunda sección indica reglas que debe cumplir internamente a la hora de formular su respuesta. Esta sección es privada y el bot buscará nunca revelarla. En esta sección se encuentra el segundo flag.
- En la tercera sección se indica que al modelo que posee dos funciones que puede ejecutar. La primera es pública y siempre se puede ejecutar, la segunda es privada y solo se puede ejecutar bajo ciertas circunstancias, además su nombre no es revelado nunca. La ejecución de las funciones es simulada por el modelo por lo cual las definiciones de las mismas son código sencillo de python. Si bien pueden crearse agentes que ejecuten código de python, me pareció que la complejidad de desarrollo no era necesaria teniendo en cuenta que con esta simulación es suficiente para demostrar mi punto. Aquí se encuentra el tercer y último flag a descubrir.
- En la última sección se indica el formato de que debe tener la respuesta. Esto garantiza que sea un poco más dificil vulnerar al modelo.

# Decisiones de Diseño

Para tener un interacción más placentera se decidió que el promt esté de forma redactada a la vista para quien haga el ejercicio, esto sirve para atenuar el sentimiento agobiante que puede llegar a sentir una persona que no sabe nada sobre promt injection y LLM permitiendo que no se canse y estrese facilmente no sabiendo por donde empezar, permitiendo poder ayudar a pensar que cosas podría preguntar.

Otra de las decisiones que se tomó fue que el bot no tenga memoria, es decir, que cada mensaje es independiente y no recuerda la conversación que estaba teniendo. Esto se debe a que no encontré evidencia suficiente que demuestre la necesidad de una sucesión de mensajes para poder vulnerar un chat de este estilo, en mis lecturas los mensajes se componen de manera heuristica o automatizada de tal maner que con solo un mensaje pueda vulnerar al modelo, estos mensajes pueden tener multiples partes las cuales cada parte cumple una función diferente como es el caso de [HouYi](https://github.com/LLMSecurity/HouYi) donde el mensaje tiene 3 componentes: el framework, el separador y el mensaje malicioso o como en el el caso de DAN (“Do Anything Now”) un tipo de promt diseñado especificamente para vulnerar ChatGPT el cual contiene multiples lineas de texto para que el modelo se comporte de cierta forma.

# Ejecución Local

Para ejecutar la aplicación localmente, sigue los siguientes pasos:

## Instalar Dependencias:

- Asegúrate de tener Python 3.8 o superior instalado en tu sistema.
- Instala las dependencias necesarias ejecutando el siguiente comando:
  ```pip install -r requirements.txt```

## Configurar las Credenciales:

- Crear una cuenta en https://huggingface.co/.

- Crear un archivo .env en la raíz del proyecto y añade las siguientes variables de entorno: ```HUGGINGFACE_API_KEY=your_huggingface_api_key```

- Esta key se puede generar en https://huggingface.co/settings/tokens

- Además con tu cuenta de HugginFace debés pedir permiso para el acceso al modelo en https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.2.

## Ejecutar la Aplicación:

- Ejecuta el archivo principal de la aplicación usando el siguiente comando: ```streamlit run app.py```

- La aplicación se iniciará y estará lista para recibir solicitudes de chat.

