# Codigo del Curso JavaServer Faces Edit

# Codigo del Curso JavaServer Faces Edit

En este curso estudiaremos 5 lecciones para analizar muchos de los elementos que componen el framework de JSF (JavaServer Faces):

Introducción a JavaServer Faces (JSF)
ManagedBeans y Navegación en JSF
Manejo de Eventos en JSF
Validadores y Convertidores en JSF
Facelets de JSF

JSF es una tecnología que fué creada para simplificar la creación de interfaces web de usuario para aplicaciones web JavaEE.

Mediante ejemplos prácticos analizaremos las características de este framework, utilizando diferentes herramientas de desarrollo para crear nuestras aplicaciones web Java con JSF e integrarlas con otras tecnologías Java como EJB y JPA.

Algunas de las características principales de JSF son:

Es un marco de trabajo (framework) para crear aplicaciones JavaEE basadas en el patrón de diseño MVC (Modelo-Vista-Controlador) y utilizando la API de Servlets.
Utiliza páginas JSP para generar las vistas, añadiendo una biblioteca de etiquetas propia para crear componentes reutilizables: JavaScript, HTML, CSS, … que podrán ser desplegados en cualquier tipo de cliente (navegadores, móviles, …), ahorrando mucho tiempo en el desarrollo de aplicaciones web. Este concepto se conoce como Render Kits.
JSF resuelve validaciones, conversiones, mensajes de error e internacionalización (i18n).
Es extensible, pudiendo crearse nuevos elementos de la interfaz o modificar los ya existentes. JSF dispone de varias implementaciones diferentes, incluyendo un conjunto de etiquetas y APIs estándar que forman el núcleo del framework. Algunas de las implementaciones son: PrimeFaces, RichFaces, IceFaces, cada una de las cuales contiene un número diferente de componentes.
Soporte nativo para AJAX, por tanto, facilita el tratamiento de peticiones asíncronas.
Soporte por defecto para el uso de la tecnología de Facelets.
Y lo que es más importante: forma parte del estándar J2EE.
Arquitectura general:
En el caso de JSF 2 la definición de la interfaz se realiza en forma de páginas XHTML con distintos tipos de etiquetas que veremos más adelante. Estas páginas se denominan páginas JSF. La siguiente figura muestra el funcionamiento de JSF para generar una página por primera vez.
https://dc722jrlp2zu8.cloudfront.net/media/django-summernote/2017-07-24/ef3183c9-d483-47ef-85c1-879410aca35f.png

El navegador realiza una petición a una determinada URL en la que reside la página JSF que se quiere mostrar. En el servidor un servlet que llamamos motor de JSF recibe la petición y construye un árbol de componentes a partir de la página JSF que se solicita. Este árbol de componentes replica en forma de objetos Java la estructura de la página JSF original y representa la estructura de la página que se va a devolver al navegador. Una vez construido el árbol de componentes, se ejecuta código Java en el servidor para rellenar los elementos del árbol con los datos de la aplicación. Por último, a partir del árbol de componentes se genera la página HTML que se envía al navegador.
https://dc722jrlp2zu8.cloudfront.net/media/django-summernote/2017-07-24/08cae69e-fc22-4de0-8059-84f1f0f6199f.png

Lado del cliente:
Vistas web: HTML, CSS y JavaScript.
Lado del servidor:
Capa de Presentación: JSP, JSF, Ajax, …
Capa de Negocio (objetos de negocio): EJB
Capa de Datos (objetos entidad): JPA, JDBC
MVC con JSF:
https://dc722jrlp2zu8.cloudfront.net/media/django-summernote/2017-07-25/28b851fa-73d1-491b-bd8a-ae46e93b666e.png
Modelo: es el encargado de almacenar los datos de la aplicación web. Se puede implementar con clases java (POJO: Plain Old Java Object) o con Managed Bean de modelo.
Vista: define la interfaz de usuario utilizando JSF (Facelets o JSPs), JSTL, EL, ... para desplegar la información del modelo.
Controlador: define el flujo de la aplicación y las interacciones del usuario. Se puede implementar con Managed Bean de control.
El flujo de una aplicación web dinámica que utiliza JSF es análogo al flujo que utilizábamos en el desarrollo web utilizando JSP.

Para utilizar JSF en nuestros proyectos tenemos que añadir a nuestro entorno configurado en el curso de Desarrollo Web JavaEE, los siguientes ficheros JAR:

myfaces-api.jar (jsf-api.jar), JavaServer Faces API para representar los componentes.
myfaces-impl.jar (jsf-impl.jar), Tag libraries para añadir los componentes a la páginas web.
Que podremos descargar desde nuestro IDE Eclipse desde varias fuentes como:

Oracle Mojarra
Apache MyFaces
Una vez añadidos los ficheros JAR tendremos que configurar nuestro fichero descriptor web.xml y añadir el fichero faces-config.xml (opcional a partir de la versión JSF 2.0).

Añadir JSF a nuestro Dynamic Web Project en Eclipse IDE
Para añadir la compatibilidad con JSF a nuestro proyecto de JavaEE tendremos que editar las propiedades de nuestro proyecto: Project properties → Project Facets y añadir la opción: “JavaServer Faces 2.1”:


La primera vez que activamos esta opción nos aparece una nueva ventana, que nos permitirá descargar las librerías necesarias, tras pulsar en el mensaje de error “Further configuration required...”


a continuación pulsamos en el icono para descargar las librerías y descargamos JSF 2.1 de Apache, seleccionándola y pulsando en “Next”:


y aceptamos la licencia para que se inicie la descarga tras pulsar el botón “Finish”:


Una vez descargado ya lo tenemos disponible en la ventana de selección de “JSF Capabilities”. El resto de parámetros los dejamos por defecto.


Entre los parámetros aparece:

