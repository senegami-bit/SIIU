# SIIU
PROYECTO FINAL 
# Importar librerías necesarias para interfaz gráfica y base de datos
import tkinter as tk  # Librería principal para crear interfaces gráficas
from tkinter import ttk, messagebox  # ttk para widgets temáticos, messagebox para ventanas de mensaje
import sqlite3  # Para trabajar con bases de datos SQLite
from datetime import datetime  # Para manejar fechas y horas
import re  # Para usar expresiones regulares en validaciones

# =============================================
# SISTEMA DE AUTENTICACIÓN DE USUARIOS
# =============================================

class LoginSystem:
    """
    Sistema de login con interfaz gráfica para autenticación de usuarios
    Permite el acceso a cualquier email válido con cualquier contraseña no vacía
    """
    
    def __init__(self, root):
        """Inicializa la ventana principal de login"""
        # Configuración de la ventana principal
        self.root = root  # Guardar referencia a la ventana principal
        self.root.title("SIIU - Sistema Integrado de Información")  # Establecer título de ventana
        self.root.geometry("500x650")  # Definir tamaño fijo de ventana (ancho x alto)
        self.root.configure(bg='white')  # Configurar color de fondo blanco
        self.root.resizable(False, False)  # Hacer que la ventana no sea redimensionable
        
        # Centrar ventana en pantalla
        self.center_window()  # Llamar método para centrar ventana
        
        # Crear componentes de la interfaz
        self.create_widgets()  # Llamar método para crear todos los elementos visuales
    
    def center_window(self):
        """Centra la ventana en el centro de la pantalla"""
        # Actualizar ventana para obtener dimensiones correctas
        self.root.update_idletasks()  # Forzar actualización de la interfaz
        # Calcular posición central
        width = 500  # Ancho de la ventana
        height = 650  # Alto de la ventana
        x = (self.root.winfo_screenwidth() // 2) - (width // 2)  # Calcular posición X centrada
        y = (self.root.winfo_screenheight() // 2) - (height // 2)  # Calcular posición Y centrada
        # Aplicar posición calculada
        self.root.geometry(f'{width}x{height}+{x}+{y}')  # Establecer geometría con posición
    
    def create_widgets(self):
        """Crea todos los elementos visuales de la interfaz de login"""
        # Frame principal con márgenes
        main_frame = tk.Frame(self.root, bg='white', padx=50, pady=60)  # Crear frame con padding
        main_frame.pack(expand=True, fill='both')  # Empacar frame para que ocupe todo el espacio
        
        # Crear secciones de la interfaz
        self.create_header(main_frame)      # Llamar método para crear encabezado
        self.create_login_form(main_frame)  # Llamar método para crear formulario
        self.create_footer(main_frame)      # Llamar método para crear pie de página
    
    def create_header(self, parent):
        """Crea el encabezado con el logo y título de la aplicación"""
        header_frame = tk.Frame(parent, bg='white')  # Crear frame para header
        header_frame.pack(pady=(0, 40))  # Empacar frame con espacio inferior
        
        # Título principal
        title_label = tk.Label(  # Crear etiqueta para título
            header_frame,  # Frame padre
            text="SIIU",  # Texto del título
            font=('Arial', 28, 'bold'),  # Fuente grande y negrita
            fg='#2c3e50',  # Color de texto gris oscuro
            bg='white'  # Color de fondo blanco
        )
        title_label.pack()  # Empacar etiqueta
        
        # Subtítulo descriptivo
        subtitle_label = tk.Label(  # Crear etiqueta para subtítulo
            header_frame,  # Frame padre
            text="Sistema Integrado de Información\nUniversidad Politécnica del Estado de Nayarit",  # Texto de dos líneas
            font=('Arial', 12),  # Fuente más pequeña
            fg='#7f8c8d',  # Color de texto gris
            bg='white',  # Color de fondo blanco
            justify='center'  # Centrar texto
        )
        subtitle_label.pack(pady=(10, 20))  # Empacar con espacio superior e inferior
    
    def create_login_form(self, parent):
        """Crea el formulario de inicio de sesión con campos de email y contraseña"""
        form_frame = tk.Frame(parent, bg='white')  # Crear frame para formulario
        form_frame.pack(pady=30, fill='x')  # Empacar con espacio y que ocupe ancho completo
        
        # Campo de email
        email_label = tk.Label(  # Crear etiqueta para campo email
            form_frame,  # Frame padre
            text="Correo Electrónico",  # Texto de la etiqueta
            font=('Arial', 12, 'bold'),  # Fuente negrita
            fg='#2c3e50',  # Color de texto
            bg='white',  # Color de fondo
            anchor='w'  # Alinear texto a la izquierda
        )
        email_label.pack(fill='x', pady=(0, 8))  # Empacar que ocupe ancho con espacio inferior
        
        self.email_entry = tk.Entry(  # Crear campo de entrada para email
            form_frame,  # Frame padre
            font=('Arial', 14),  # Fuente grande
            relief='solid',  # Borde sólido
            bd=1,  # Grosor del borde
            bg='#f8f9fa'  # Color de fondo gris claro
        )
        self.email_entry.pack(fill='x', pady=(0, 20), ipady=10)  # Empacar con relleno interno
        # Texto guía inicial
        self.email_entry.insert(0, "tucorreo@ejemplo.com")  # Insertar texto placeholder
        # Evento para limpiar texto guía al hacer clic
        self.email_entry.bind('<FocusIn>', self.clear_placeholder_email)  # Vincular evento focus
        
        # Campo de contraseña
        password_label = tk.Label(  # Crear etiqueta para campo contraseña
            form_frame,  # Frame padre
            text="Contraseña",  # Texto de la etiqueta
            font=('Arial', 12, 'bold'),  # Fuente negrita
            fg='#2c3e50',  # Color de texto
            bg='white',  # Color de fondo
            anchor='w'  # Alinear texto a la izquierda
        )
        password_label.pack(fill='x', pady=(0, 8))  # Empacar que ocupe ancho con espacio inferior
        
        self.password_entry = tk.Entry(  # Crear campo de entrada para contraseña
            form_frame,  # Frame padre
            font=('Arial', 14),  # Fuente grande
            show='•',  # Mostrar bullets en lugar de texto
            relief='solid',  # Borde sólido
            bd=1,  # Grosor del borde
            bg='#f8f9fa'  # Color de fondo gris claro
        )
        self.password_entry.pack(fill='x', pady=(0, 25), ipady=10)  # Empacar con relleno interno
        # Texto guía inicial
        self.password_entry.insert(0, "Ingresa tu contraseña")  # Insertar texto placeholder
        # Evento para limpiar texto guía al hacer clic
        self.password_entry.bind('<FocusIn>', self.clear_placeholder_password)  # Vincular evento focus
        
        # Botón de inicio de sesión
        login_button = tk.Button(  # Crear botón de login
            form_frame,  # Frame padre
            text="Iniciar Sesión",  # Texto del botón
            font=('Arial', 14, 'bold'),  # Fuente grande y negrita
            bg='#3498db',  # Color de fondo azul
            fg='white',  # Color de texto blanco
            relief='flat',  # Sin relieve (botón plano)
            cursor='hand2',  # Cursor de mano al pasar sobre botón
            command=self.login,  # Función a ejecutar al hacer clic
            height=2  # Altura del botón
        )
        login_button.pack(fill='x', pady=(10, 0))  # Empacar que ocupe ancho completo
        
        # Enlace para recuperación de contraseña
        forgot_link = tk.Label(  # Crear etiqueta como enlace
            form_frame,  # Frame padre
            text="¿Olvidaste tu contraseña?",  # Texto del enlace
            font=('Arial', 11),  # Fuente normal
            fg='#3498db',  # Color azul como enlace
            bg='white',  # Color de fondo blanco
            cursor='hand2'  # Cursor de mano
        )
        forgot_link.pack(pady=(20, 0))  # Empacar con espacio superior
        forgot_link.bind('<Button-1>', self.forgot_password)  # Vincular clic a función
        
        # Información de demostración
        demo_label = tk.Label(  # Crear etiqueta informativa
            form_frame,  # Frame padre
            text="Demo: Usa cualquier email válido y cualquier contraseña no vacía",  # Texto informativo
            font=('Arial', 10, 'bold'),  # Fuente pequeña y negrita
            fg='#2c3e50',  # Color de texto oscuro
            bg='white',  # Color de fondo blanco
            wraplength=400  # Ancho máximo antes de salto de línea
        )
        demo_label.pack(pady=(25, 0))  # Empacar con espacio superior
    
    def create_footer(self, parent):
        """Crea el pie de página con información de copyright"""
        footer_frame = tk.Frame(parent, bg='white')  # Crear frame para footer
        footer_frame.pack(side='bottom', pady=30)  # Empacar en parte inferior con espacio
        
        copyright_label = tk.Label(  # Crear etiqueta de copyright
            footer_frame,  # Frame padre
            text="© 2025 UPEN - Todos los derechos reservados",  # Texto de copyright
            font=('Arial', 10),  # Fuente pequeña
            fg='#95a5a6',  # Color de texto gris claro
            bg='white'  # Color de fondo blanco
        )
        copyright_label.pack()  # Empacar etiqueta
    
    def clear_placeholder_email(self, event):
        """Limpia el texto guía del campo email cuando recibe foco"""
        if self.email_entry.get() == "tucorreo@ejemplo.com":  # Si el texto es el placeholder
            self.email_entry.delete(0, tk.END)  # Borrar todo el texto
            self.email_entry.config(fg='black')  # Cambiar color a negro
    
    def clear_placeholder_password(self, event):
        """Limpia el texto guía del campo contraseña cuando recibe foco"""
        if self.password_entry.get() == "Ingresa tu contraseña":  # Si el texto es el placeholder
            self.password_entry.delete(0, tk.END)  # Borrar todo el texto
            self.password_entry.config(fg='black', show='•')  # Cambiar color y mostrar bullets
    
    def is_valid_email(self, email):
        """Valida el formato de email usando expresión regular"""
        pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'  # Patrón para validar email
        return re.match(pattern, email) is not None  # Retornar True si coincide con patrón
    
    def login(self):
        """Procesa el intento de inicio de sesión"""
        # Obtener credenciales
        email = self.email_entry.get().strip()  # Obtener email y quitar espacios
        password = self.password_entry.get()  # Obtener contraseña
        
        # Validaciones de campos
        if not email or email == "tucorreo@ejemplo.com":  # Si email está vacío o es placeholder
            messagebox.showwarning("Advertencia", "Por favor ingresa tu correo electrónico")  # Mostrar advertencia
            return  # Salir de la función
        
        if not self.is_valid_email(email):  # Si el email no tiene formato válido
            messagebox.showwarning("Advertencia", "Por favor ingresa un correo electrónico válido")  # Mostrar advertencia
            return  # Salir de la función
        
        if not password or password == "Ingresa tu contraseña":  # Si contraseña está vacía o es placeholder
            messagebox.showwarning("Advertencia", "Por favor ingresa tu contraseña")  # Mostrar advertencia
            return  # Salir de la función
        
        # Acceso concedido - abre aplicación principal
        self.open_siu_app(email)  # Llamar función para abrir aplicación principal
    
    def open_siu_app(self, user_email):
        """Cierra el login y abre la aplicación principal"""
        self.root.destroy()  # Cerrar ventana de login actual
        
        # Crear nueva ventana para aplicación principal
        root_siu = tk.Tk()  # Crear nueva ventana Tkinter
        app = SIUApp(root_siu, user_email)  # Crear instancia de aplicación principal
        root_siu.mainloop()  # Iniciar loop principal de la nueva ventana
    
    def forgot_password(self, event):
        """Maneja la solicitud de recuperación de contraseña"""
        email = self.email_entry.get()  # Obtener email del campo
        if email and email != "tucorreo@ejemplo.com" and self.is_valid_email(email):  # Si email es válido
            messagebox.showinfo("Recuperar Contraseña",  # Mostrar mensaje informativo
                              f"Se ha enviado un enlace de recuperación a: {email}")  # Con email del usuario
        else:
            messagebox.showinfo("Recuperar Contraseña",  # Mostrar mensaje
                              "Por favor ingresa tu correo electrónico para recuperar tu contraseña")  # Solicitar email

# =============================================
# SISTEMA DE GESTIÓN DE BASE DE DATOS
# =============================================

class Database:
    """Maneja todas las operaciones de base de datos SQLite"""
    
    def __init__(self):
        """Inicializa conexión a base de datos y crea tablas necesarias"""
        self.conn = sqlite3.connect('siu_app.db')  # Conectar a base de datos SQLite
        self.create_tables()  # Llamar método para crear tablas
    
    def create_tables(self):
        """Crea las tablas de la base de datos si no existen"""
        cursor = self.conn.cursor()  # Crear cursor para ejecutar consultas
        
        # Tabla de personas (estudiantes)
        cursor.execute('''  # Ejecutar consulta SQL
            CREATE TABLE IF NOT EXISTS personas (  # Crear tabla si no existe
                id INTEGER PRIMARY KEY AUTOINCREMENT,  # ID autoincremental
                nombre TEXT NOT NULL,  # Campo nombre obligatorio
                apellido TEXT NOT NULL,  # Campo apellido obligatorio
                email TEXT UNIQUE,  # Email único
                telefono TEXT,  # Teléfono opcional
                fecha_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP  # Fecha automática
            )
        ''')
        
        # Tabla de materias
        cursor.execute('''  # Ejecutar consulta SQL
            CREATE TABLE IF NOT EXISTS materias (  # Crear tabla si no existe
                id INTEGER PRIMARY KEY AUTOINCREMENT,  # ID autoincremental
                nombre TEXT NOT NULL,  # Nombre de materia obligatorio
                codigo TEXT UNIQUE,  # Código único
                creditos INTEGER DEFAULT 3,  # Créditos por defecto 3
                profesor TEXT,  # Nombre del profesor
                color TEXT DEFAULT '#2E86AB'  # Color para interfaz
            )
        ''')
        
        # Tabla de calificaciones
        cursor.execute('''  # Ejecutar consulta SQL
            CREATE TABLE IF NOT EXISTS calificaciones (  # Crear tabla si no existe
                id INTEGER PRIMARY KEY AUTOINCREMENT,  # ID autoincremental
                persona_id INTEGER,  # ID de persona (clave foránea)
                materia_id INTEGER,  # ID de materia (clave foránea)
                calificacion REAL NOT NULL,  # Calificación numérica
                fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP,  # Fecha automática
                tipo_evaluacion TEXT DEFAULT 'Parcial',  # Tipo de evaluación
                FOREIGN KEY (persona_id) REFERENCES personas (id),  # Relación con personas
                FOREIGN KEY (materia_id) REFERENCES materias (id)  # Relación con materias
            )
        ''')
        
        self.conn.commit()  # Guardar cambios en base de datos
        self.insert_default_data()  # Insertar datos iniciales
    
    def insert_default_data(self):
        """Inserta datos iniciales de materias en la base de datos"""
        cursor = self.conn.cursor()  # Crear cursor
        
        # Materias del tercer cuatrimestre por defecto
        materias_default = [  # Lista de tuplas con datos de materias
            ('INGLÉS III', 'ING301', 5, 'Mrs. Smith', '#2E86AB'),
            ('DESARROLLO DEL PENSAMIENTO Y TOMA DE DECISIONES', 'DPTD302', 4, 'Dr. García', '#A23B72'),
            ('CÁLCULO INTEGRAL', 'CAL303', 4, 'Dr. Hernández', '#F18F01'),
            ('TÓPICOS DE CALIDAD PARA EL DISEÑO DE SOFTWARE', 'TCDS304', 6, 'Ing. Martínez', '#1B998B'),
            ('BASES DE DATOS', 'BD305', 5, 'Lic. Rodríguez', '#9D4EDD'),
            ('PROGRAMACIÓN ORIENTADA A OBJETOS', 'POO306', 7, 'Mtro. López', '#E71D36'),
            ('PROYECTO INTEGRADOR I', 'PINT307', 4, 'Dra. González', '#34495E')
        ]
        
        cursor.executemany('''  # Ejecutar múltiples inserciones
            INSERT OR IGNORE INTO materias (nombre, codigo, creditos, profesor, color)  # Insertar ignorando duplicados
            VALUES (?, ?, ?, ?, ?)  # Valores parametrizados
        ''', materias_default)  # Lista de datos a insertar
        
        self.conn.commit()  # Guardar cambios
    
    def is_valid_phone(self, telefono):
        """Valida que el número de teléfono tenga 10 dígitos numéricos"""
        if not telefono:  # Si teléfono está vacío
            return True  # Teléfono es opcional, vacío es válido
        
        digits_only = re.sub(r'\D', '', telefono)  # Remover caracteres no numéricos
        
        if len(digits_only) != 10:  # Si no tiene exactamente 10 dígitos
            return False  # Teléfono inválido
        
        if not digits_only.isdigit():  # Si no son todos dígitos
            return False  # Teléfono inválido
            
        return True  # Teléfono válido
    
    # ========== MÉTODOS PARA GESTIÓN DE PERSONAS ==========
    
    def agregar_persona(self, nombre, apellido, email, telefono):
        """Agrega una nueva persona a la base de datos"""
        cursor = self.conn.cursor()  # Crear cursor
        try:
            # Validar formato de teléfono
            if telefono and not self.is_valid_phone(telefono):  # Si hay teléfono y no es válido
                return False, "El número de teléfono debe tener exactamente 10 dígitos numéricos"  # Retornar error
            
            cursor.execute('''  # Ejecutar consulta de inserción
                INSERT INTO personas (nombre, apellido, email, telefono)  # Insertar en tabla personas
                VALUES (?, ?, ?, ?)  # Valores parametrizados
            ''', (nombre, apellido, email, telefono))  # Parámetros
            self.conn.commit()  # Guardar cambios
            return True, "Persona agregada correctamente"  # Retornar éxito
        except sqlite3.IntegrityError:  # Si viola restricción única (email duplicado)
            return False, "El email ya existe"  # Retornar error
    
    def obtener_personas(self):
        """Obtiene todas las personas ordenadas por apellido y nombre"""
        cursor = self.conn.cursor()  # Crear cursor
        cursor.execute('SELECT * FROM personas ORDER BY apellido, nombre')  # Consulta ordenada
        return cursor.fetchall()  # Retornar todos los resultados
    
    def buscar_persona(self, texto):
        """Busca personas por nombre, apellido o email"""
        cursor = self.conn.cursor()  # Crear cursor
        cursor.execute('''  # Consulta con LIKE para búsqueda
            SELECT * FROM personas  # Seleccionar de personas
            WHERE nombre LIKE ? OR apellido LIKE ? OR email LIKE ?  # Buscar en tres campos
        ''', (f'%{texto}%', f'%{texto}%', f'%{texto}%'))  # Patrón de búsqueda
        return cursor.fetchall()  # Retornar resultados
    
    # ========== MÉTODOS PARA GESTIÓN DE CALIFICACIONES ==========
    
    def agregar_calificacion(self, persona_id, materia_id, calificacion, tipo_evaluacion):
        """Agrega una calificación para una persona en una materia"""
        cursor = self.conn.cursor()  # Crear cursor
        cursor.execute('''  # Ejecutar inserción
            INSERT INTO calificaciones (persona_id, materia_id, calificacion, tipo_evaluacion)  # Insertar calificación
            VALUES (?, ?, ?, ?)  # Valores parametrizados
        ''', (persona_id, materia_id, calificacion, tipo_evaluacion))  # Parámetros
        self.conn.commit()  # Guardar cambios
    
    def obtener_calificaciones_persona(self, persona_id):
        """Obtiene todas las calificaciones de una persona específica"""
        cursor = self.conn.cursor()  # Crear cursor
        cursor.execute('''  # Consulta con JOIN
            SELECT m.nombre, m.codigo, c.calificacion, c.tipo_evaluacion, c.fecha, m.color  # Seleccionar campos
            FROM calificaciones c  # Desde tabla calificaciones
            JOIN materias m ON c.materia_id = m.id  # Unir con materias
            WHERE c.persona_id = ?  # Filtrar por persona
            ORDER BY c.fecha DESC  # Ordenar por fecha descendente
        ''', (persona_id,))  # Parámetro persona_id
        return cursor.fetchall()  # Retornar resultados
    
    def obtener_promedio_persona(self, persona_id):
        """Calcula el promedio general de calificaciones de una persona"""
        cursor = self.conn.cursor()  # Crear cursor
        cursor.execute('''  # Consulta con función de agregación
            SELECT AVG(calificacion)  # Calcular promedio
            FROM calificaciones  # Desde tabla calificaciones
            WHERE persona_id = ?  # Filtrar por persona
        ''', (persona_id,))  # Parámetro persona_id
        return cursor.fetchone()[0] or 0  # Retornar promedio o 0 si es None
    
    # ========== MÉTODOS PARA GESTIÓN DE MATERIAS ==========
    
    def obtener_materias(self):
        """Obtiene todas las materias ordenadas por nombre"""
        cursor = self.conn.cursor()  # Crear cursor
        cursor.execute('SELECT * FROM materias ORDER BY nombre')  # Consulta ordenada
        return cursor.fetchall()  # Retornar resultados
    
    def obtener_materias_con_promedio(self, persona_id):
        """Obtiene materias con sus promedios para una persona específica"""
        cursor = self.conn.cursor()  # Crear cursor
        cursor.execute('''  # Consulta con LEFT JOIN y funciones de agregación
            SELECT m.id, m.nombre, m.codigo, m.color,  # Seleccionar campos de materia
                   AVG(c.calificacion) as promedio,  # Calcular promedio
                   COUNT(c.calificacion) as evaluaciones  # Contar evaluaciones
            FROM materias m  # Desde tabla materias
            LEFT JOIN calificaciones c ON m.id = c.materia_id AND c.persona_id = ?  # Unir con calificaciones filtradas
            GROUP BY m.id, m.nombre, m.codigo, m.color  # Agrupar por materia
        ''', (persona_id,))  # Parámetro persona_id
        return cursor.fetchall()  # Retornar resultados

# =============================================
# APLICACIÓN PRINCIPAL - SISTEMA INTEGRADO UNIVERSITARIO
# =============================================

class SIUApp:
    """Aplicación principal con interfaz para gestión académica"""
    
    def __init__(self, root, user_email):
        """Inicializa la aplicación principal con el email del usuario"""
        self.root = root  # Guardar referencia a ventana principal
        self.user_email = user_email  # Guardar email del usuario
        # Configuración de ventana principal
        self.root.title("SIIU - Sistema Integral Universitario")  # Establecer título
        self.root.geometry("400x700")  # Establecer tamaño
        self.root.configure(bg='#f8f9fa')  # Configurar color de fondo
        
        # Inicializar base de datos
        self.db = Database()  # Crear instancia de base de datos
        
        # Esquema de colores de la interfaz
        self.primary_color = '#2E86AB'  # Color primario azul
        self.secondary_color = '#A23B72'  # Color secundario morado
        self.accent_color = '#F18F01'  # Color de acento naranja
        self.success_color = '#1B998B'  # Color de éxito verde
        self.danger_color = '#E71D36'  # Color de peligro rojo
        
        # Construir interfaz de usuario
        self.create_ui()  # Llamar método para crear interfaz
        # Mostrar sección principal por defecto
        self.mostrar_seccion_principal()  # Mostrar dashboard principal
    
    def create_ui(self):
        """Construye la interfaz gráfica principal de la aplicación"""
        # ========== HEADER SUPERIOR ==========
        header_frame = tk.Frame(self.root, bg=self.primary_color, height=80)  # Crear frame de header
        header_frame.pack(fill='x', padx=10, pady=(10, 5))  # Empacar con padding
        header_frame.pack_propagate(False)  # Evitar que cambie tamaño
        
        # Información del usuario
        user_info_frame = tk.Frame(header_frame, bg=self.primary_color)  # Frame para info usuario
        user_info_frame.pack(side='left', padx=20, pady=10)  # Empacar a la izquierda
        
        # Título de la aplicación
        title_label = tk.Label(  # Crear etiqueta de título
            user_info_frame,  # Frame padre
            text="SIIU Mobile",  # Texto del título
            bg=self.primary_color,  # Color de fondo
            fg='white',  # Color de texto blanco
            font=('Arial', 16, 'bold')  # Fuente grande y negrita
        )
        title_label.pack(anchor='w')  # Empacar alineado a la izquierda
        
        # Email del usuario
        user_label = tk.Label(  # Crear etiqueta para usuario
            user_info_frame,  # Frame padre
            text=f"Usuario: {self.user_email}",  # Texto con email
            bg=self.primary_color,  # Color de fondo
            fg='white',  # Color de texto blanco
            font=('Arial', 9)  # Fuente pequeña
        )
        user_label.pack(anchor='w')  # Empacar alineado a la izquierda
        
        # Botón de cierre de sesión
        logout_btn = tk.Button(  # Crear botón de logout
            header_frame,  # Frame padre
            text="Cerrar Sesión",  # Texto del botón
            bg='white',  # Color de fondo blanco
            fg=self.primary_color,  # Color de texto azul
            font=('Arial', 9, 'bold'),  # Fuente negrita
            relief='flat',  # Sin relieve
            command=self.cerrar_sesion  # Función a ejecutar
        )
        logout_btn.pack(side='right', padx=20, pady=20)  # Empacar a la derecha
        
        # ========== BARRA DE NAVEGACIÓN ==========
        nav_frame = tk.Frame(self.root, bg='#e9ecef', height=50)  # Crear frame de navegación
        nav_frame.pack(fill='x', padx=10, pady=5)  # Empacar que ocupe ancho
        nav_frame.pack_propagate(False)  # Evitar que cambie tamaño
        
        # Botones de navegación entre secciones
        btn_principal = tk.Button(  # Botón para sección principal
            nav_frame,  # Frame padre
            text="Principal",  # Texto del botón
            bg='white',  # Color de fondo blanco
            fg=self.primary_color,  # Color de texto azul
            font=('Arial', 10, 'bold'),  # Fuente negrita
            relief='flat',  # Sin relieve
            command=self.mostrar_seccion_principal  # Función a ejecutar
        )
        btn_principal.pack(side='left', expand=True, fill='both', padx=2)  # Empacar expandible
        
        btn_personas = tk.Button(  # Botón para sección personas
            nav_frame,  # Frame padre
            text="Personas",  # Texto del botón
            bg='white',  # Color de fondo blanco
            fg=self.primary_color,  # Color de texto azul
            font=('Arial', 10),  # Fuente normal
            relief='flat',  # Sin relieve
            command=self.mostrar_seccion_personas  # Función a ejecutar
        )
        btn_personas.pack(side='left', expand=True, fill='both', padx=2)  # Empacar expandible
        
        btn_calificaciones = tk.Button(  # Botón para sección calificaciones
            nav_frame,  # Frame padre
            text="Calificaciones",  # Texto del botón
            bg='white',  # Color de fondo blanco
            fg=self.primary_color,  # Color de texto azul
            font=('Arial', 10),  # Fuente normal
            relief='flat',  # Sin relieve
            command=self.mostrar_seccion_calificaciones  # Función a ejecutar
        )
        btn_calificaciones.pack(side='left', expand=True, fill='both', padx=2)  # Empacar expandible
        
        btn_tramites = tk.Button(  # Botón para sección trámites
            nav_frame,  # Frame padre
            text="Trámites",  # Texto del botón
            bg='white',  # Color de fondo blanco
            fg=self.primary_color,  # Color de texto azul
            font=('Arial', 10),  # Fuente normal
            relief='flat',  # Sin relieve
            command=self.mostrar_seccion_tramites  # Función a ejecutar
        )
        btn_tramites.pack(side='left', expand=True, fill='both', padx=2)  # Empacar expandible

        # ========== ÁREA DE CONTENIDO PRINCIPAL CON SCROLL ==========
        self.main_scroll_frame = tk.Frame(self.root, bg='#f8f9fa')  # Frame para contenido con scroll
        self.main_scroll_frame.pack(fill='both', expand=True, padx=10, pady=5)  # Empacar expandible
        
        # Configuración de sistema de scroll
        self.canvas = tk.Canvas(self.main_scroll_frame, bg='#f8f9fa', highlightthickness=0)  # Canvas para scroll
        self.v_scrollbar = ttk.Scrollbar(self.main_scroll_frame, orient=tk.VERTICAL, command=self.canvas.yview)  # Scrollbar vertical
        self.h_scrollbar = ttk.Scrollbar(self.main_scroll_frame, orient=tk.HORIZONTAL, command=self.canvas.xview)  # Scrollbar horizontal
        
        # Frame que contendrá el contenido dinámico
        self.content_frame = tk.Frame(self.canvas, bg='#f8f9fa')  # Frame para contenido
        
        # Configurar scroll automático
        self.content_frame.bind(  # Vincular evento de configuración
            "<Configure>",  # Cuando el contenido cambia de tamaño
            lambda e: self.canvas.configure(scrollregion=self.canvas.bbox("all"))  # Actualizar región de scroll
        )
        
        # Posicionar elementos en el layout
        self.canvas.create_window((0, 0), window=self.content_frame, anchor="nw")  # Crear ventana en canvas
        self.canvas.configure(yscrollcommand=self.v_scrollbar.set, xscrollcommand=self.h_scrollbar.set)  # Configurar scrollbars
        
        # Grid layout para canvas y scrollbars
        self.canvas.grid(row=0, column=0, sticky='nsew')  # Posicionar canvas en grid
        self.v_scrollbar.grid(row=0, column=1, sticky='ns')  # Posicionar scrollbar vertical
        self.h_scrollbar.grid(row=1, column=0, sticky='ew')  # Posicionar scrollbar horizontal
        
        # Configurar expansión de elementos
        self.main_scroll_frame.grid_rowconfigure(0, weight=1)  # Fila 0 expandible
        self.main_scroll_frame.grid_columnconfigure(0, weight=1)  # Columna 0 expandible
    
    def cerrar_sesion(self):
        """Cierra la sesión actual y regresa a la pantalla de login"""
        if messagebox.askyesno("Cerrar Sesión", "¿Estás seguro de que quieres cerrar sesión?"):  # Confirmar cierre
            self.root.destroy()  # Cerrar ventana actual
            # Volver al sistema de login
            root_login = tk.Tk()  # Crear nueva ventana
            login_app = LoginSystem(root_login)  # Crear instancia de login
            root_login.mainloop()  # Iniciar loop de login
    
    def limpiar_contenido(self):
        """Elimina todo el contenido del área principal"""
        for widget in self.content_frame.winfo_children():  # Recorrer todos los widgets hijos
            widget.destroy()  # Destruir cada widget
    
    # ========== SECCIÓN DE TRÁMITES UNIVERSITARIOS ==========
    
    def mostrar_detalle_tramite(self, nombre_tramite):
        """Muestra ventana emergente con detalles completos de un trámite"""
        # Crear ventana de detalles
        detalle_window = tk.Toplevel(self.root)  # Crear ventana hija
        detalle_window.title(f"Trámite: {nombre_tramite}")  # Establecer título
        detalle_window.geometry("500x600")  # Establecer tamaño
        detalle_window.configure(bg='white')  # Configurar fondo blanco
        detalle_window.resizable(True, True)  # Permitir redimensionamiento
        
        # Configurar ventana como modal
        detalle_window.transient(self.root)  # Hacer ventana hija de principal
        detalle_window.grab_set()  # Hacer modal (capturar eventos)
        
        # Frame principal de la ventana
        main_frame = tk.Frame(detalle_window, bg='white', padx=20, pady=20)  # Frame con padding
        main_frame.pack(fill='both', expand=True)  # Empacar expandible
        
        # Título del trámite
        titulo_frame = tk.Frame(main_frame, bg='white')  # Frame para título
        titulo_frame.pack(fill='x', pady=(0, 20))  # Empacar con espacio inferior
        
        tk.Label(  # Crear etiqueta de título
            titulo_frame,  # Frame padre
            text=nombre_tramite,  # Nombre del trámite
            bg='white',  # Fondo blanco
            fg='#2c3e50',  # Color de texto
            font=('Arial', 18, 'bold')  # Fuente grande y negrita
        ).pack(anchor='w')  # Empacar alineado a izquierda
        
        # Área de información con scroll
        info_frame = tk.Frame(main_frame, bg='white')  # Frame para información
        info_frame.pack(fill='both', expand=True)  # Empacar expandible
        
        # Sistema de scroll para contenido extenso
        canvas = tk.Canvas(info_frame, bg='white', highlightthickness=0)  # Canvas para scroll
        scrollbar = ttk.Scrollbar(info_frame, orient=tk.VERTICAL, command=canvas.yview)  # Scrollbar vertical
        scrollable_frame = tk.Frame(canvas, bg='white')  # Frame scrollable
        
        scrollable_frame.bind(  # Vincular evento de configuración
            "<Configure>",  # Cuando cambia tamaño
            lambda e: canvas.configure(scrollregion=canvas.bbox("all"))  # Actualizar región de scroll
        )
        
        canvas.create_window((0, 0), window=scrollable_frame, anchor="nw")  # Crear ventana en canvas
        canvas.configure(yscrollcommand=scrollbar.set)  # Configurar scrollbar
        
        # Obtener información específica del trámite
        pasos, requisitos, tiempo, costo = self.obtener_info_tramite(nombre_tramite)  # Obtener datos
        
        # Descripción del trámite
        desc_frame = tk.Frame(scrollable_frame, bg='white')  # Frame para descripción
        desc_frame.pack(fill='x', pady=(0, 20))  # Empacar con espacio inferior
        
        tk.Label(  # Etiqueta "Descripción"
            desc_frame,  # Frame padre
            text="Descripción:",  # Texto de etiqueta
            bg='white',  # Fondo blanco
            fg='#2c3e50',  # Color de texto
            font=('Arial', 12, 'bold')  # Fuente negrita
        ).pack(anchor='w')  # Empacar alineado a izquierda
        
        desc_text = self.obtener_descripcion_tramite(nombre_tramite)  # Obtener descripción
        tk.Label(  # Etiqueta con descripción
            desc_frame,  # Frame padre
            text=desc_text,  # Texto de descripción
            bg='white',  # Fondo blanco
            fg='#7f8c8d',  # Color de texto gris
            font=('Arial', 10),  # Fuente normal
            wraplength=450,  # Ancho máximo
            justify='left'  # Justificar a izquierda
        ).pack(anchor='w', pady=(5, 0))  # Empacar con espacio superior
        
        # Información general (tiempo y costo)
        info_general_frame = tk.Frame(scrollable_frame, bg='#f8f9fa', relief='raised', bd=1)  # Frame con relieve
        info_general_frame.pack(fill='x', pady=(0, 20))  # Empacar con espacio inferior
        
        tk.Label(  # Etiqueta "Información General"
            info_general_frame,  # Frame padre
            text="Información General",  # Texto de etiqueta
            bg='#f8f9fa',  # Fondo gris claro
            fg='#2c3e50',  # Color de texto
            font=('Arial', 12, 'bold')  # Fuente negrita
        ).pack(anchor='w', padx=10, pady=10)  # Empacar con padding
        
        # Tiempo de procesamiento
        tiempo_frame = tk.Frame(info_general_frame, bg='#f8f9fa')  # Frame para tiempo
        tiempo_frame.pack(fill='x', padx=10, pady=(0, 5))  # Empacar con padding
        
        tk.Label(  # Etiqueta con tiempo
            tiempo_frame,  # Frame padre
            text=f"Tiempo de procesamiento: {tiempo}",  # Texto con tiempo
            bg='#f8f9fa',  # Fondo gris claro
            fg='#2c3e50',  # Color de texto
            font=('Arial', 10)  # Fuente normal
        ).pack(anchor='w')  # Empacar alineado a izquierda
        
        # Costo del trámite
        costo_frame = tk.Frame(info_general_frame, bg='#f8f9fa')  # Frame para costo
        costo_frame.pack(fill='x', padx=10, pady=(0, 10))  # Empacar con padding
        
        tk.Label(  # Etiqueta con costo
            costo_frame,  # Frame padre
            text=f"Costo: {costo}",  # Texto con costo
            bg='#f8f9fa',  # Fondo gris claro
            fg='#2c3e50',  # Color de texto
            font=('Arial', 10)  # Fuente normal
        ).pack(anchor='w')  # Empacar alineado a izquierda
        
        # Lista de requisitos
        requisitos_frame = tk.Frame(scrollable_frame, bg='white')  # Frame para requisitos
        requisitos_frame.pack(fill='x', pady=(0, 20))  # Empacar con espacio inferior
        
        tk.Label(  # Etiqueta "Requisitos"
            requisitos_frame,  # Frame padre
            text="Requisitos:",  # Texto de etiqueta
            bg='white',  # Fondo blanco
            fg='#2c3e50',  # Color de texto
            font=('Arial', 12, 'bold')  # Fuente negrita
        ).pack(anchor='w')  # Empacar alineado a izquierda
        
        # Mostrar cada requisito como item de lista
        for i, requisito in enumerate(requisitos, 1):  # Recorrer requisitos
            req_frame = tk.Frame(requisitos_frame, bg='white')  # Frame para cada requisito
            req_frame.pack(fill='x', pady=2)  # Empacar con espacio
            
            tk.Label(  # Etiqueta con requisito
                req_frame,  # Frame padre
                text=f"• {requisito}",  # Texto con bullet
                bg='white',  # Fondo blanco
                fg='#7f8c8d',  # Color de texto gris
                font=('Arial', 10),  # Fuente normal
                wraplength=450,  # Ancho máximo
                justify='left'  # Justificar a izquierda
            ).pack(anchor='w')  # Empacar alineado a izquierda
        
        # Pasos del trámite
        pasos_frame = tk.Frame(scrollable_frame, bg='white')  # Frame para pasos
        pasos_frame.pack(fill='x', pady=(0, 20))  # Empacar con espacio inferior
        
        tk.Label(  # Etiqueta "Pasos a seguir"
            pasos_frame,  # Frame padre
            text="Pasos a seguir:",  # Texto de etiqueta
            bg='white',  # Fondo blanco
            fg='#2c3e50',  # Color de texto
            font=('Arial', 12, 'bold')  # Fuente negrita
        ).pack(anchor='w')  # Empacar alineado a izquierda
        
        # Mostrar cada paso numerado
        for i, paso in enumerate(pasos, 1):  # Recorrer pasos con numeración
            paso_frame = tk.Frame(pasos_frame, bg='white', relief='raised', bd=1)  # Frame con relieve
            paso_frame.pack(fill='x', pady=2)  # Empacar con espacio
            
            # Número del paso con color
            num_frame = tk.Frame(paso_frame, bg=self.primary_color, width=30)  # Frame para número
            num_frame.pack(side='left', fill='y')  # Empacar a izquierda
            num_frame.pack_propagate(False)  # Evitar que cambie tamaño
            
            tk.Label(  # Etiqueta con número
                num_frame,  # Frame padre
                text=str(i),  # Número del paso
                bg=self.primary_color,  # Fondo azul
                fg='white',  # Texto blanco
                font=('Arial', 12, 'bold')  # Fuente negrita
            ).pack(expand=True)  # Empacar centrado
            
            # Descripción del paso
            desc_paso_frame = tk.Frame(paso_frame, bg='white')  # Frame para descripción
            desc_paso_frame.pack(fill='both', expand=True, padx=10, pady=10)  # Empacar expandible
            
            tk.Label(  # Etiqueta con descripción del paso
                desc_paso_frame,  # Frame padre
                text=paso,  # Texto del paso
                bg='white',  # Fondo blanco
                fg='#2c3e50',  # Color de texto
                font=('Arial', 10),  # Fuente normal
                wraplength=400,  # Ancho máximo
                justify='left'  # Justificar a izquierda
            ).pack(anchor='w')  # Empacar alineado a izquierda
        
        # Botón de solicitud
        boton_frame = tk.Frame(scrollable_frame, bg='white')  # Frame para botón
        boton_frame.pack(fill='x', pady=20)  # Empacar con espacio
        
        tk.Button(  # Botón para solicitar trámite
            boton_frame,  # Frame padre
            text="Solicitar este trámite",  # Texto del botón
            bg=self.success_color,  # Fondo verde
            fg='white',  # Texto blanco
            font=('Arial', 12, 'bold'),  # Fuente negrita
            relief='flat',  # Sin relieve
            padx=20,  # Padding horizontal
            pady=10,  # Padding vertical
            command=lambda: self.solicitar_tramite(nombre_tramite, detalle_window)  # Función a ejecutar
        ).pack()  # Empacar centrado
        
        # Posicionar elementos de scroll
        canvas.pack(side="left", fill="both", expand=True)  # Empacar canvas expandible
        scrollbar.pack(side="right", fill="y")  # Empacar scrollbar vertical
    
    def obtener_descripcion_tramite(self, nombre_tramite):
        """Retorna la descripción detallada de un trámite específico"""
        descripciones = {  # Diccionario con descripciones
            "Constancia de Estudios": "Documento oficial que certifica que el estudiante se encuentra inscrito y activo en la universidad. Es requerido para diversos trámites bancarios, becas, y procesos administrativos.",
            "Historial Académico": "Documento que contiene el registro completo de todas las materias cursadas, calificaciones obtenidas, promedios y situación académica del estudiante.",
            "Kardex": "Registro oficial que detalla el avance académico del estudiante, incluyendo materias aprobadas, reprobadas y en curso, con sus respectivas calificaciones.",
            "Credencial Universitaria": "Identificación oficial de la universidad que acredita la condición de estudiante. Incluye fotografía, datos personales y periodo de vigencia.",
            "Servicio Social": "Proceso para registrar, dar seguimiento y concluir el servicio social obligatorio requerido para la titulación.",
            "Prácticas Profesionales": "Sistema de gestión para el registro, supervisión y evaluación de las prácticas profesionales realizadas en empresas e instituciones.",
            "Baja Temporal": "Procedimiento para solicitar la suspensión temporal de estudios por uno o dos semestres por motivos académicos, personales o de salud."
        }
        return descripciones.get(nombre_tramite, "Descripción no disponible.")  # Retornar descripción o mensaje por defecto
    
    def obtener_info_tramite(self, nombre_tramite):
        """Retorna información estructurada de un trámite específico"""
        info_tramites = {  # Diccionario con información completa
            "Constancia de Estudios": (
                [  # Lista de pasos
                    "Iniciar sesión en el sistema SIIU",
                    "Seleccionar la opción 'Trámites'",
                    "Elegir 'Constancia de Estudios'",
                    "Verificar que no existan adeudos en la cuenta",
                    "Confirmar la solicitud",
                    "Descargar el documento en formato PDF"
                ],
                [  # Lista de requisitos
                    "Estar inscrito en el semestre actual",
                    "No tener adeudos con la institución",
                    "Tener al menos el 80% de asistencia",
                    "Estar al corriente en pagos administrativos"
                ],
                "24-48 horas hábiles",  # Tiempo de procesamiento
                "Gratuito"  # Costo
            ),
            "Historial Académico": (
                [  # Lista de pasos
                    "Acceder al portal SIIU con credenciales",
                    "Navegar a la sección de Trámites",
                    "Seleccionar 'Historial Académico'",
                    "Verificar información personal",
                    "Seleccionar formato (PDF o físico)",
                    "Confirmar solicitud y descargar"
                ],
                [  # Lista de requisitos
                    "Haber cursado al menos un semestre",
                    "No tener materias con calificación pendiente",
                    "Cuenta estudiantil activa",
                    "Identificación oficial vigente"
                ],
                "48-72 horas hábiles",  # Tiempo de procesamiento
                "$50.00 MXN (formato físico)"  # Costo
            ),
            # ... (estructuras similares para otros trámites)
        }
        return info_tramites.get(nombre_tramite, ([], [], "No especificado", "No especificado"))  # Retornar info o valores por defecto
    
    def solicitar_tramite(self, nombre_tramite, ventana):
        """Procesa la solicitud formal de un trámite"""
        respuesta = messagebox.askyesno(  # Mostrar diálogo de confirmación
            "Solicitar Trámite",  # Título
            f"¿Confirmas que deseas solicitar el trámite:\n\n'{nombre_tramite}'?\n\n"  # Mensaje
            f"Recibirás un correo de confirmación en:\n{self.user_email}"  # Información de contacto
        )
        
        if respuesta:  # Si usuario confirma
            messagebox.showinfo(  # Mostrar mensaje de éxito
                "Solicitud Enviada",  # Título
                f"Tu solicitud para '{nombre_tramite}' ha sido registrada.\n\n"  # Mensaje
                f"Correo: Se ha enviado un comprobante a: {self.user_email}\n"  # Confirmación email
                f"Tiempo: Tiempo estimado de procesamiento: 24-48 horas\n"  # Información tiempo
                f"Seguimiento: Puedes dar seguimiento en la sección de Trámites"  # Instrucciones seguimiento
            )
            ventana.destroy()  # Cerrar ventana de detalles
    
    # ========== SECCIÓN PRINCIPAL/DASHBOARD ==========
    
    def mostrar_seccion_principal(self):
        """Muestra el dashboard principal con resumen del sistema"""
        self.limpiar_contenido()  # Limpiar contenido anterior
        
        # Frame de bienvenida
        welcome_frame = tk.Frame(self.content_frame, bg='white', relief='raised', bd=1)  # Frame con relieve
        welcome_frame.pack(fill='x', pady=(0, 10))  # Empacar con espacio inferior
        
        # Extraer nombre de usuario del email
        username = self.user_email.split('@')[0].capitalize()  # Obtener nombre antes de @ y capitalizar
        
        # Mensaje personalizado de bienvenida
        tk.Label(welcome_frame, text=f"¡Bienvenido, {username}!", bg='white',  # Etiqueta de bienvenida
                fg='#2c3e50', font=('Arial', 14, 'bold')).pack(anchor='w', padx=10, pady=5)  # Empacar con padding
        tk.Label(welcome_frame, text=f"Correo: {self.user_email}", bg='white',  # Etiqueta con email
                fg='#7f8c8d', font=('Arial', 10)).pack(anchor='w', padx=10, pady=(0, 10))  # Empacar con padding
        
        # Estadísticas del sistema
        stats_frame = tk.Frame(self.content_frame, bg='white', relief='raised', bd=1)  # Frame para estadísticas
        stats_frame.pack(fill='x', pady=(0, 10))  # Empacar con espacio inferior
        
        # Obtener datos de la base de datos
        personas = self.db.obtener_personas()  # Obtener lista de personas
        materias = self.db.obtener_materias()  # Obtener lista de materias
        
        tk.Label(stats_frame, text="Resumen General", bg='white',  # Etiqueta "Resumen General"
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', padx=10, pady=5)  # Empacar con padding
        
        # Mostrar estadísticas
        info_text = f"• {len(personas)} Estudiantes registrados\n• {len(materias)} Materias disponibles\n• Sistema actualizado"  # Texto con estadísticas
        tk.Label(stats_frame, text=info_text, bg='white', fg='#7f8c8d',  # Etiqueta con estadísticas
                font=('Arial', 10), justify='left').pack(anchor='w', padx=10, pady=(0, 10))  # Empacar con padding
        
        # Lista de últimos estudiantes (si existen)
        if personas:  # Si hay personas registradas
            tk.Label(self.content_frame, text="Últimos Estudiantes", bg='#f8f9fa',  # Etiqueta "Últimos Estudiantes"
                    fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', pady=(10, 5))  # Empacar con espacio
            
            # Mostrar últimas 5 personas registradas
            for persona in personas[-5:]:  # Recorrer últimas 5 personas
                persona_frame = tk.Frame(self.content_frame, bg='white', relief='raised', bd=1)  # Frame para persona
                persona_frame.pack(fill='x', pady=2)  # Empacar con espacio
                
                nombre_completo = f"{persona[1]} {persona[2]}"  # Concatenar nombre y apellido
                tk.Label(persona_frame, text=nombre_completo, bg='white',  # Etiqueta con nombre
                        fg='#2c3e50', font=('Arial', 10, 'bold')).pack(anchor='w', padx=10, pady=5)  # Empacar con padding
                tk.Label(persona_frame, text=f"Email: {persona[3]}", bg='white',  # Etiqueta con email
                        fg='#7f8c8d', font=('Arial', 8)).pack(anchor='w', padx=10, pady=(0, 5))  # Empacar con padding
    
    # ========== SECCIÓN DE GESTIÓN DE PERSONAS/ESTUDIANTES ==========
    
    def mostrar_seccion_personas(self):
        """Muestra la interfaz para gestión de estudiantes"""
        self.limpiar_contenido()  # Limpiar contenido anterior
        
        # Formulario para agregar nuevas personas
        form_frame = tk.Frame(self.content_frame, bg='white', relief='raised', bd=1)  # Frame para formulario
        form_frame.pack(fill='x', pady=(0, 10))  # Empacar con espacio inferior
        
        tk.Label(form_frame, text="Agregar Nueva Persona", bg='white',  # Etiqueta del formulario
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', padx=10, pady=5)  # Empacar con padding
        
        # Campos del formulario
        fields_frame = tk.Frame(form_frame, bg='white')  # Frame para campos
        fields_frame.pack(fill='x', padx=10, pady=5)  # Empacar con padding
        
        # Campo: Nombre
        tk.Label(fields_frame, text="Nombre:", bg='white').grid(row=0, column=0, sticky='w', pady=2)  # Etiqueta nombre
        entry_nombre = tk.Entry(fields_frame, width=20)  # Campo de entrada para nombre
        entry_nombre.grid(row=0, column=1, pady=2, padx=5)  # Posicionar en grid
        
        # Campo: Apellido
        tk.Label(fields_frame, text="Apellido:", bg='white').grid(row=1, column=0, sticky='w', pady=2)  # Etiqueta apellido
        entry_apellido = tk.Entry(fields_frame, width=20)  # Campo de entrada para apellido
        entry_apellido.grid(row=1, column=1, pady=2, padx=5)  # Posicionar en grid
        
        # Campo: Email
        tk.Label(fields_frame, text="Email:", bg='white').grid(row=2, column=0, sticky='w', pady=2)  # Etiqueta email
        entry_email = tk.Entry(fields_frame, width=20)  # Campo de entrada para email
        entry_email.grid(row=2, column=1, pady=2, padx=5)  # Posicionar en grid
        
        # Campo: Teléfono con validación
        tk.Label(fields_frame, text="Teléfono:", bg='white').grid(row=3, column=0, sticky='w', pady=2)  # Etiqueta teléfono
        entry_telefono = tk.Entry(fields_frame, width=20)  # Campo de entrada para teléfono
        entry_telefono.grid(row=3, column=1, pady=2, padx=5)  # Posicionar en grid
        
        # Función de validación para teléfono (solo números, máximo 10 dígitos)
        def validate_phone_input(char, entry_value):  # Función de validación
            if char == '':  # Permitir borrado (tecla borrar)
                return True  # Válido
            if not char.isdigit():  # Solo dígitos
                return False  # Inválido
            current_text = entry_telefono.get()  # Obtener texto actual
            new_text = current_text + char  # Texto con nuevo carácter
            if len(new_text) > 10:  # Máximo 10 dígitos
                return False  # Inválido
            return True  # Válido
        
        # Registrar validación
        vcmd = (self.root.register(validate_phone_input), '%S', '%P')  # Registrar función de validación
        entry_telefono.configure(validate='key', validatecommand=vcmd)  # Configurar validación en campo
        
        # Función para guardar nueva persona
        def guardar_persona():  # Función para guardar
            nombre = entry_nombre.get().strip()  # Obtener y limpiar nombre
            apellido = entry_apellido.get().strip()  # Obtener y limpiar apellido
            email = entry_email.get().strip()  # Obtener y limpiar email
            telefono = entry_telefono.get().strip()  # Obtener y limpiar teléfono
            
            # Validar campos obligatorios
            if not nombre or not apellido:  # Si nombre o apellido están vacíos
                messagebox.showwarning("Advertencia", "Nombre y apellido son obligatorios")  # Mostrar advertencia
                return  # Salir de función
            
            # Validar formato de teléfono
            if telefono and not self.db.is_valid_phone(telefono):  # Si hay teléfono y no es válido
                messagebox.showwarning("Advertencia", "El número de teléfono debe tener exactamente 10 dígitos numéricos")  # Mostrar advertencia
                return  # Salir de función
            
            # Intentar guardar en base de datos
            success, message = self.db.agregar_persona(nombre, apellido, email, telefono)  # Llamar método de base de datos
            
            if success:  # Si se guardó correctamente
                messagebox.showinfo("Éxito", message)  # Mostrar mensaje de éxito
                # Limpiar formulario
                entry_nombre.delete(0, tk.END)  # Limpiar campo nombre
                entry_apellido.delete(0, tk.END)  # Limpiar campo apellido
                entry_email.delete(0, tk.END)  # Limpiar campo email
                entry_telefono.delete(0, tk.END)  # Limpiar campo teléfono
                # Actualizar lista
                self.mostrar_lista_personas()  # Actualizar lista de personas
            else:  # Si hubo error
                messagebox.showerror("Error", message)  # Mostrar mensaje de error
        
        # Botón de guardado
        tk.Button(form_frame, text="Guardar Persona", bg=self.success_color,  # Botón guardar
                 fg='white', command=guardar_persona).pack(pady=10)  # Empacar con espacio
        
        # Mostrar lista existente de personas
        self.mostrar_lista_personas()  # Llamar método para mostrar lista
    
    def mostrar_lista_personas(self):
        """Muestra la lista de todas las personas en formato tabla"""
        lista_frame = tk.Frame(self.content_frame, bg='#f8f9fa')  # Frame para lista
        lista_frame.pack(fill='both', expand=True)  # Empacar expandible
        
        tk.Label(lista_frame, text="Lista de Estudiantes", bg='#f8f9fa',  # Etiqueta "Lista de Estudiantes"
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', pady=(10, 5))  # Empacar con espacio
        
        # Contenedor para tabla con scroll
        tree_frame = tk.Frame(lista_frame, bg='#f8f9fa', height=300)  # Frame para tabla
        tree_frame.pack(fill='both', expand=True)  # Empacar expandible
        tree_frame.pack_propagate(False)  # Evitar que cambie tamaño
        
        # Configurar tabla (Treeview)
        columns = ('ID', 'Nombre', 'Apellido', 'Email', 'Teléfono')  # Definir columnas
        tree = ttk.Treeview(tree_frame, columns=columns, show='headings', height=12)  # Crear treeview
        
        # Configurar columnas
        for col in columns:  # Recorrer cada columna
            tree.heading(col, text=col)  # Establecer heading
        
        # Anchos de columnas
        tree.column('ID', width=50, anchor='center')  # Columna ID centrada
        tree.column('Nombre', width=100, anchor='w')  # Columna nombre alineada izquierda
        tree.column('Apellido', width=100, anchor='w')  # Columna apellido alineada izquierda
        tree.column('Email', width=150, anchor='w')  # Columna email alineada izquierda
        tree.column('Teléfono', width=100, anchor='center')  # Columna teléfono centrada
        
        # Scrollbars
        v_scrollbar = ttk.Scrollbar(tree_frame, orient=tk.VERTICAL, command=tree.yview)  # Scrollbar vertical
        h_scrollbar = ttk.Scrollbar(tree_frame, orient=tk.HORIZONTAL, command=tree.xview)  # Scrollbar horizontal
        tree.configure(yscrollcommand=v_scrollbar.set, xscrollcommand=h_scrollbar.set)  # Configurar scrollbars
        
        # Layout de tabla y scrollbars
        tree.grid(row=0, column=0, sticky='nsew')  # Posicionar tabla en grid
        v_scrollbar.grid(row=0, column=1, sticky='ns')  # Posicionar scrollbar vertical
        h_scrollbar.grid(row=1, column=0, sticky='ew')  # Posicionar scrollbar horizontal
        
        # Configurar expansión
        tree_frame.grid_rowconfigure(0, weight=1)  # Fila 0 expandible
        tree_frame.grid_columnconfigure(0, weight=1)  # Columna 0 expandible
        
        # Insertar datos de personas
        personas = self.db.obtener_personas()  # Obtener personas de base de datos
        for persona in personas:  # Recorrer cada persona
            tree.insert('', tk.END, values=persona)  # Insertar fila en tabla
    
    # ========== SECCIÓN DE GESTIÓN DE CALIFICACIONES ==========
    
    def mostrar_seccion_calificaciones(self):
        """Muestra la interfaz para gestión de calificaciones"""
        self.limpiar_contenido()  # Limpiar contenido anterior
        
        # Selector de estudiante
        seleccion_frame = tk.Frame(self.content_frame, bg='white', relief='raised', bd=1)  # Frame para selector
        seleccion_frame.pack(fill='x', pady=(0, 10))  # Empacar con espacio inferior
        
        tk.Label(seleccion_frame, text="Seleccionar Estudiante", bg='white',  # Etiqueta "Seleccionar Estudiante"
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', padx=10, pady=5)  # Empacar con padding
        
        # Obtener lista de personas
        personas = self.db.obtener_personas()  # Obtener personas de base de datos
        persona_var = tk.StringVar()  # Variable para almacenar selección
        
        if personas:  # Si hay personas registradas
            # Crear combobox con nombres de personas
            nombres_personas = [f"{p[0]} - {p[1]} {p[2]}" for p in personas]  # Lista de nombres formateados
            combo_personas = ttk.Combobox(seleccion_frame, textvariable=persona_var,  # Crear combobox
                                         values=nombres_personas, state='readonly')  # Valores y estado solo lectura
            combo_personas.pack(fill='x', padx=10, pady=5)  # Empacar que ocupe ancho
            # Evento al seleccionar persona
            combo_personas.bind('<<ComboboxSelected>>',  # Vincular evento de selección
                               lambda e: self.mostrar_calificaciones_persona(personas[combo_personas.current()][0]))  # Llamar función con ID
        else:  # Si no hay personas
            tk.Label(seleccion_frame, text="No hay personas registradas",  # Mostrar mensaje
                    bg='white', fg='#7f8c8d').pack(pady=10)  # Empacar con espacio
    
    def mostrar_calificaciones_persona(self, persona_id):
        """Muestra calificaciones y formulario para una persona específica"""
        # Limpiar contenido excepto el selector
        for widget in self.content_frame.winfo_children()[1:]:  # Recorrer widgets desde el segundo
            widget.destroy()  # Destruir widget
        
        # Formulario para agregar calificaciones
        form_frame = tk.Frame(self.content_frame, bg='white', relief='raised', bd=1)  # Frame para formulario
        form_frame.pack(fill='x', pady=(0, 10))  # Empacar con espacio inferior
        
        tk.Label(form_frame, text="Agregar Calificación", bg='white',  # Etiqueta "Agregar Calificación"
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', padx=10, pady=5)  # Empacar con padding
        
        fields_frame = tk.Frame(form_frame, bg='white')  # Frame para campos
        fields_frame.pack(fill='x', padx=10, pady=5)  # Empacar con padding
        
        # Campo: Materia
        tk.Label(fields_frame, text="Materia:", bg='white').grid(row=0, column=0, sticky='w', pady=2)  # Etiqueta materia
        materias = self.db.obtener_materias()  # Obtener materias de base de datos
        materia_var = tk.StringVar()  # Variable para materia seleccionada
        combo_materias = ttk.Combobox(fields_frame, textvariable=materia_var,  # Combobox para materias
                                     values=[f"{m[1]} - {m[0]}" for m in materias],  # Valores formateados
                                     state='readonly')  # Solo lectura
        combo_materias.grid(row=0, column=1, pady=2, padx=5)  # Posicionar en grid
        
        # Campo: Calificación
        tk.Label(fields_frame, text="Calificación:", bg='white').grid(row=1, column=0, sticky='w', pady=2)  # Etiqueta calificación
        entry_calificacion = tk.Entry(fields_frame, width=10)  # Campo para calificación
        entry_calificacion.grid(row=1, column=1, pady=2, padx=5)  # Posicionar en grid
        
        # Campo: Tipo de evaluación
        tk.Label(fields_frame, text="Tipo:", bg='white').grid(row=2, column=0, sticky='w', pady=2)  # Etiqueta tipo
        tipo_var = tk.StringVar(value='Parcial')  # Variable con valor por defecto
        combo_tipo = ttk.Combobox(fields_frame, textvariable=tipo_var,  # Combobox para tipos
                                 values=['Parcial', 'Final', 'Proyecto', 'Tarea'],  # Valores posibles
                                 state='readonly')  # Solo lectura
        combo_tipo.grid(row=2, column=1, pady=2, padx=5)  # Posicionar en grid
        
        # Función para guardar calificación
        def guardar_calificacion():  # Función para guardar calificación
            if materia_var.get() and entry_calificacion.get():  # Si ambos campos tienen valores
                try:
                    calificacion = float(entry_calificacion.get())  # Convertir a número
                    if 0 <= calificacion <= 100:  # Si está en rango válido
                        materia_id = materias[combo_materias.current()][0]  # Obtener ID de materia seleccionada
                        self.db.agregar_calificacion(  # Llamar método para agregar calificación
                            persona_id, materia_id, calificacion, tipo_var.get()  # Parámetros
                        )
                        messagebox.showinfo("Éxito", "Calificación agregada correctamente")  # Mostrar éxito
                        entry_calificacion.delete(0, tk.END)  # Limpiar campo calificación
                        # Actualizar vistas
                        self.mostrar_cards_materias(persona_id)  # Actualizar cards de materias
                        self.mostrar_historial_calificaciones(persona_id)  # Actualizar historial
                    else:  # Si calificación fuera de rango
                        messagebox.showerror("Error", "La calificación debe estar entre 0 y 100")  # Mostrar error
                except ValueError:  # Si no se puede convertir a número
                    messagebox.showerror("Error", "La calificación debe ser un número")  # Mostrar error
            else:  # Si campos vacíos
                messagebox.showwarning("Advertencia", "Todos los campos son obligatorios")  # Mostrar advertencia
        
        tk.Button(form_frame, text="Guardar Calificación", bg=self.success_color,  # Botón guardar
                 fg='white', command=guardar_calificacion).pack(pady=10)  # Empacar con espacio
        
        # Mostrar resumen de materias
        self.mostrar_cards_materias(persona_id)  # Llamar método para mostrar cards
        # Mostrar historial detallado
        self.mostrar_historial_calificaciones(persona_id)  # Llamar método para mostrar historial
    
    def mostrar_cards_materias(self, persona_id):
        """Muestra tarjetas con promedios por materia"""
        cards_container = tk.Frame(self.content_frame, bg='#f8f9fa')  # Frame contenedor
        cards_container.pack(fill='x', pady=(0, 10))  # Empacar con espacio inferior
        
        tk.Label(cards_container, text="Materias y Promedios", bg='#f8f9fa',  # Etiqueta "Materias y Promedios"
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', pady=(10, 5))  # Empacar con espacio
        
        # Contenedor scrollable para las cards
        scroll_container = tk.Frame(cards_container, bg='#f8f9fa', height=200)  # Frame para scroll
        scroll_container.pack(fill='both', expand=True)  # Empacar expandible
        scroll_container.pack_propagate(False)  # Evitar que cambie tamaño
        
        # Sistema de scroll
        canvas = tk.Canvas(scroll_container, bg='#f8f9fa', highlightthickness=0)  # Canvas para scroll
        scrollbar = ttk.Scrollbar(scroll_container, orient=tk.VERTICAL, command=canvas.yview)  # Scrollbar vertical
        scrollable_frame = tk.Frame(canvas, bg='#f8f9fa')  # Frame scrollable
        
        scrollable_frame.bind(  # Vincular evento de configuración
            "<Configure>",  # Cuando cambia tamaño
            lambda e: canvas.configure(scrollregion=canvas.bbox("all"))  # Actualizar región de scroll
        )
        
        canvas.create_window((0, 0), window=scrollable_frame, anchor="nw")  # Crear ventana en canvas
        canvas.configure(yscrollcommand=scrollbar.set)  # Configurar scrollbar
        
        canvas.pack(side="left", fill="both", expand=True)  # Empacar canvas expandible
        scrollbar.pack(side="right", fill="y")  # Empacar scrollbar vertical
        
        # Obtener materias con promedios
        materias_con_promedio = self.db.obtener_materias_con_promedio(persona_id)  # Obtener datos
        
        # Crear una card por materia
        for materia in materias_con_promedio:  # Recorrer cada materia
            card = tk.Frame(scrollable_frame, bg='white', relief='raised', bd=1)  # Frame para card
            card.pack(fill='x', pady=2, padx=5)  # Empacar con espacio
            
            # Barra lateral de color
            color_frame = tk.Frame(card, bg=materia[3], width=5)  # Frame para color
            color_frame.pack(side='left', fill='y')  # Empacar a izquierda
            color_frame.pack_propagate(False)  # Evitar que cambie tamaño
            
            # Información de la materia
            info_frame = tk.Frame(card, bg='white')  # Frame para información
            info_frame.pack(fill='x', expand=True, padx=10, pady=8)  # Empacar expandible
            
            # Nombre y código
            tk.Label(info_frame, text=materia[1], bg='white',  # Etiqueta nombre materia
                    fg='#2c3e50', font=('Arial', 10, 'bold')).pack(anchor='w')  # Empacar alineado izquierda
            tk.Label(info_frame, text=f"Código: {materia[2]}", bg='white',  # Etiqueta código
                    fg='#7f8c8d', font=('Arial', 8)).pack(anchor='w')  # Empacar alineado izquierda
            
            # Promedio y evaluaciones
            promedio = materia[4] if materia[4] else "Sin calificaciones"  # Promedio o mensaje
            evaluaciones = materia[5] if materia[5] else 0  # Número de evaluaciones
            
            # Color según promedio
            if materia[4]:  # Si hay promedio calculado
                color_promedio = self.success_color if materia[4] >= 70 else self.danger_color  # Verde si >=70, rojo si no
                promedio_text = f"Promedio: {promedio:.1f}"  # Texto con formato
            else:  # Si no hay calificaciones
                color_promedio = '#7f8c8d'  # Color gris
                promedio_text = f"Promedio: {promedio}"  # Texto sin formato
            
            tk.Label(info_frame, text=promedio_text, bg='white',  # Etiqueta promedio
                    fg=color_promedio, font=('Arial', 9, 'bold')).pack(anchor='w')  # Empacar con color
            tk.Label(info_frame, text=f"Evaluaciones: {evaluaciones}", bg='white',  # Etiqueta evaluaciones
                    fg='#7f8c8d', font=('Arial', 8)).pack(anchor='w')  # Empacar alineado izquierda
    
    def mostrar_historial_calificaciones(self, persona_id):
        """Muestra historial completo de calificaciones en tabla"""
        historial_frame = tk.Frame(self.content_frame, bg='#f8f9fa')  # Frame para historial
        historial_frame.pack(fill='both', expand=True)  # Empacar expandible
        
        tk.Label(historial_frame, text="Historial de Calificaciones", bg='#f8f9fa',  # Etiqueta "Historial"
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', pady=(10, 5))  # Empacar con espacio
        
        # Obtener datos
        calificaciones = self.db.obtener_calificaciones_persona(persona_id)  # Obtener calificaciones
        promedio_general = self.db.obtener_promedio_persona(persona_id)  # Obtener promedio general
        
        # Mostrar promedio general
        if calificaciones:  # Si hay calificaciones
            promedio_label = tk.Label(historial_frame,  # Crear etiqueta promedio
                                text=f"Promedio General: {promedio_general:.1f}",  # Texto con promedio
                                bg='#f8f9fa', fg='#2c3e50', font=('Arial', 10, 'bold'))  # Estilo
            promedio_label.pack(anchor='w', pady=(0, 10))  # Empacar alineado izquierda
        
        # Contenedor para tabla
        tree_container = tk.Frame(historial_frame, bg='#f8f9fa', height=250)  # Frame para tabla
        tree_container.pack(fill='both', expand=True)  # Empacar expandible
        tree_container.pack_propagate(False)  # Evitar que cambie tamaño
        
        tree_frame = tk.Frame(tree_container, bg='#f8f9fa')  # Frame interno para tabla
        tree_frame.pack(fill='both', expand=True)  # Empacar expandible
        
        if calificaciones:  # Si hay calificaciones
            # Configurar tabla
            columns = ('Materia', 'Código', 'Calificación', 'Tipo', 'Fecha')  # Definir columnas
            tree = ttk.Treeview(tree_frame, columns=columns, show='headings', height=10)  # Crear treeview
            
            # Dimensiones de columnas
            tree.column('Materia', width=120, anchor='w')  # Columna materia alineada izquierda
            tree.column('Código', width=80, anchor='center')  # Columna código centrada
            tree.column('Calificación', width=80, anchor='center')  # Columna calificación centrada
            tree.column('Tipo', width=80, anchor='center')  # Columna tipo centrada
            tree.column('Fecha', width=100, anchor='center')  # Columna fecha centrada
            
            # Encabezados
            for col in columns:  # Recorrer columnas
                tree.heading(col, text=col)  # Establecer heading
            
            # Scrollbars
            v_scrollbar = ttk.Scrollbar(tree_frame, orient=tk.VERTICAL, command=tree.yview)  # Scrollbar vertical
            tree.configure(yscrollcommand=v_scrollbar.set)  # Configurar scrollbar vertical
            
            h_scrollbar = ttk.Scrollbar(tree_frame, orient=tk.HORIZONTAL, command=tree.xview)  # Scrollbar horizontal
            tree.configure(xscrollcommand=h_scrollbar.set)  # Configurar scrollbar horizontal
            
            # Insertar datos con colores según calificación
            for cal in calificaciones:  # Recorrer calificaciones
                calificacion_val = cal[2]  # Obtener valor de calificación
                # Asignar etiqueta de color según rango
                if calificacion_val >= 90:  # Si 90 o más
                    tags = ('excelente',)  # Etiqueta excelente
                elif calificacion_val >= 80:  # Si 80-89
                    tags = ('bueno',)  # Etiqueta bueno
                elif calificacion_val >= 70:  # Si 70-79
                    tags = ('regular',)  # Etiqueta regular
                else:  # Menos de 70
                    tags = ('bajo',)  # Etiqueta bajo
                    
                tree.insert('', tk.END, values=cal[:5], tags=tags)  # Insertar fila con etiquetas
            
            # Configurar colores de fondo
            tree.tag_configure('excelente', background='#d4edda')  # Verde claro para excelente
            tree.tag_configure('bueno', background='#fff3cd')  # Amarillo claro para bueno
            tree.tag_configure('regular', background='#ffeaa7')  # Amarillo para regular
            tree.tag_configure('bajo', background='#f8d7da')  # Rojo claro para bajo
            
            # Layout de tabla
            tree.grid(row=0, column=0, sticky='nsew')  # Posicionar tabla en grid
            v_scrollbar.grid(row=0, column=1, sticky='ns')  # Posicionar scrollbar vertical
            h_scrollbar.grid(row=1, column=0, sticky='ew')  # Posicionar scrollbar horizontal
            
            # Configurar expansión
            tree_frame.grid_rowconfigure(0, weight=1)  # Fila 0 expandible
            tree_frame.grid_columnconfigure(0, weight=1)  # Columna 0 expandible
            
            # Contador de calificaciones
            total_label = tk.Label(historial_frame,  # Etiqueta para contador
                                 text=f"Total de calificaciones: {len(calificaciones)}",  # Texto con cantidad
                                 bg='#f8f9fa', fg='#7f8c8d', font=('Arial', 9))  # Estilo
            total_label.pack(anchor='w', pady=(5, 0))  # Empacar alineado izquierda
            
        else:  # Si no hay calificaciones
            # Mensaje cuando no hay datos
            no_data_frame = tk.Frame(historial_frame, bg='#f8f9fa', height=100)  # Frame para mensaje
            no_data_frame.pack(fill='both', expand=True, pady=20)  # Empacar expandible
            no_data_frame.pack_propagate(False)  # Evitar que cambie tamaño
            
            tk.Label(no_data_frame, text="No hay calificaciones registradas",  # Etiqueta principal
                    bg='#f8f9fa', fg='#7f8c8d', font=('Arial', 11)).pack(expand=True)  # Empacar centrada
            tk.Label(no_data_frame, text="Agrega calificaciones usando el formulario superior",  # Etiqueta secundaria
                    bg='#f8f9fa', fg='#95a5a6', font=('Arial', 9)).pack()  # Empacar centrada
    
    # ========== SECCIÓN DE TRÁMITES (VISTA PRINCIPAL) ==========
    
    def mostrar_seccion_tramites(self):
        """Muestra la sección principal de trámites universitarios"""
        self.limpiar_contenido()  # Limpiar contenido anterior

        # Título de la sección
        titulo = tk.Label(  # Crear etiqueta de título
            self.content_frame,  # Frame padre
            text="Trámites Universitarios",  # Texto del título
            bg='#f8f9fa', fg='#2c3e50',  # Colores
            font=('Arial', 14, 'bold')  # Fuente grande y negrita
        )
        titulo.pack(anchor='w', pady=(5, 15))  # Empacar alineado izquierda con espacio

        # Contenedor de trámites
        tram_frame = tk.Frame(self.content_frame, bg='#f8f9fa')  # Frame para trámites
        tram_frame.pack(fill='both', expand=True)  # Empacar expandible

        # Lista de trámites disponibles
        tramites = [  # Lista de tuplas (nombre, descripción, color)
            ("Constancia de Estudios", "Documento oficial que certifica tu estatus activo.", "#2E86AB"),
            ("Historial Académico", "Resumen detallado de tus materias y calificaciones.", "#A23B72"),
            ("Kardex", "Registro completo de tu avance académico.", "#F18F01"),
            ("Credencial Universitaria", "Trámite para solicitar o reponer credencial.", "#1B998B"),
            ("Servicio Social", "Información y estatus del servicio social.", "#9D4EDD"),
            ("Prácticas Profesionales", "Gestión y seguimiento de tus prácticas.", "#E71D36"),
            ("Baja Temporal", "Solicitud para suspender temporalmente tus estudios.", "#34495E")
        ]

        # Crear una tarjeta por cada trámite
        for nombre, desc, color in tramites:  # Recorrer trámites
            card = tk.Frame(tram_frame, bg='white', relief='raised', bd=1)  # Frame para card
            card.pack(fill='x', pady=5, padx=5)  # Empacar con espacio

            # Barra lateral de color
            left = tk.Frame(card, bg=color, width=8)  # Frame para barra de color
            left.pack(side='left', fill='y')  # Empacar a izquierda

            # Contenido de la tarjeta
            info = tk.Frame(card, bg='white')  # Frame para información
            info.pack(fill='both', expand=True, padx=10, pady=10)  # Empacar expandible

            # Nombre del trámite
            tk.Label(info, text=nombre, bg='white', fg='#2c3e50',  # Etiqueta nombre trámite
                    font=('Arial', 11, 'bold')).pack(anchor='w')  # Empacar alineado izquierda
            # Descripción
            tk.Label(info, text=desc, bg='white', fg='#7f8c8d',  # Etiqueta descripción
                    font=('Arial', 9), wraplength=250, justify='left').pack(anchor='w', pady=(2, 5))  # Empacar con espacio

            # Botón para ver detalles
            tk.Button(  # Botón "Ver trámite"
                info,  # Frame padre
                text="Ver trámite",  # Texto del botón
                bg=color,  # Color del botón
                fg='white',  # Texto blanco
                relief='flat',  # Sin relieve
                font=('Arial', 9, 'bold'),  # Fuente negrita
                padx=10,  # Padding horizontal
                pady=3,  # Padding vertical
                command=lambda t=nombre: self.mostrar_detalle_tramite(t)  # Función con parámetro
            ).pack(anchor='e', pady=5)  # Empacar alineado derecha con espacio

# =============================================
# PUNTO DE INICIO DE LA APLICACIÓN
# =============================================

if __name__ == "__main__":  # Si se ejecuta como script principal
    # Iniciar aplicación con pantalla de login
    root_login = tk.Tk()  # Crear ventana principal de Tkinter
    login_app = LoginSystem(root_login)  # Crear instancia del sistema de login
    root_login.mainloop()  # Iniciar loop principal de la aplicación
