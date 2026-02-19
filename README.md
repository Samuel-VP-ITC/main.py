# main.py
Calculadora básica con interfaz gráfica en flet y programada en python
```
import flet as ft

def main(page: ft.Page):
    page.title = "Calculadora Estática - TAP"
    page.window_width = 280
    page.window_height = 500
    page.window_resizable = False
    page.padding = 15

    # --- VARIABLES DE MEMORIA (ESTADO) ---
    memoria = {"num1": 0, "operacion": "", "nuevo_digito": False}

    # --- 1. SECCIÓN DISPLAY (ROJO) ---
    texto_display = ft.Text("0", size=20)

    def boton_click(e):
        # Si acabamos de pulsar una operación o hay un 0, reemplazamos el texto
        if texto_display.value == "0" or memoria["nuevo_digito"]:
            texto_display.value = e.control.data
            memoria["nuevo_digito"] = False
        else:
            texto_display.value += e.control.data
        page.update()

    # Función para los botones de + y -
    def operacion_click(e):
        memoria["num1"] = float(texto_display.value)
        memoria["operacion"] = e.control.data # Guarda "+" o "-"
        memoria["nuevo_digito"] = True
        page.update()

    # Función para el botón de Resultado (=)
    def calcular(e):
        num2 = float(texto_display.value)
        if memoria["operacion"] == "+":
            resultado = memoria["num1"] + num2
        elif memoria["operacion"] == "-":
            resultado = memoria["num1"] - num2
        else:
            resultado = num2

        # Convertimos a entero si no tiene decimales para que se vea limpio
        texto_display.value = str(int(resultado) if resultado.is_integer() else resultado)
        memoria["nuevo_digito"] = True
        page.update()

    seccion_display = ft.Container(
        content=texto_display,
        bgcolor=ft.Colors.BLACK12,
        height=70,
        alignment=ft.alignment.Alignment(0, 0),
        padding=10,
        border=ft.border.all(1, ft.Colors.RED)
    )

    # --- 2. SECCIÓN BOTONES NUMÉRICOS (AZUL) ---
    # He mantenido tus botones originales tal cual
    def crear_boton_azul(texto):
        return ft.Container(
            expand=1, height=50, bgcolor="blue",
            border=ft.border.all(1, "white"),
            alignment=ft.alignment.Alignment(0, 0),
            content=ft.Text(texto, color="white"),
            data=texto, on_click=boton_click
        )

    seccion_numeros = ft.Column(
        controls=[
            ft.Row(controls=[crear_boton_azul("1"), crear_boton_azul("2"), crear_boton_azul("3")]),
            ft.Row(controls=[crear_boton_azul("4"), crear_boton_azul("5"), crear_boton_azul("6")]),
            ft.Row(controls=[crear_boton_azul("7"), crear_boton_azul("8"), crear_boton_azul("9")]),
            ft.Row(controls=[crear_boton_azul("0")]),
        ],
        spacing=10
    )

    # --- 3. SECCIÓN OPERACIONES (VERDE) ---
    seccion_operaciones = ft.Row(
        controls=[
            # Botón Suma
            ft.Container(
                expand=1, height=60, bgcolor="green", border=ft.border.all(1, "white"),
                content=ft.Text("+", color="white", size=20, weight="bold"),
                alignment=ft.alignment.Alignment(0, 0),
                data="+", on_click=operacion_click
            ),
            # Botón Resta
            ft.Container(
                expand=1, height=60, bgcolor="green", border=ft.border.all(1, "white"),
                content=ft.Text("-", color="white", size=20, weight="bold"),
                alignment=ft.alignment.Alignment(0, 0),
                data="-", on_click=operacion_click
            ),
            # Botón Resultado (=)
            ft.Container(
                expand=1, height=60, bgcolor="green", border=ft.border.all(1, "white"),
                content=ft.Text("=", color="white", size=20, weight="bold"),
                alignment=ft.alignment.Alignment(0, 0),
                on_click=calcular
            ),
        ]
    )

    # --- CONSTRUCCIÓN FINAL ---
    page.add(
        ft.Column(
            controls=[
                seccion_display,
                ft.Text("Números:", size=12),
                seccion_numeros,
                ft.Divider(),
                ft.Text("Operaciones:", size=12),
                seccion_operaciones
            ],
            spacing=15
        )
    )

if __name__ == "__main__":
    ft.app(target=main)
```