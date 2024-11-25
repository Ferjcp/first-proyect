from kivy.app import App
from kivy.uix.screenmanager import Screen, ScreenManager
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.label import Label
from kivy.uix.button import Button
from kivy.uix.textinput import TextInput
from kivy.uix.popup import Popup
from kivy.uix.image import Image  # Importa Image para mostrar imágenes

# Lista de nombres permitidos
NOMBRES_PERMITIDOS = ["fernando", "servinorte"]

# Pantalla Principal
class PantallaPrincipal(Screen):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        layout = BoxLayout(orientation='vertical', padding=20, spacing=10)

        # Logo (imagen)
        layout.add_widget(Image(source="assets/logo.png", size_hint=(1, 0.4)))  # Ruta de la imagen

        # Título
        layout.add_widget(Label(text="Bienvenido al Formulario de SERVINORTE", font_size=20, halign="center", size_hint=(1, 0.2)))

        # Entrada de nombre
        self.input_nombre = TextInput(hint_text="Ingresa tu nombre", font_size=16, multiline=False, size_hint=(1, 0.2))
        layout.add_widget(self.input_nombre)

        # Botón Ingresar
        boton_ingresar = Button(text="Ingresar", size_hint=(1, 0.2))
        boton_ingresar.bind(on_press=self.verificar_nombre)
        layout.add_widget(boton_ingresar)

        self.add_widget(layout)

    def verificar_nombre(self, instance):
        nombre_usuario = self.input_nombre.text.strip().lower()

        if not nombre_usuario:
            self.mostrar_popup("Error", "Por favor, ingresa tu nombre.")
        elif nombre_usuario in NOMBRES_PERMITIDOS:
            self.manager.current = "codigo_sed"
        else:
            self.mostrar_popup("Acceso Denegado", f"Hola {nombre_usuario}, este formulario está reservado para 'SERVINORTE'.")

    def mostrar_popup(self, titulo, mensaje):
        popup = Popup(title=titulo, content=Label(text=mensaje), size_hint=(0.8, 0.4))
        popup.open()


# Pantalla de Código SED
class PantallaCodigoSED(Screen):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        layout = BoxLayout(orientation='vertical', padding=20, spacing=10)

        # Título
        layout.add_widget(Label(text="Formulario Código SED", font_size=20, halign="center", size_hint=(1, 0.2)))

        # Entrada de código SED
        self.input_codigo_sed = TextInput(hint_text="Ingresa el código SED", font_size=16, multiline=False, size_hint=(1, 0.2))
        layout.add_widget(self.input_codigo_sed)

        # Botones
        botones_layout = BoxLayout(size_hint=(1, 0.2), spacing=10)
        boton_atras = Button(text="Atrás")
        boton_atras.bind(on_press=self.volver_atras)
        botones_layout.add_widget(boton_atras)

        boton_siguiente = Button(text="Siguiente")
        boton_siguiente.bind(on_press=self.verificar_codigo)
        botones_layout.add_widget(boton_siguiente)

        layout.add_widget(botones_layout)

        self.add_widget(layout)

    def verificar_codigo(self, instance):
        if not self.input_codigo_sed.text.strip():
            self.mostrar_popup("Error", "Por favor, ingresa el código SED.")
        else:
            self.mostrar_popup("Éxito", "Código SED ingresado correctamente.")

    def volver_atras(self, instance):
        self.manager.current = "principal"

    def mostrar_popup(self, titulo, mensaje):
        popup = Popup(title=titulo, content=Label(text=mensaje), size_hint=(0.8, 0.4))
        popup.open()


# Administrador de Pantallas
class FormularioApp(App):
    def build(self):
        sm = ScreenManager()
        sm.add_widget(PantallaPrincipal(name="principal"))
        sm.add_widget(PantallaCodigoSED(name="codigo_sed"))
        return sm


# Ejecutar la aplicación
if __name__ == "__main__":
    FormularioApp().run()