JSF Configuration File, fichero de configuración faces-config.xml
JSF Servlet Name y JSF Servlet Class Name, nombre del Servlet especificado en nuestro archivo web.xml
URL Mapping Patterns: mapeo del Servlet que atenderá las peticiones de JSF. Cualquier petición dirigida a una ruta que contenga /faces/* será atendida por el framework de JSF.
Tras aceptar las modificaciones, la estructura de nuestro proyecto nos muestras los siguientes cambios:


Si editamos el fichero web.xml podremos comprobar la integración de JSF mediante la incorporación de un Servlet:

Integración de JSF, cualquier petición cuya URL contenga la palabra “faces” será atendida por el framework de JSF.

.
.
.
<session-config>
    <session-timeout>30</session-timeout>
  </session-config>
  <servlet>
    <servlet-name>Faces Servlet</servlet-name>
    <servlet-class>javax.faces.webapp.FacesServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>Faces Servlet</servlet-name>
    <url-pattern>/faces/*</url-pattern>
  </servlet-mapping>
  <context-param>
    <param-name>javax.servlet.jsp.jstl.fmt.localizationContext</param-name>
    <param-value>resources.application</param-value>
  </context-param>
.
.
.
Si abrimos el fichero faces-config.xml podemos ver que se carga un editor gráfico y que existen otras muchas formas de visualizarlo (Introduction, Overview, Navigation Rule, ManagedBean, Component, Others, Source):


y si nos vamos a la vista de “Source” tenemos:

Etiqueta faces-config
Versión 2.1 JSF
<?xml version="1.0" encoding="UTF-8"?>

<faces-config
    xmlns="http://java.sun.com/xml/ns/javaee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-facesconfig_2_1.xsd"
    version="2.1">

</faces-config>


Para crear nuestro primer JSF, crearemos un nuevo fichero .xhtml dentro del directorio de publicación de nuestra aplicación.


Seleccionamos New → HTML File y pulsamos “Next”. El nombre del fichero será index.xhtml:


y seleccionaremos el tipo “New Facelet Composition Page”:


Contenido base, añade los namespaces para poder importar y utilizar las etiquetas de JSF.

Borramos el resto de código para esta prueba.

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml"
	xmlns:ui="http://xmlns.jcp.org/jsf/facelets"
	xmlns:h="http://xmlns.jcp.org/jsf/html"
	xmlns:f="http://xmlns.jcp.org/jsf/core">

<ui:composition template="">
	<ui:define name="header">
	    Add your header here or delete to use the default
	</ui:define>
	<ui:define name="content">
	    Add your content here or delete to use the default
	</ui:define>
	<ui:define name="footer">
	    Add your footer here or delete to use the default
	</ui:define>
</ui:composition>
</html>

Abrimos el index.xhtml con el editor web de Eclipse (Botón derecho → Open With → Web Page Editor).


Desde la paleta de componentes añadimos:

JSF HTML
Body
Output Label, escribimos el contenido “HolaMundo JSF”
Ejecutamos el fichero index.xhtml en Tomcat y comprobamos que se ejecuta correctamente reconociendo que se trata de un JSF (ruta /faces/*). En caso contrario se imprimiría la etiqueta outputLabel y no el contenido de dicha etiqueta (“HolaMundo JSF”).

Managed Beans

Los Managed Beans son clases java que siguen la nomenclatura y convenciones de los JavaBeans (atributos de clase privados, getters y setters, …). Al igual que estos, los Managed Beans se ejecutan en el lado del servidor y pueden clasificarse en los siguientes tipos:

Model Managed-Bean: Normally session scope. This type of managed-bean participates in the "Model" concern of the MVC design pattern.
Backing Managed-Bean: Normally request scope. This type of managed-bean participates in the "View" concern of the MVC design pattern.
Controller Managed-Bean: Normally request scope. This type of managed-bean participates in the "Controller" concern of the MVC design pattern.
Support Managed-Bean: Normally session or application scope. This type of bean "supports" one or more views in the "View" concern of the MVC design pattern.
Utility Managed-Bean
JSF se encarga de administrar el ciclo de vida de los Managed Beans realizando todas las tareas de control, tales como:

Crear las instancias (las clases deben definirse con un constructor vacío)
Determinar el ámbito de alcance (request, response, application, session)
Llamar a los métodos getters y setters. Ejemplo: “#{servidor.estado}” llamará al método getEstado().
Podemos declarar los Managed Beans utilizando:

Anotaciones en las propias clases (@ManagedBean), antes del nombre de la clase, y a continuación el alcance del contexto (@RequestScoped, @ViewScoped, @SessionScoped, @ApplicationScoped). Es la forma más extendida de declarar Managed Beans.
Utilizando CDI (Contexts and Dependency Inyection) asociados a un contexto (request, session, application, …), mediante la incorporación de:
Anotación @Named, antes del nombre de la clase, y a continuación el alcance del contexto (@RequestScoped, @ConversationScoped, @SessionScoped, @ApplicationScoped).
Fichero beans.xml en nuestro directorio WEB-INF.
En el archivo faces-config.xml (opcional a partir de JSF 2.0) utilizando los tags “<managed-bean> … </managed-bean>”
A parte de los alcances ya conocidos, JSF (a partir de 2.0) añade alguno más. Si listados de forma ordenada, según el tiempo de vida que ofrece cada alcance tenemos:

request (@RequestScoped), permanece en la petición del usuario.
view (@ViewScoped), este scope dura desde que se muestra una página JSF al usuario hasta que el usuario navega hacia otra página. Es muy útil para páginas que usan AJAX, ya que ejecutan actualizaciones sobre la misma página.
session (@SessionScoped), permanece en la sesión del usuario.
application (@ApplicationScoped), permanece para toda la aplicación.
La definición de los alcances para JSF se encuentra en el package: javax.faces.bean

Los alcances utilizando CDI tienen algunas diferencias con respecto a los existentes en JSF. En este caso el alcance “view” es sustituido por el alcance “Conversation”:

request (@RequestScoped)
conversation (@ConversationScoped), este alcance permite guardar elementos entre páginas relacionadas hasta finalizar la tarea.
session (@SessionScoped)
application (@ApplicationScoped)
La definición de los alcances para CDI se encuentra en el package: javax.enterprise.context


er código fuente del proyecto: “capitulo2_examples”.

Creamos un nuevo proyecto “Dynamic Web Project”
Target runtime: Tomcat v8.0
Dynamic web module version: 3.1
Añadimos JSF 2.1 desde Project properties → Project Facets.
Creamos un nuevo Managed Bean mediante el asistente gráfico de ManagedBean disponible mediante faces-config.xml. Al abrirse el asistente pulsamos en “Add” → “Create a New Java Class” → “Next”:


Package: model

Nombre de la clase: ManagedBean1
Nombre Managed Bean: managedBean1 (lo convierte a minúsculas ya será el nombre del objeto cuando se cree una instancia de la clase)
Alcance: request (se creará y se destruirá con cada nueva petición web)
y finalizamos guardando los cambios en el fichero faces-config.xml:


Comprobamos que se crea la siguiente clase:

package model;

public class ManagedBean1 {

	public ManagedBean1() {
		// TODO Auto-generated constructor stub
	}

}
Creamos el mismo Managed Bean mediante anotaciones:

Añadimos los packages para JSF.
Añadimos las anotaciones.
Importamos los packages para las anotaciones de JSF.

Anotaciones que indican que será una clase Managed Bean administrada por el framework de JSF y que tendrá un alcance de Request.

Constructor vacío, es necesario que cada atributo sea privado y tenga sus métodos get y set públicos.


package model;

import javax.faces.bean.*;

@ManagedBean
@RequestScoped
public class ManagedBean1 {

	public ManagedBean1() {
		// TODO Auto-generated constructor stub
	}

}
Acceso a los atributos del Model Managed-Bean
Creamos 2 clases con el siguiente contenido:

src / model.ManagedBean1.java
WebContent / index.xhtml
Atributo privado y métodos get y set.

package model;

import javax.faces.bean.*;

@ManagedBean
@RequestScoped
public class ManagedBean1 {

	private String name = "Prueba";
	
	public ManagedBean1() {
		// TODO Auto-generated constructor stub
	}
	
	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}
Contenido base, añade los namespaces para poder importar y utilizar las etiquetas de JSF.

Formulario, que usa las etiquetas de JSF, para mostrar el valor del atributo name del ManagedBean1. Podemos utilizar el editor web o escribir las etiquetas en la vista de código.


	<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml"
	xmlns:ui="http://xmlns.jcp.org/jsf/facelets"
	xmlns:f="http://xmlns.jcp.org/jsf/core"
	xmlns:h="http://java.sun.com/jsf/html">


<h:body>HolaMundo JSF<br /><br />
	<h:form>
		<h:outputLabel for="name" value="Nombre:"></h:outputLabel>
		<h:inputText id="name" value="#{managedBean1.name}"></h:inputText>
		<h:message for="name"></h:message>
		<h:commandButton value="Enviar"></h:commandButton>
	</h:form>
</h:body>
</html>
Si ejecutamos el index.xhtml podemos comprobar que la página JSF accede correctamente al atributo del Managed Bean utilizando la sintaxis de EL (Expression Language):

Creamos las clases con el siguiente contenido:

src / model.Usuario.java
src / backing.Solicitud.java
WebContent / index2.xhtml
WebContent / correcto.xhtml (tipo “New Facelet Composition Page”)
WebContent / incorrecto.xhtml (tipo “New Facelet Composition Page”)
Importamos los packages para las anotaciones de JSF.

Anotaciones que indican que será una clase Managed Bean administrada por el framework de JSF y que tendrá un alcance de Request.

Atributos con valores por defecto para la prueba.

Constructor vacío, es necesario que cada atributo sea privado y tenga sus métodos get y set públicos.

package model;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.RequestScoped;

@ManagedBean
@RequestScoped
public class Usuario {

    private String nombre = "NombrePorDefecto";
    private String email = "emailPorDefecto";
   
    public Usuario() {
		// TODO Auto-generated constructor stub
    }

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
Dentro de JSF, la anotación @ManagedProperty se utiliza para la inyección de dependencias (DI) de un managed bean dentro de un atributo de otro managed bean.

Para inyectar el valor de nuestro Managed Bean usuario utilizamos EL y la anotación @ManagedProperty seguido del atributo de la clase de objeto a inyectar.

A continuación se definen los métodos get y set para dicho atributo.

Método público para controlar el flujo de nuestra aplicación, dependiendo del nombre de usuario introducido, se devuelve la siguiente página JSF a mostrar:

correcto.xhtml
incorrecto.xhtml
En versiones anteriores el flujo de la aplicación debía ser definido en el fichero faces-config.xml

package backing;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.ManagedProperty;
import javax.faces.bean.RequestScoped;

import model.Usuario;

@ManagedBean
@RequestScoped
public class Solicitud {
	
	@ManagedProperty(value="#{usuario}")
	private Usuario usuario;

	public Solicitud() {
		// TODO Auto-generated constructor stub
	}
	
	public Usuario getUsuario() {
		return usuario;
	}

	public void setUsuario(Usuario usuario) {
		this.usuario = usuario;
	}
	
	public String solicitar(){
		if(this.usuario.getNombre().equals("Marina")){
			//Redirigimos el flujo directamente especificando el nombre de la nueva página en la devolución del método
			System.out.println("Correcto para usuario: " + this.usuario.getNombre());
			return "correcto";//=correcto.xhtml
		}else{
			//Redirigimos el flujo directamente especificando el nombre de la nueva página en la devolución del método
			System.out.println("Incorrecto para usuario: " + this.usuario.getNombre());
			return "incorrecto";//=incorrecto.xhtml
		}
	}

}
Añadimos al formulario un botón (commandButton) para:

Llamar al método “solicitar()” de nuestro backing bean “solicitud”.
Imprimir la cadena que devuelve la llamada a dicho método utilizando EL.
Redirigir a la página cuyo nombre coincida con la cadena devuelta por le método (correcto o incorrecto).
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml"
	xmlns:ui="http://xmlns.jcp.org/jsf/facelets"
	xmlns:f="http://xmlns.jcp.org/jsf/core"
	xmlns:h="http://java.sun.com/jsf/html">


<h:body>
	<h:form>
		<h:outputLabel for="nombre" value="Nombre:"></h:outputLabel>
		<h:inputText id="nombre" value="#{usuario.nombre}"></h:inputText>
		<h:message for="nombre"></h:message>
		<h:commandButton action="#{solicitud.solicitar}" value="Enviar"></h:commandButton>
	</h:form>
</h:body>
</html>
Tipo “New Facelet Composition Page”

Formulario JSF para imprimir un mensaje. El mensaje se mete dentro de un formulario para poder incluir un botón activo (CommandLink). Para usar EL con nuestro Bean, utilizamos el nombre del Bean (clase con minúscula inicial). El framework de JSF crea las instancias automáticamente con esta nomenclatura.

CommandLink JSF para regresar a nuestro index2.

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml"
	xmlns:ui="http://xmlns.jcp.org/jsf/facelets"
	xmlns:h="http://xmlns.jcp.org/jsf/html"
	xmlns:f="http://xmlns.jcp.org/jsf/core">

<h:head>
    <title>Correcto</title>
    </h:head>
    <h:body>
        <h:form>
            El usuario #{usuario.nombre} con email #{usuario.email} ha cursado la solicitud correctamente.
            <br/>
        <h:commandLink action="index2">
			<h:outputText value="Volver"></h:outputText>
		</h:commandLink>            
        </h:form>
    </h:body>
</html>

li>Tipo “New Facelet Composition Page”
Idem “correcto.xhtml”
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml"
	xmlns:ui="http://xmlns.jcp.org/jsf/facelets"
	xmlns:f="http://xmlns.jcp.org/jsf/core"
	xmlns:h="http://java.sun.com/jsf/html">

<h:head>
	<title>Incorrecto</title>
</h:head>
<h:body>
	<h:form>
         No se puede cursar la solicitud para el usuario especificado: #{usuario.nombre}.
         <br />
    <h:commandLink action="index2">
		<h:outputText value="Volver"></h:outputText>
	</h:commandLink>
	</h:form>
</h:body>
</html>
Ejemplo de uso:

Alcance de request, por tanto, el valor “nombre” se recupera desde el objeto “usuario” de la petición en curso.
Si pulsamos en el link “Volver”, recuperará el valor del nombre por defecto ya que se ejecuta una nueva llamada al “index2”.
Creación de un Bean
Notación JSF

Seleccionar alcance.
Nombre de la clase con Mayúsculas.
@ManagedBean
[@RequestScoped | @ViewScoped | @SessionScoped | @ApplicationScoped]
public class NombreClase{}
Notación CDI

Permite personalizar el nombre de los objetos de esta clase.
Esta notación facilita la integración con otras tecnologías como EJB y JPA.
@Named(“nombreClasePersonalizado”)
public class NombreClase{}
Uso de un Bean
Nombre del objeto con minúsculas.
Uso de EL normalmente.
#{nombreClase.atributo}
Notación CDI

	#{nombreClasePersonalizado.atributo}
Inyección de Beans
Con anotaciones dentro del Managed Bean

@ManagedProperty(value="#{usuario}")
O en el archivo opcional “WEB-INF/faces-config.xml” (en desuso).

Alcance por defecto ”request” a no ser que se especifique lo contrario, por ejemplo, “session”.
Para agregar una dependencia, inyectamos la dependencia, mediante el tag <managed-property>, dentro de la definición de la clase. Internamente se utilizará el método “setUsuario()” declarado en la clase “Solicitud”.
<?xml version="1.0" encoding="UTF-8"?>

<faces-config
    xmlns="http://java.sun.com/xml/ns/javaee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-facesconfig_2_1.xsd"
    version="2.1">

	<managed-bean>
		<managed-bean-name>usuario</managed-bean-name>
		<managed-bean-class>model.Usuario</managed-bean-class>
		<managed-bean-scope>session</managed-bean-scope>
        </managed-bean>
        <managed-bean>
		<managed-bean-name>solicitud</managed-bean-name>
		<managed-bean-class>backing.Solicitud</managed-bean-class>

               <managed-property>"#{usuario}"</managed-property>
      	</managed-bean>
</faces-config>


Para utilizar EL y sus operadores aritméticos o lógicos, se actuará igual que utilizando páginas JSPs (ver curso completo de Desarrollo web JavaEE).

Para resolver el alcance (Request, View, Session, Application) y el acceso a los objetos implícitos, se actuará igual que utilizando páginas JSPs (ver curso completo de Desarrollo web JavaEE).


Para determinar el flujo de navegación entre las distintas páginas, que utilicen la misma tecnología, podemos diferenciar varios aspectos:

Tipos de navegación
Tenemos diferentes tipos de navegación:

Navegación estática, no se implementa ninguna lógica para determinar la página siguiente: <h:commandButton action="solicitar" value="Enviar"></h:commandButton>
La página siguiente se puede determinar de 2 formas:
Añadiendo la misma extensión de la página solicitante (xhtml, jspx) al outcome (p.e. solicitar.xhtml) y buscando dicha página en el mismo directorio que ejecuto la petición. Esta alternativa es prioritaria sobre el fichero faces-config.xml.
Buscando en el fichero faces-config.xml el caso de navegación que determina la siguiente página.
Navegación dinámica, se implementa una lógica para determinar la página siguiente: <h:commandButton action="#{solicitud.solicitar}" value="Enviar"></h:commandButton>
Revisar lógica en “capitulo2_examples”: index2.xhtml → Solicitud.solicitar() → correcto.xhtml | incorrecto.xhtml
Configurar el flujo de navegación
Tenemos varias formas de configurar la navegación:

Navegación implícita (JSF), busca una página en el directorio actual cuyo nombre coincida con el especificado por la cadena de salida de la página actual y con la misma extensión que dicha página (.xhtml, jspx). Esta alternativa es prioritaria sobre el fichero faces-config.xml.
Navegación explícita, se define la navegación en el fichero faces-config.xml (Ver proyecto javaquiz).
Caso de navegación genérico para redireccionar cualquier página que no coincida con ninguna cadena de salida a la página de inicio (index.xhtml).

Para añadir las diferentes reglas de navegación se utiliza el tag “<navigation-rule>”.

Tag para especificar la página desde la que proviene la petición.

Tag para especificar los casos de navegación para la página desde la que proviene la petición (<from-view-id>). Podrán existir múltiples opciones con una misma página origen. Por cada caso de navegación para una misma página “origen” debemos añadir un nuevo bloque <navigation-rule>.

Define la cadena de salida (outcome) de la página anterior. En este caso “failure”.

Define la siguiente vista a mostrar (“again.xhtml”) en el caso que la salida de la página actual coincida con el outcome “failure”.

<?xml version="1.0"?>
<faces-config xmlns="http://java.sun.com/xml/ns/javaee" 
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
      http://java.sun.com/xml/ns/javaee/web-facesconfig_2_0.xsd"
   version="2.0">
   <navigation-rule>
      <navigation-case>
         <from-outcome>startOver</from-outcome>
         <to-view-id>/index.xhtml</to-view-id>
      </navigation-case>
   </navigation-rule>
   <navigation-rule>
      <from-view-id>/again.xhtml</from-view-id>
      <navigation-case>
         <from-outcome>failure</from-outcome>
         <to-view-id>/failure.xhtml</to-view-id>
      </navigation-case>
   </navigation-rule>
   <navigation-rule>
      <navigation-case>
         <from-outcome>failure</from-outcome>
         <to-view-id>/again.xhtml</to-view-id>
      </navigation-case>
   </navigation-rule>

   <application>
      <resource-bundle>
         <base-name>com.corejsf.messages</base-name>
         <var>msgs</var>
      </resource-bundle>
   </application>
</faces-config>




as validaciones en JSF son necesarios para verificar que los datos introducidos por los usuarios son correctos (rango de números, longitud de cadenas de texto, formato de fechas, ...).

Tenemos varios mecanismos para realizar la validación de los datos:

Añadir al atributo el tag required=”true”.
Utilizar un validador estándar (validateLongRange, validateDoubleRange, validateLength, …).
Utilizar un validador personalizado, mediante el tag validator.
Visualizar los errores sobre los componentes JSF con el tag “h:message”.
Visualizar todos los errores de la página completa con el tag “h:messages”.
faces-config.xml (Definir los mensajes propios o sobrescribir los internos de JSF)

Mediante la definición, en un fichero global de idioma (messages.properties, messages.properties_en, ...), de todos los mensajes a utilizar en nuestra la aplicación podemos implementar fácilmente la compatibilidad multidioma (Internacionalización I18n). Se especifica la localización del fichero messages pero omitiendo la extensión .properties.

El tag <message-bundle> se utilizará siempre que deseemos reemplazar los mensajes de advertencia/error predeterminados de JSF para los mecanismos de validación/conversión.

Siguiendo la estrategia de internacionalización, definimos el fichero “com.corejsf.messages.properties” para definir los mensajes.

El tag <resource-bundle> se utiliza para registrar un recurso localizado (msgs → “com.corejsf.messages.properties”) y que esté disponible en toda la aplicación JSF sin necesidad de especificar <f: loadBundle> en cada vista.


	<?xml version="1.0"?>
<faces-config xmlns="http://java.sun.com/xml/ns/javaee" 
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
      http://java.sun.com/xml/ns/javaee/web-facesconfig_2_0.xsd"
   version="2.0">
   <application>
      <message-bundle>com.corejsf.messages</message-bundle>
      <resource-bundle>
         <base-name>com.corejsf.messages</base-name>
         <var>msgs</var>
      </resource-bundle>
   </application>
</faces-config>

Definir los mensajes propios a utilizar en la aplicación, en el idioma deseado.

Sobrescribir los mensajes internos de JSF.

title=An Application to Test Validation
enterPayment=Please enter the payment information 
amount=Amount
creditCard=Credit Card
expirationDate=Expiration date (Month/Year)
paymentInformation=Payment information
canceled=The transaction has been canceled.
process=Process
cancel=Cancel
back=Back
cardRequired=A credit card number is required.

javax.faces.component.UIInput.REQUIRED=Valor obligatorio (mensaje desde messages.properties)
Proyecto “validator” → index.xhtml

Recurso (msgs) disponible mediante su definición previa en “faces-config.xml”.

Atributo “required”, así validaremos que el campo “amount” contenga datos antes de poder enviar el formulario (Info: mensaje personalizado en messages.properties).

Convertidores (ver punto “Convertidores”)

Validadores estándar: validación de rango.

Mensajes de error asociados a los errores sobre los componentes JSF, especificados por el campo “for”, utilizando el tag “h:message”.

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"  
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:f="http://java.sun.com/jsf/core" 
      xmlns:h="http://java.sun.com/jsf/html">
   <h:head>
      <h:outputStylesheet library="css" name="styles.css"/>
      <title>#{msgs.title}</title>
   </h:head>
   <h:body>
      <h:form>
         <h1>#{msgs.enterPayment}</h1>
         <h:panelGrid columns="3">
            #{msgs.amount}
            <h:inputText id="amount" label="#{msgs.amount}"
                         value="#{payment.amount}" required="true">
               <f:convertNumber minFractionDigits="2"/>
               <f:validateDoubleRange minimum="10" maximum="10000"/>
            </h:inputText>
            <h:message for="amount" styleClass="errorMessage"/>

            #{msgs.creditCard}
            <h:inputText id="card" label="#{msgs.creditCard}"
                         value="#{payment.card}" required="true"
                         requiredMessage="#{msgs.cardRequired}">
               <f:validateLength minimum="13"/>
            </h:inputText>
            <h:message for="card" styleClass="errorMessage"/>

            #{msgs.expirationDate}
            <h:inputText id="date" label="#{msgs.expirationDate}"
                         value="#{payment.date}" required="true">
               <f:convertDateTime pattern="MM/yyyy"/>
            </h:inputText>
            <h:message for="date" styleClass="errorMessage"/>
         </h:panelGrid>
         <h:commandButton value="#{msgs.process}" action="result"/>
         <h:commandButton value="#{msgs.cancel}" action="canceled" 
                          immediate="true"/>
      </h:form>
   </h:body>
</html>
Proyecto “validator2” → index.xhtml

Recurso (msgs) disponible mediante su definición previa en “faces-config.xml”.

Convertidores (ver punto “Convertidores”)

Validador personalizado: utilizando el tag ”…/> (ver “CreditCardValidator.java”). El nombre del validador debe estar definido en faces-config.xml o en el propio Bean utilizando: @FacesValidator("com.corejsf.Card") e implementar el método validate dentro de este Bean.

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"  
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:f="http://java.sun.com/jsf/core" 
      xmlns:h="http://java.sun.com/jsf/html">
   <h:head>
      <h:outputStylesheet library="css" name="styles.css"/>
      <title>#{msgs.title}</title>
   </h:head>
   <h:body>
      <h:form>
         <h1>#{msgs.enterPayment}</h1>
         <h:panelGrid columns="3">
            #{msgs.amount}
            <h:inputText id="amount" label="#{msgs.amount}"
                         value="#{payment.amount}">
               <f:convertNumber minFractionDigits="2"/>
            </h:inputText>
            <h:message for="amount" styleClass="errorMessage"/>

            #{msgs.creditCard}
            <h:inputText id="card" label="#{msgs.creditCard}"
                         value="#{payment.card}" required="true">
               <f:converter converterId="com.corejsf.Card"/>
               <f:validator validatorId="com.corejsf.Card"/>
            </h:inputText>
            <h:message for="card" styleClass="errorMessage"/>

            #{msgs.expirationDate}
            <h:inputText id="date" label="#{msgs.expirationDate}"
                         value="#{payment.date}">
               <f:convertDateTime pattern="MM/yyyy"/>
            </h:inputText>
            <h:message for="date" styleClass="errorMessage"/>
         </h:panelGrid>
         <h:commandButton value="#{msgs.process}" action="result"/>
      </h:form>
   </h:body>
</html>

Proyecto “validator2” → CreditCardValidator.java

Requisitos para utilizar un validador personalizado:

Tag @FacesValidator()
Método validate
package com.corejsf;

import javax.faces.application.FacesMessage;
import javax.faces.component.UIComponent;
import javax.faces.context.FacesContext;
import javax.faces.validator.FacesValidator;
import javax.faces.validator.Validator;
import javax.faces.validator.ValidatorException;

@FacesValidator("com.corejsf.Card")
public class CreditCardValidator implements Validator {
   public void validate(FacesContext context, UIComponent component, Object value) {
      if(value == null) return;
      String cardNumber;
      if (value instanceof CreditCard)
         cardNumber = value.toString();
      else 
         cardNumber = value.toString().replaceAll("\\D", ""); // remove non-digits
      if(!luhnCheck(cardNumber)) {
         FacesMessage message 
            = com.corejsf.util.Messages.getMessage(
               "com.corejsf.messages", "badLuhnCheck", null);
         message.setSeverity(FacesMessage.SEVERITY_ERROR);
         throw new ValidatorException(message);
      }
   }

   private static boolean luhnCheck(String cardNumber) {
      int sum = 0;

      for(int i = cardNumber.length() - 1; i >= 0; i -= 2) {
         sum += Integer.parseInt(cardNumber.substring(i, i + 1));
         if(i > 0) {
            int d = 2 * Integer.parseInt(cardNumber.substring(i - 1, i));
            if(d > 9) d -= 9;
            sum += d;
         }
      }
      
      return sum % 10 == 0;
   }
}
Proyecto “messages” → index.xhtml

Mensajes de error asociados todos los errores de la página completa con el tag “h:messages”.

Mensajes de error asociados a los errores sobre los componentes JSF, especificados por el campo “for”, utilizando el tag “h:message”.


	<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"  
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:f="http://java.sun.com/jsf/core" xmlns:h="http://java.sun.com/jsf/html">
   <h:head>
      <title>#{msgs.windowTitle}</title>
      <h:outputStylesheet library="css" name="styles.css"/>
   </h:head>
   <h:body>
      <h:form>
         <h:outputText value="#{msgs.greeting}" styleClass="emphasis"/>
         <br/>
         <h:messages errorClass="errors" layout="table"/>
         <h:panelGrid columns="3">
            #{msgs.namePrompt}:
            <h:inputText id="name" value="#{user.name}" required="true"
                         label="#{msgs.namePrompt}"/>
            <h:message for="name" errorClass="errors"/>
            #{msgs.agePrompt}:
            <h:inputText id="age" value="#{user.age}" required="true"
                         size="3" label="#{msgs.agePrompt}"/>
            <h:message for="age" errorClass="errors"/>
         </h:panelGrid>
         <h:commandButton value="#{msgs.submitPrompt}"/>
      </h:form>
   </h:body>
</html>


Los convertidores en JSF son necesarios para realizar funciones de conversión ente la Vista y el Modelo y viceversa.

Conversión entre Vista y Modelo, p.e. convertir un cadena (String) en una fecha (Date) y asignarla a un atributo de tipo Date de las clases del modelo.

Conversión entre Modelo y Vista, p.e. mostrar solo 1 decimal de un atributo tipo double del objeto del modelo que estamos representando.

Podemos utilizar 3 tipos de convertidores:

Convertidores implícitos (tipos primitivos Integer o String, …), son conversiones que se realizan de forma automática al pasar datos entre la vista y el modelo.
Convertidores explícitos, mediante el tag de componente JSF converter llamando a una clase de un tipo primitivo (<f:converter converterId=”javax.faces.Integer” />), o bien, utilizando tags predefinidos (convertNumber, convertDateTime, ...)
Convertidores personalizados, utilizando una clase personalizada de tipo Converter mediante el uso de:
Atributo convert especificando la clase que va a realizar la conversión (<h:inputText id=”fecha” value=”#{usuario.nacimiento}” convert=”com.corefs.FechaConverter” />).
Clase de tipo Converter (implementa la interfaz javax.faces.convert.Converter) que contenga el código de la conversión. Esta clase se debe registrar en faces-config.xml o agregar la anotación @FacesConverter en dicha clase. Esta clase Converter deberá sobrescribir estos 2 métodos dependiendo del tipo de conversión requerida:
getAsObject (), convierte el String que se pasa como parámetro en un Objeto. Durante la fase “Apply Request Values”, cuando se procesan los métodos de decodificación de los componentes, la implementación de JavaServer Faces busca el valor local del componente en la solicitud y llama al método getAsObject.
getAsString (), convierte el Objeto que se pasa como parámetro en un String. Durante la fase “Render Response”, en la que se llaman los métodos de codificación de los componentes, la implementación de JavaServer Faces llama al método getAsString para generar la salida apropiada.
Proyecto “converter” → index.xhtml

Recurso (msgs) disponible mediante su definición previa en “faces-config.xml”.

Convertidor implícito, el valor introducido (número de la tarjeta) se tratará como un String (ver Bean “PaymentBean.java”).

Convertidores explícitos:

Mínimo 2 decimales
Formato de fecha MM/yyyy
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:f="http://java.sun.com/jsf/core" 
      xmlns:h="http://java.sun.com/jsf/html">
   <h:head>
      <h:outputStylesheet library="css" name="styles.css"/>
      <title>#{msgs.title}</title>
   </h:head>
   <h:body>
      <h:form>
         <h1>#{msgs.enterPayment}</h1>
         <h:panelGrid columns="3">
            #{msgs.amount}
            <h:inputText id="amount" label="#{msgs.amount}"
                         value="#{payment.amount}">
               <f:convertNumber minFractionDigits="2"/>
            </h:inputText>
            <h:message for="amount" styleClass="errorMessage"/>

            #{msgs.creditCard}
            <h:inputText id="card" label="#{msgs.creditCard}"
                         value="#{payment.card}"/>
            <h:message for="card" styleClass="errorMessage" />

            #{msgs.expirationDate}
            <h:inputText id="date" label="#{msgs.expirationDate}"
                         value="#{payment.date}">
               <f:convertDateTime pattern="MM/yyyy"/>
            </h:inputText>
            <h:message for="date" styleClass="errorMessage"/>
         </h:panelGrid>
         <h:commandButton value="#{msgs.process}" action="result"/>
      </h:form>
   </h:body>
</html>
Proyecto “converter” → PaymentBean.java

“card” es un atributo tipo String.

package com.corejsf;

import java.io.Serializable;
import java.util.Date;

import javax.faces.bean.ManagedBean; 
   // or import javax.inject.Named;
import javax.faces.bean.SessionScoped; 
   // or import javax.enterprise.context.SessionScoped;

@ManagedBean(name="payment") // or @Named("payment")
@SessionScoped
public class PaymentBean implements Serializable {
   private double amount;
   private String card = "";
   private Date date = new Date();

   public void setAmount(double newValue) { amount = newValue; }
   public double getAmount() { return amount; }

   public void setCard(String newValue) { card = newValue; }
   public String getCard() { return card; }

   public void setDate(Date newValue) { date = newValue; }
   public Date getDate() { return date; }
}
Proyecto “converter2” → index.xhtml

Recurso (msgs) disponible mediante su definición previa en “faces-config.xml”.

Convertidor personalizado, ya que sobre cualquier instancia del objeto de la clase “CreditCard”, como es el atributo “card” de la clase “PaymentBean” (payment.card), se aplica el convertidor “CreditCardConverter” al especificar en su definición la anotación “@FacesConverter(forClass=CreditCard.class)”

Convertidores explícitos

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"  
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:f="http://java.sun.com/jsf/core" 
      xmlns:h="http://java.sun.com/jsf/html">
   <h:head>
      <h:outputStylesheet library="css" name="styles.css"/>
      <title>#{msgs.title}</title>
   </h:head>
   <h:body>
      <h:form>
         <h1>#{msgs.enterPayment}</h1>
         <h:panelGrid columns="3">
            #{msgs.amount}
            <h:inputText id="amount" label="#{msgs.amount}"
                         value="#{payment.amount}">
               <f:convertNumber minFractionDigits="2"/>
            </h:inputText>
            <h:message for="amount" styleClass="errorMessage"/>
            
            #{msgs.creditCard}
            <h:inputText id="card" label="#{msgs.creditCard}"
                         value="#{payment.card}">
            </h:inputText>
            <h:message for="card" styleClass="errorMessage"/>
            
            #{msgs.expirationDate}
            <h:inputText id="date" label="#{msgs.expirationDate}"
                         value="#{payment.date}">
               <f:convertDateTime pattern="MM/yyyy"/>
            </h:inputText>
            <h:message for="date" styleClass="errorMessage"/>
         </h:panelGrid>
         <h:commandButton value="#{msgs.process}" action="result"/>
      </h:form>
   </h:body>
</html>

Proyecto “converter2” → PaymentBean.java

“card” es un objeto de la clase “CreditCard”.

package com.corejsf;

import java.io.Serializable;
import java.util.Date;

import javax.faces.bean.ManagedBean; 
   // or import javax.inject.Named;
import javax.faces.bean.SessionScoped; 
   // or import javax.enterprise.context.SessionScoped;

@ManagedBean(name="payment") // or @Named("payment")
@SessionScoped
public class PaymentBean implements Serializable {
   private double amount;
   private CreditCard card = new CreditCard("");
   private Date date = new Date();

   public void setAmount(double newValue) { amount = newValue; }
   public double getAmount() { return amount; }

   public void setCard(CreditCard newValue) { card = newValue; }
   public CreditCard getCard() { return card; }

   public void setDate(Date newValue) { date = newValue; }
   public Date getDate() { return date; }
}
Proyecto “converter2” → CreditCardConverter.java

Convertidor personalizado:

@FacesConverter(forClass=CreditCard.class)

implements Converter

Sobrescribe 2 métodos: Object getAsObject, String getAsString

package com.corejsf;

import javax.faces.application.FacesMessage;
import javax.faces.component.UIComponent;
import javax.faces.context.FacesContext;
import javax.faces.convert.Converter;
import javax.faces.convert.ConverterException;
import javax.faces.convert.FacesConverter;

@FacesConverter(forClass=CreditCard.class)
public class CreditCardConverter implements Converter {
   public Object getAsObject(FacesContext context, UIComponent component,
         String newValue) throws ConverterException {
      StringBuilder builder = new StringBuilder(newValue);
      boolean foundInvalidCharacter = false;
      char invalidCharacter = '\0';
      int i = 0;
      while (i < builder.length() && !foundInvalidCharacter) {
         char ch = builder.charAt(i);
         if (Character.isDigit(ch))
            i++;
         else if (Character.isWhitespace(ch))
            builder.deleteCharAt(i);
         else {
            foundInvalidCharacter = true;
            invalidCharacter = ch;
         }
      }

      if (foundInvalidCharacter) {
         FacesMessage message = com.corejsf.util.Messages.getMessage(
               "com.corejsf.messages", "badCreditCardCharacter",
               new Object[]{ new Character(invalidCharacter) });
         message.setSeverity(FacesMessage.SEVERITY_ERROR);
         throw new ConverterException(message);
      }

      return new CreditCard(builder.toString());
   }

   public String getAsString(FacesContext context, UIComponent component,
         Object value) throws ConverterException {
      // length 13: xxxx xxx xxx xxx
      // length 14: xxxxx xxxx xxxxx
      // length 15: xxxx xxxxxx xxxxx
      // length 16: xxxx xxxx xxxx xxxx
      // length 22: xxxxxx xxxxxxxx xxxxxxxx
      String v = value.toString();
      int[] boundaries = null;
      int length = v.length();
      if (length == 13)
         boundaries = new int[]{ 4, 7, 10 };
      else if (length == 14)
         boundaries = new int[]{ 5, 9 };
      else if (length == 15)
         boundaries = new int[]{ 4, 10 };
      else if (length == 16)
         boundaries = new int[]{ 4, 8, 12 };
      else if (length == 22)
         boundaries = new int[]{ 6, 14 };
      else
         return v;
      StringBuilder result = new StringBuilder();
      int start = 0;
      for (int i = 0; i < boundaries.length; i++) {
         int end = boundaries[i];
         result.append(v.substring(start, end));
         result.append(" ");
         start = end;
      }
      result.append(v.substring(start));
      return result.toString();
   }
}
Proyecto “validator2” → index.xhtml

Recurso (msgs) disponible mediante su definición previa en “faces-config.xml”.

Convertidor personalizado, otra forma de hacer lo mismo es mediante la llamada desde el componente JSF a la clase del convertidor.

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"  
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:f="http://java.sun.com/jsf/core" 
      xmlns:h="http://java.sun.com/jsf/html">
   <h:head>
      <h:outputStylesheet library="css" name="styles.css"/>
      <title>#{msgs.title}</title>
   </h:head>
   <h:body>
      <h:form>
         <h1>#{msgs.enterPayment}</h1>
         <h:panelGrid columns="3">
            #{msgs.amount}
            <h:inputText id="amount" label="#{msgs.amount}"
                         value="#{payment.amount}">
               <f:convertNumber minFractionDigits="2"/>
            </h:inputText>
            <h:message for="amount" styleClass="errorMessage"/>

            #{msgs.creditCard}
            <h:inputText id="card" label="#{msgs.creditCard}"
                         value="#{payment.card}" required="true">
               <f:converter converterId="com.corejsf.Card"/>
               <f:validator validatorId="com.corejsf.Card"/>
            </h:inputText>
            <h:message for="card" styleClass="errorMessage"/>

            #{msgs.expirationDate}
            <h:inputText id="date" label="#{msgs.expirationDate}"
                         value="#{payment.date}">
               <f:convertDateTime pattern="MM/yyyy"/>
            </h:inputText>
            <h:message for="date" styleClass="errorMessage"/>
         </h:panelGrid>
         <h:commandButton value="#{msgs.process}" action="result"/>
      </h:form>
   </h:body>
</html>
Proyecto “validator2” → CreditCardConverter.java

Convertidor personalizado:

@FacesConverter(“com.corejsf.Card”)
implements Converter
Sobrescribe 2 métodos: Object getAsObject, String getAsString
package com.corejsf;

import javax.faces.application.FacesMessage;
import javax.faces.component.UIComponent;
import javax.faces.context.FacesContext;
import javax.faces.convert.Converter;
import javax.faces.convert.ConverterException;
import javax.faces.convert.FacesConverter;

@FacesConverter("com.corejsf.Card")
public class CreditCardConverter implements Converter {
   public Object getAsObject(
      FacesContext context,
      UIComponent component,
      String newValue)
      throws ConverterException {
      StringBuilder builder = new StringBuilder(newValue);
      boolean foundInvalidCharacter = false;
      char invalidCharacter = '\0';
      int i = 0;
      while (i < builder.length() && !foundInvalidCharacter) {
         char ch = builder.charAt(i);
         if (Character.isDigit(ch))
            i++;
         else if (Character.isWhitespace(ch))
            builder.deleteCharAt(i);
         else {
            foundInvalidCharacter = true;
            invalidCharacter = ch;
         }
      }

      if (foundInvalidCharacter) {
         FacesMessage message 
            = com.corejsf.util.Messages.getMessage(
               "com.corejsf.messages", "badCreditCardCharacter", 
               new Object[] { new Character(invalidCharacter) } );
         message.setSeverity(FacesMessage.SEVERITY_ERROR);
         throw new ConverterException(message);
      }

      return new CreditCard(builder.toString());
   }

   public String getAsString(
      FacesContext context,
      UIComponent component,
      Object value)
      throws ConverterException {
      // length 13: xxxx xxx xxx xxx
      // length 14: xxxxx xxxx xxxxx
      // length 15: xxxx xxxxxx xxxxx
      // length 16: xxxx xxxx xxxx xxxx
      // length 22: xxxxxx xxxxxxxx xxxxxxxx
      String v = value.toString();
      int[] boundaries = null;
      int length = v.length();
      if (length == 13)
         boundaries = new int[] { 4, 7, 10 };
      else if (length == 14)
         boundaries = new int[] { 5, 9 };
      else if (length == 15)
         boundaries = new int[] { 4, 10 };
      else if (length == 16)
         boundaries = new int[] { 4, 8, 12 };
      else if (length == 22)
         boundaries = new int[] { 6, 14 };
      else
         return v;
      StringBuilder result = new StringBuilder();
      int start = 0;
      for (int i = 0; i < boundaries.length; i++) {
         int end = boundaries[i];
         result.append(v.substring(start, end));
         result.append(" ");
         start = end;
      }
      result.append(v.substring(start));
      return result.toString();
   }
}


Facelets es un framework basado en el servidor que permite definir la estructura general de las páginas (su layout) mediante plantillas.

Las principales características de los Facelets:

Tecnología estándar de despliegue a partir de JSF 2.0
Los Facelets sustituyen el uso de los JSP’s debido a que aportan diversas ventajas:
Utilizar un motor XML mucho más rápido.
Tienen un menor coste de compilación.
Construyen un Component Tree más ligero.
Soporte para Plantillas (Templates).
Dentro de una página Facelet no es posible utilizar los tags de JSP (<jsp: …/>) pero si podemos seguir utilizando los tags correspondientes a JSTL (<c: ... />) y las expresiones EL.

La sustitución de JSP por Facelets como lenguaje básico para definir la disposición de las páginas permite separar perfectamente las responsabilidades de cada parte del framework. La estructura de la página se define utilizando las etiquetas Facelets y los componentes específicos que deben presentar los datos de la aplicación utilizando etiquetas JSF.

El objetivo de las plantillas es reutilizar código, implementando partes de código ya escritas en una página conocida como “TEMPLATE” dentro de otras páginas conocidas como “CLIENTES DEL TEMPLATE”.

Un ejemplo son las aplicaciones donde siempre se muestra una cabecera, un menú y un pie de página, lo único que va cambiando es la sección de contenido. Implementando las plantillas de JSF 2.0 en nuestras paginas solo nos preocuparíamos por la parte del contenido de la pagina y nos olvidaríamos de cabeceras, menús laterales y pies de pagina.

Ejemplos de plantillas:


Para implementar plantillas en JSF 2.0 debemos implementar dos elementos:

Facelets Template, es la plantilla o diseño donde se especifican las diferentes secciones dentro de una página, aquí se definen cabeceras, menús laterales, pies de página y contenido.
Facelets Template Client, es la pagina que va a implementar al TEMPLATE, solo se preocupara por escribir alguna(s) secciones de código, ya que al importar el TEMPLATE se importan sus diferentes regiones. Los usuarios finales accederán siempre a esta plantilla-cliente.
Para comenzar a utilizar plantillas debemos importar el namespace “ui” para poder hacer uso de los tags de templates en nuestra página JSF.

xmlns:ui="http://java.sun.com/jsf/facelets"
Si queremos aplicar una hoja de estilos a nuestra página usamos la etiqueta:

<h:outputStylesheet library="css" name="styles.css"/>




Utilizando el mecanismo de plantillas de Facelets podremos encapsular los componentes comunes de una aplicación. De esta forma modificaremos el aspecto de nuestra página aplicando cambios sobre la plantilla, y no individualmente sobre cada página.

Veamos ahora cómo definir la plantilla de una aplicación. Supongamos que queremos definir una plantilla con la siguiente estructura:

Menú a la izquierda
Cabecera
Contenido variable, en función de la opción del menú que se ha seleccionado.
El usuario visualizará la misma página y sólo irá cambiando el contenido del centro de la pantalla. El aspecto de la página que queremos construir es el que aparece en la siguiente figura:


templates → masterLayout.xhtml

Dentro del subdirectorio “templates” creamos un “Facelets TEMPLATE” para posteriormente definir un “Facelets TEMPLATE CLIENT”

Importamos el namespace “ui” para poder hacer uso de los tags de templates en nuestra página JSF.
Carga de la hoja de estilos. Deberemos ubicar el fichero “styles.css” dentro de una carpeta resources/css en nuestro WebContent.
Sección para el Título. Para definir las secciones utilizamos el tag <ui:insert ...>
Sección para el header.
Sección para la barra lateral izquierda.
Sección para los contenidos.
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:ui="http://java.sun.com/jsf/facelets"
      xmlns:h="http://java.sun.com/jsf/html">
   
   <h:head>
      <title><ui:insert name="windowTitle"/></title>
      <h:outputStylesheet library="css" name="styles.css"/>
   </h:head>
   
   <h:body>   
      <div id="heading">
         <ui:insert name="heading">
            <ui:include src="/sections/planetarium/header.xhtml"/>
         </ui:insert>
      </div>
      
      
      <div id="sidebarLeft">
         <ui:insert name="sidebarLeft">
            <ui:include src="/sections/planetarium/sidebarLeft.xhtml"/>
         </ui:insert>
      </div>    
      
      <div id="content">
         <ui:insert name="content"/>
      </div>
      <ui:debug/>
   </h:body>
</html>
login.xhtml

Definir un “Facelets TEMPLATE CLIENT” haciendo uso del “Facelets TEMPLATE” definido antes.

Importamos el namespace “ui” para poder hacer uso de los tags de templates en nuestra página JSF.
Cargamos el TEMPLATE principal con el tag <ui:composition ...> y lo cerramos antes de la etiqueta de fin </body>.
Sección para el Título. Para cargar el contenido a mostrar en cada sección (definida con <ui:insert ...>) utilizamos el tag <ui:define ...> y lo cerramos tras los contenidos a incluir en dicha sección.
Sección para el header. Sobrescribe el definido por el TEMPLATE para cargar uno propio de la pantalla de login.
Sección para la barra lateral izquierda. Sobrescribe el definido por el TEMPLATE para cargar uno propio de la pantalla de login.
Sección para los contenidos.
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:ui="http://java.sun.com/jsf/facelets">
   <head><title>IGNORED</title></head>
   <body>
      <ui:composition template="/templates/masterLayout.xhtml">
         <ui:define name="windowTitle">
            #{msgs.loginTitle}
         </ui:define>

         <ui:define name="heading">
            <ui:include src="/sections/login/header.xhtml"/>
         </ui:define>

         <ui:define name="sidebarLeft">
            <ui:include src="/sections/login/sidebarLeft.xhtml"/>
         </ui:define>

         <ui:define name="content">
            <h:form>
               <h:panelGrid columns="2">
                  #{msgs.namePrompt}
                  <h:inputText id="name" value="#{user.name}"/>
                  #{msgs.passwordPrompt}
                  <h:inputSecret id="password" value="#{user.password}"/>
               </h:panelGrid>
               <p>
                  <h:commandButton value="#{msgs.loginButtonText}"
                                   action="planetarium"/>
               </p>
            </h:form>
         </ui:define>
      </ui:composition>
   </body>
</html>
planetarium.xhtml

“Facelets TEMPLATE CLIENT”

Importamos el namespace “ui” para poder hacer uso de los tags de templates en nuestra página JSF.

Cargamos el TEMPLATE principal con el tag <ui:composition ...> y lo cerramos antes de la etiqueta de fin </body>.

Sección para el Título. Para cargar el contenido a mostrar en cada sección (definida con <ui:insert ...>) utilizamos el tag <ui:define ...> y lo cerramos tras los contenidos a incluir en dicha sección.

Sección para los contenidos.

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:ui="http://java.sun.com/jsf/facelets">
   <head><title>IGNORED</title></head>
   <body>
      <ui:composition template="/templates/masterLayout.xhtml">     
         <ui:define name="windowTitle">
            #{msgs.planetariumTitle}
         </ui:define> 
         
         <ui:define name="content">
            #{msgs.planetariumWelcome}
         </ui:define>
      </ui:composition>
   </body>
</html>