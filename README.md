# SIIU
PROYECTO FINAL 
import tkinter as tk
from tkinter import ttk, messagebox
import sqlite3
from datetime import datetime
import re

# =============================================
# SISTEMA DE LOGIN CON CUALQUIER EMAIL
# =============================================

class LoginSystem:
    def __init__(self, root):
        self.root = root
        self.root.title("SIIU - Sistema Integrado de Información")
        self.root.geometry("400x500")
        self.root.configure(bg='white')
        self.root.resizable(False, False)
        
        # Centrar la ventana
        self.center_window()
        
        self.create_widgets()
    
    def center_window(self):
        """Centra la ventana en la pantalla"""
        self.root.update_idletasks()
        width = 400
        height = 500
        x = (self.root.winfo_screenwidth() // 2) - (width // 2)
        y = (self.root.winfo_screenheight() // 2) - (height // 2)
        self.root.geometry(f'{width}x{height}+{x}+{y}')
    
    def create_widgets(self):
        # Frame principal
        main_frame = tk.Frame(self.root, bg='white', padx=40, pady=40)
        main_frame.pack(expand=True, fill='both')
        
        # Logo/Header
        self.create_header(main_frame)
        
        # Formulario de login
        self.create_login_form(main_frame)
        
        # Footer
        self.create_footer(main_frame)
    
    def create_header(self, parent):
        header_frame = tk.Frame(parent, bg='white')
        header_frame.pack(pady=(0, 30))
        
        # Título SIIU
        title_label = tk.Label(
            header_frame,
            text="SIIU",
            font=('Arial', 24, 'bold'),
            fg='#2c3e50',
            bg='white'
        )
        title_label.pack()
        
        # Subtítulo
        subtitle_label = tk.Label(
            header_frame,
            text="Sistema Integrado de Información\nUniversidad Politécnica del Estado de Nayarit",
            font=('Arial', 10),
            fg='#7f8c8d',
            bg='white',
            justify='center'
        )
        subtitle_label.pack(pady=(5, 20))
    
    def create_login_form(self, parent):
        form_frame = tk.Frame(parent, bg='white')
        form_frame.pack(pady=20, fill='x')
        
        # Etiqueta Email
        email_label = tk.Label(
            form_frame,
            text="Correo Electrónico",
            font=('Arial', 10, 'bold'),
            fg='#2c3e50',
            bg='white',
            anchor='w'
        )
        email_label.pack(fill='x', pady=(0, 5))
        
        # Campo Email
        self.email_entry = tk.Entry(
            form_frame,
            font=('Arial', 12),
            relief='solid',
            bd=1,
            bg='#f8f9fa'
        )
        self.email_entry.pack(fill='x', pady=(0, 15), ipady=8)
        self.email_entry.insert(0, "tucorreo@ejemplo.com")
        self.email_entry.bind('<FocusIn>', self.clear_placeholder_email)
        
        # Etiqueta Contraseña
        password_label = tk.Label(
            form_frame,
            text="Contraseña",
            font=('Arial', 10, 'bold'),
            fg='#2c3e50',
            bg='white',
            anchor='w'
        )
        password_label.pack(fill='x', pady=(0, 5))
        
        # Campo Contraseña
        self.password_entry = tk.Entry(
            form_frame,
            font=('Arial', 12),
            show='•',
            relief='solid',
            bd=1,
            bg='#f8f9fa'
        )
        self.password_entry.pack(fill='x', pady=(0, 20), ipady=8)
        self.password_entry.insert(0, "Ingresa tu contraseña")
        self.password_entry.bind('<FocusIn>', self.clear_placeholder_password)
        
        # Botón Iniciar Sesión
        login_button = tk.Button(
            form_frame,
            text="Iniciar Sesión",
            font=('Arial', 12, 'bold'),
            bg='#3498db',
            fg='white',
            relief='flat',
            cursor='hand2',
            command=self.login,
            height=2
        )
        login_button.pack(fill='x', pady=(10, 0))
        
        # Enlace "Olvidé mi contraseña"
        forgot_link = tk.Label(
            form_frame,
            text="¿Olvidaste tu contraseña?",
            font=('Arial', 10),
            fg='#3498db',
            bg='white',
            cursor='hand2'
        )
        forgot_link.pack(pady=(15, 0))
        forgot_link.bind('<Button-1>', self.forgot_password)
        
        # Información de demo
        demo_label = tk.Label(
            form_frame,
            text="Demo: Usa cualquier email válido y cualquier contraseña",
            font=('Arial', 8),
            fg='#95a5a6',
            bg='white'
        )
        demo_label.pack(pady=(20, 0))
    
    def create_footer(self, parent):
        footer_frame = tk.Frame(parent, bg='white')
        footer_frame.pack(side='bottom', pady=20)
        
        copyright_label = tk.Label(
            footer_frame,
            text="© 2025 UPEN - Todos los derechos reservados",
            font=('Arial', 9),
            fg='#95a5a6',
            bg='white'
        )
        copyright_label.pack()
    
    def clear_placeholder_email(self, event):
        if self.email_entry.get() == "tucorreo@ejemplo.com":
            self.email_entry.delete(0, tk.END)
            self.email_entry.config(fg='black')
    
    def clear_placeholder_password(self, event):
        if self.password_entry.get() == "Ingresa tu contraseña":
            self.password_entry.delete(0, tk.END)
            self.password_entry.config(fg='black', show='•')
    
    def is_valid_email(self, email):
        """Valida el formato del email usando una expresión regular"""
        pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
        return re.match(pattern, email) is not None
    
    def login(self):
        email = self.email_entry.get().strip()
        password = self.password_entry.get()
        
        # Validaciones básicas
        if not email or email == "tucorreo@ejemplo.com":
            messagebox.showwarning("Advertencia", "Por favor ingresa tu correo electrónico")
            return
        
        if not self.is_valid_email(email):
            messagebox.showwarning("Advertencia", "Por favor ingresa un correo electrónico válido")
            return
        
        if not password or password == "Ingresa tu contraseña":
            messagebox.showwarning("Advertencia", "Por favor ingresa tu contraseña")
            return
        
        # ACEPTA CUALQUIER EMAIL VÁLIDO Y CUALQUIER CONTRASEÑA NO VACÍA
        self.open_siu_app(email)
    
    def open_siu_app(self, user_email):
        """Cierra la ventana de login y abre la aplicación principal"""
        self.root.destroy()  # Cierra la ventana de login
        
        # Crear nueva ventana para la aplicación principal
        root_siu = tk.Tk()
        app = SIUApp(root_siu, user_email)
        root_siu.mainloop()
    
    def forgot_password(self, event):
        email = self.email_entry.get()
        if email and email != "tucorreo@ejemplo.com" and self.is_valid_email(email):
            messagebox.showinfo("Recuperar Contraseña", 
                              f"Se ha enviado un enlace de recuperación a: {email}")
        else:
            messagebox.showinfo("Recuperar Contraseña", 
                              "Por favor ingresa tu correo electrónico para recuperar tu contraseña")

# =============================================
# SISTEMA PRINCIPAL SIU (CON MEJORAS)
# =============================================

class Database:
    def __init__(self):
        self.conn = sqlite3.connect('siu_app.db')
        self.create_tables()
    
    def create_tables(self):
        cursor = self.conn.cursor()
        
        # Tabla de personas (estudiantes)
        cursor.execute('''
            CREATE TABLE IF NOT EXISTS personas (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                nombre TEXT NOT NULL,
                apellido TEXT NOT NULL,
                email TEXT UNIQUE,
                telefono TEXT,
                fecha_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP
            )
        ''')
        
        # Tabla de materias
        cursor.execute('''
            CREATE TABLE IF NOT EXISTS materias (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                nombre TEXT NOT NULL,
                codigo TEXT UNIQUE,
                creditos INTEGER DEFAULT 3,
                profesor TEXT,
                color TEXT DEFAULT '#2E86AB'
            )
        ''')
        
        # Tabla de calificaciones
        cursor.execute('''
            CREATE TABLE IF NOT EXISTS calificaciones (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                persona_id INTEGER,
                materia_id INTEGER,
                calificacion REAL NOT NULL,
                fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                tipo_evaluacion TEXT DEFAULT 'Parcial',
                FOREIGN KEY (persona_id) REFERENCES personas (id),
                FOREIGN KEY (materia_id) REFERENCES materias (id)
            )
        ''')
        
        self.conn.commit()
        self.insert_default_data()
    
    def insert_default_data(self):
        cursor = self.conn.cursor()
        
        # Insertar materias por defecto si no existen
        materias_default = [
            ('Matemáticas', 'MAT101', 4, 'Dr. García', '#2E86AB'),
            ('Programación', 'PROG102', 5, 'Ing. Martínez', '#A23B72'),
            ('Base de Datos', 'BD103', 4, 'Lic. Rodríguez', '#F18F01'),
            ('Redes', 'RED104', 3, 'Mtro. López', '#1B998B'),
            ('Inglés', 'ING105', 2, 'Mrs. Smith', '#9D4EDD'),
            ('Física', 'FIS106', 4, 'Dr. Hernández', '#E71D36'),
            ('Química', 'QUI107', 4, 'Dra. González', '#34495E'),
            ('Historia', 'HIS108', 3, 'Mtro. Pérez', '#1ABC9C')
        ]
        
        cursor.executemany('''
            INSERT OR IGNORE INTO materias (nombre, codigo, creditos, profesor, color)
            VALUES (?, ?, ?, ?, ?)
        ''', materias_default)
        
        self.conn.commit()
    
    # Métodos para Personas
    def agregar_persona(self, nombre, apellido, email, telefono):
        cursor = self.conn.cursor()
        try:
            cursor.execute('''
                INSERT INTO personas (nombre, apellido, email, telefono)
                VALUES (?, ?, ?, ?)
            ''', (nombre, apellido, email, telefono))
            self.conn.commit()
            return True
        except sqlite3.IntegrityError:
            return False
    
    def obtener_personas(self):
        cursor = self.conn.cursor()
        cursor.execute('SELECT * FROM personas ORDER BY apellido, nombre')
        return cursor.fetchall()
    
    def buscar_persona(self, texto):
        cursor = self.conn.cursor()
        cursor.execute('''
            SELECT * FROM personas 
            WHERE nombre LIKE ? OR apellido LIKE ? OR email LIKE ?
        ''', (f'%{texto}%', f'%{texto}%', f'%{texto}%'))
        return cursor.fetchall()
    
    # Métodos para Calificaciones
    def agregar_calificacion(self, persona_id, materia_id, calificacion, tipo_evaluacion):
        cursor = self.conn.cursor()
        cursor.execute('''
            INSERT INTO calificaciones (persona_id, materia_id, calificacion, tipo_evaluacion)
            VALUES (?, ?, ?, ?)
        ''', (persona_id, materia_id, calificacion, tipo_evaluacion))
        self.conn.commit()
    
    def obtener_calificaciones_persona(self, persona_id):
        cursor = self.conn.cursor()
        cursor.execute('''
            SELECT m.nombre, m.codigo, c.calificacion, c.tipo_evaluacion, c.fecha, m.color
            FROM calificaciones c
            JOIN materias m ON c.materia_id = m.id
            WHERE c.persona_id = ?
            ORDER BY c.fecha DESC
        ''', (persona_id,))
        return cursor.fetchall()
    
    def obtener_promedio_persona(self, persona_id):
        cursor = self.conn.cursor()
        cursor.execute('''
            SELECT AVG(calificacion) 
            FROM calificaciones 
            WHERE persona_id = ?
        ''', (persona_id,))
        return cursor.fetchone()[0] or 0
    
    # Métodos para Materias
    def obtener_materias(self):
        cursor = self.conn.cursor()
        cursor.execute('SELECT * FROM materias ORDER BY nombre')
        return cursor.fetchall()
    
    def obtener_materias_con_promedio(self, persona_id):
        cursor = self.conn.cursor()
        cursor.execute('''
            SELECT m.id, m.nombre, m.codigo, m.color, 
                   AVG(c.calificacion) as promedio,
                   COUNT(c.calificacion) as evaluaciones
            FROM materias m
            LEFT JOIN calificaciones c ON m.id = c.materia_id AND c.persona_id = ?
            GROUP BY m.id, m.nombre, m.codigo, m.color
        ''', (persona_id,))
        return cursor.fetchall()

class SIUApp:
    def __init__(self, root, user_email):
        self.root = root
        self.user_email = user_email
        self.root.title("SIIU - Sistema Integral Universitario")
        self.root.geometry("400x700")
        self.root.configure(bg='#f8f9fa')
        
        self.db = Database()
        
        # Colores
        self.primary_color = '#2E86AB'
        self.secondary_color = '#A23B72'
        self.accent_color = '#F18F01'
        self.success_color = '#1B998B'
        self.danger_color = '#E71D36'
        
        self.create_ui()
        self.mostrar_seccion_principal()
    
    def create_ui(self):
        # Header
        header_frame = tk.Frame(self.root, bg=self.primary_color, height=80)
        header_frame.pack(fill='x', padx=10, pady=(10, 5))
        header_frame.pack_propagate(False)
        
        # Información del usuario
        user_info_frame = tk.Frame(header_frame, bg=self.primary_color)
        user_info_frame.pack(side='left', padx=20, pady=10)
        
        title_label = tk.Label(
            user_info_frame,
            text="SIIU Mobile",
            bg=self.primary_color,
            fg='white',
            font=('Arial', 16, 'bold')
        )
        title_label.pack(anchor='w')
        
        user_label = tk.Label(
            user_info_frame,
            text=f"Usuario: {self.user_email}",
            bg=self.primary_color,
            fg='white',
            font=('Arial', 9)
        )
        user_label.pack(anchor='w')
        
        # Botón de cerrar sesión
        logout_btn = tk.Button(
            header_frame,
            text="Cerrar Sesión",
            bg='white',
            fg=self.primary_color,
            font=('Arial', 9, 'bold'),
            relief='flat',
            command=self.cerrar_sesion
        )
        logout_btn.pack(side='right', padx=20, pady=20)
        
        # Navegación principal
        nav_frame = tk.Frame(self.root, bg='#e9ecef', height=50)
        nav_frame.pack(fill='x', padx=10, pady=5)
        nav_frame.pack_propagate(False)
        
        btn_principal = tk.Button(
            nav_frame,
            text="Principal",
            bg='white',
            fg=self.primary_color,
            font=('Arial', 10, 'bold'),
            relief='flat',
            command=self.mostrar_seccion_principal
        )
        btn_principal.pack(side='left', expand=True, fill='both', padx=2)
        
        btn_personas = tk.Button(
            nav_frame,
            text="Personas",
            bg='white',
            fg=self.primary_color,
            font=('Arial', 10),
            relief='flat',
            command=self.mostrar_seccion_personas
        )
        btn_personas.pack(side='left', expand=True, fill='both', padx=2)
        
        btn_calificaciones = tk.Button(
            nav_frame,
            text="Calificaciones",
            bg='white',
            fg=self.primary_color,
            font=('Arial', 10),
            relief='flat',
            command=self.mostrar_seccion_calificaciones
        )
        btn_calificaciones.pack(side='left', expand=True, fill='both', padx=2)
        
        btn_tramites = tk.Button(
            nav_frame,
            text="Trámites",
            bg='white',
            fg=self.primary_color,
            font=('Arial', 10),
            relief='flat',
            command=self.mostrar_seccion_tramites
        )
        btn_tramites.pack(side='left', expand=True, fill='both', padx=2)

        # Contenido principal CON SCROLLBAR
        self.main_scroll_frame = tk.Frame(self.root, bg='#f8f9fa')
        self.main_scroll_frame.pack(fill='both', expand=True, padx=10, pady=5)
        
        # Canvas y scrollbar principal
        self.canvas = tk.Canvas(self.main_scroll_frame, bg='#f8f9fa', highlightthickness=0)
        self.v_scrollbar = ttk.Scrollbar(self.main_scroll_frame, orient=tk.VERTICAL, command=self.canvas.yview)
        self.h_scrollbar = ttk.Scrollbar(self.main_scroll_frame, orient=tk.HORIZONTAL, command=self.canvas.xview)
        
        self.content_frame = tk.Frame(self.canvas, bg='#f8f9fa')
        
        self.content_frame.bind(
            "<Configure>",
            lambda e: self.canvas.configure(scrollregion=self.canvas.bbox("all"))
        )
        
        self.canvas.create_window((0, 0), window=self.content_frame, anchor="nw")
        self.canvas.configure(yscrollcommand=self.v_scrollbar.set, xscrollcommand=self.h_scrollbar.set)
        
        # Empaquetar canvas y scrollbars
        self.canvas.grid(row=0, column=0, sticky='nsew')
        self.v_scrollbar.grid(row=0, column=1, sticky='ns')
        self.h_scrollbar.grid(row=1, column=0, sticky='ew')
        
        self.main_scroll_frame.grid_rowconfigure(0, weight=1)
        self.main_scroll_frame.grid_columnconfigure(0, weight=1)
    
    def cerrar_sesion(self):
        """Cierra la sesión y vuelve al login"""
        if messagebox.askyesno("Cerrar Sesión", "¿Estás seguro de que quieres cerrar sesión?"):
            self.root.destroy()
            # Volver a abrir el login
            root_login = tk.Tk()
            login_app = LoginSystem(root_login)
            root_login.mainloop()
    
    def limpiar_contenido(self):
        for widget in self.content_frame.winfo_children():
            widget.destroy()
    
    def mostrar_detalle_tramite(self, nombre_tramite):
        """Muestra una ventana con los pasos del trámite seleccionado"""
        # Crear ventana emergente
        detalle_window = tk.Toplevel(self.root)
        detalle_window.title(f"Trámite: {nombre_tramite}")
        detalle_window.geometry("500x600")
        detalle_window.configure(bg='white')
        detalle_window.resizable(True, True)
        
        # Centrar ventana
        detalle_window.transient(self.root)
        detalle_window.grab_set()
        
        # Frame principal
        main_frame = tk.Frame(detalle_window, bg='white', padx=20, pady=20)
        main_frame.pack(fill='both', expand=True)
        
        # Título
        titulo_frame = tk.Frame(main_frame, bg='white')
        titulo_frame.pack(fill='x', pady=(0, 20))
        
        tk.Label(
            titulo_frame,
            text=nombre_tramite,
            bg='white',
            fg='#2c3e50',
            font=('Arial', 18, 'bold')
        ).pack(anchor='w')
        
        # Información del trámite según el tipo
        info_frame = tk.Frame(main_frame, bg='white')
        info_frame.pack(fill='both', expand=True)
        
        # Canvas y scrollbar para los pasos
        canvas = tk.Canvas(info_frame, bg='white', highlightthickness=0)
        scrollbar = ttk.Scrollbar(info_frame, orient=tk.VERTICAL, command=canvas.yview)
        scrollable_frame = tk.Frame(canvas, bg='white')
        
        scrollable_frame.bind(
            "<Configure>",
            lambda e: canvas.configure(scrollregion=canvas.bbox("all"))
        )
        
        canvas.create_window((0, 0), window=scrollable_frame, anchor="nw")
        canvas.configure(yscrollcommand=scrollbar.set)
        
        # Obtener información del trámite
        pasos, requisitos, tiempo, costo = self.obtener_info_tramite(nombre_tramite)
        
        # Descripción
        desc_frame = tk.Frame(scrollable_frame, bg='white')
        desc_frame.pack(fill='x', pady=(0, 20))
        
        tk.Label(
            desc_frame,
            text="Descripción:",
            bg='white',
            fg='#2c3e50',
            font=('Arial', 12, 'bold')
        ).pack(anchor='w')
        
        desc_text = self.obtener_descripcion_tramite(nombre_tramite)
        tk.Label(
            desc_frame,
            text=desc_text,
            bg='white',
            fg='#7f8c8d',
            font=('Arial', 10),
            wraplength=450,
            justify='left'
        ).pack(anchor='w', pady=(5, 0))
        
        # Información general
        info_general_frame = tk.Frame(scrollable_frame, bg='#f8f9fa', relief='raised', bd=1)
        info_general_frame.pack(fill='x', pady=(0, 20))
        
        tk.Label(
            info_general_frame,
            text="Información General",
            bg='#f8f9fa',
            fg='#2c3e50',
            font=('Arial', 12, 'bold')
        ).pack(anchor='w', padx=10, pady=10)
        
        # Tiempo de procesamiento
        tiempo_frame = tk.Frame(info_general_frame, bg='#f8f9fa')
        tiempo_frame.pack(fill='x', padx=10, pady=(0, 5))
        
        tk.Label(
            tiempo_frame,
            text=f"Tiempo de procesamiento: {tiempo}",
            bg='#f8f9fa',
            fg='#2c3e50',
            font=('Arial', 10)
        ).pack(anchor='w')
        
        # Costo
        costo_frame = tk.Frame(info_general_frame, bg='#f8f9fa')
        costo_frame.pack(fill='x', padx=10, pady=(0, 10))
        
        tk.Label(
            costo_frame,
            text=f"Costo: {costo}",
            bg='#f8f9fa',
            fg='#2c3e50',
            font=('Arial', 10)
        ).pack(anchor='w')
        
        # Requisitos
        requisitos_frame = tk.Frame(scrollable_frame, bg='white')
        requisitos_frame.pack(fill='x', pady=(0, 20))
        
        tk.Label(
            requisitos_frame,
            text="Requisitos:",
            bg='white',
            fg='#2c3e50',
            font=('Arial', 12, 'bold')
        ).pack(anchor='w')
        
        for i, requisito in enumerate(requisitos, 1):
            req_frame = tk.Frame(requisitos_frame, bg='white')
            req_frame.pack(fill='x', pady=2)
            
            tk.Label(
                req_frame,
                text=f"• {requisito}",
                bg='white',
                fg='#7f8c8d',
                font=('Arial', 10),
                wraplength=450,
                justify='left'
            ).pack(anchor='w')
        
        # Pasos del trámite
        pasos_frame = tk.Frame(scrollable_frame, bg='white')
        pasos_frame.pack(fill='x', pady=(0, 20))
        
        tk.Label(
            pasos_frame,
            text="Pasos a seguir:",
            bg='white',
            fg='#2c3e50',
            font=('Arial', 12, 'bold')
        ).pack(anchor='w')
        
        for i, paso in enumerate(pasos, 1):
            paso_frame = tk.Frame(pasos_frame, bg='white', relief='raised', bd=1)
            paso_frame.pack(fill='x', pady=2)
            
            # Número del paso
            num_frame = tk.Frame(paso_frame, bg=self.primary_color, width=30)
            num_frame.pack(side='left', fill='y')
            num_frame.pack_propagate(False)
            
            tk.Label(
                num_frame,
                text=str(i),
                bg=self.primary_color,
                fg='white',
                font=('Arial', 12, 'bold')
            ).pack(expand=True)
            
            # Descripción del paso
            desc_paso_frame = tk.Frame(paso_frame, bg='white')
            desc_paso_frame.pack(fill='both', expand=True, padx=10, pady=10)
            
            tk.Label(
                desc_paso_frame,
                text=paso,
                bg='white',
                fg='#2c3e50',
                font=('Arial', 10),
                wraplength=400,
                justify='left'
            ).pack(anchor='w')
        
        # Botón de solicitar
        boton_frame = tk.Frame(scrollable_frame, bg='white')
        boton_frame.pack(fill='x', pady=20)
        
        tk.Button(
            boton_frame,
            text="Solicitar este trámite",
            bg=self.success_color,
            fg='white',
            font=('Arial', 12, 'bold'),
            relief='flat',
            padx=20,
            pady=10,
            command=lambda: self.solicitar_tramite(nombre_tramite, detalle_window)
        ).pack()
        
        # Empaquetar canvas y scrollbar
        canvas.pack(side="left", fill="both", expand=True)
        scrollbar.pack(side="right", fill="y")
    
    def obtener_descripcion_tramite(self, nombre_tramite):
        """Retorna la descripción detallada del trámite"""
        descripciones = {
            "Constancia de Estudios": "Documento oficial que certifica que el estudiante se encuentra inscrito y activo en la universidad. Es requerido para diversos trámites bancarios, becas, y procesos administrativos.",
            "Historial Académico": "Documento que contiene el registro completo de todas las materias cursadas, calificaciones obtenidas, promedios y situación académica del estudiante.",
            "Kardex": "Registro oficial que detalla el avance académico del estudiante, incluyendo materias aprobadas, reprobadas y en curso, con sus respectivas calificaciones.",
            "Credencial Universitaria": "Identificación oficial de la universidad que acredita la condición de estudiante. Incluye fotografía, datos personales y periodo de vigencia.",
            "Servicio Social": "Proceso para registrar, dar seguimiento y concluir el servicio social obligatorio requerido para la titulación.",
            "Prácticas Profesionales": "Sistema de gestión para el registro, supervisión y evaluación de las prácticas profesionales realizadas en empresas e instituciones.",
            "Baja Temporal": "Procedimiento para solicitar la suspensión temporal de estudios por uno o dos semestres por motivos académicos, personales o de salud."
        }
        return descripciones.get(nombre_tramite, "Descripción no disponible.")
    
    def obtener_info_tramite(self, nombre_tramite):
        """Retorna la información específica de cada trámite"""
        info_tramites = {
            "Constancia de Estudios": (
                [
                    "Iniciar sesión en el sistema SIIU",
                    "Seleccionar la opción 'Trámites'",
                    "Elegir 'Constancia de Estudios'",
                    "Verificar que no existan adeudos en la cuenta",
                    "Confirmar la solicitud",
                    "Descargar el documento en formato PDF"
                ],
                [
                    "Estar inscrito en el semestre actual",
                    "No tener adeudos con la institución",
                    "Tener al menos el 80% de asistencia",
                    "Estar al corriente en pagos administrativos"
                ],
                "24-48 horas hábiles",
                "Gratuito"
            ),
            "Historial Académico": (
                [
                    "Acceder al portal SIIU con credenciales",
                    "Navegar a la sección de Trámites",
                    "Seleccionar 'Historial Académico'",
                    "Verificar información personal",
                    "Seleccionar formato (PDF o físico)",
                    "Confirmar solicitud y descargar"
                ],
                [
                    "Haber cursado al menos un semestre",
                    "No tener materias con calificación pendiente",
                    "Cuenta estudiantil activa",
                    "Identificación oficial vigente"
                ],
                "48-72 horas hábiles",
                "$50.00 MXN (formato físico)"
            ),
            "Kardex": (
                [
                    "Ingresar al sistema SIIU",
                    "Dirigirse a Trámites Académicos",
                    "Seleccionar 'Generar Kardex'",
                    "Revisar información mostrada",
                    "Solicitar versión digital o impresa",
                    "Recibir comprobante de solicitud"
                ],
                [
                    "Estatus académico activo",
                    "Semestre mínimo cursado: 2do",
                    "Sin sanciones disciplinarias vigentes",
                    "Fotografía tamaño infantil actualizada"
                ],
                "3-5 días hábiles",
                "$30.00 MXN (digital), $80.00 MXN (físico)"
            ),
            "Credencial Universitaria": (
                [
                    "Solicitar cita en control escolar",
                    "Presentar documentación requerida",
                    "Tomar fotografía para credencial",
                    "Realizar pago correspondiente",
                    "Esperar proceso de fabricación",
                    "Recoger credencial en ventanilla"
                ],
                [
                    "Comprobante de inscripción actual",
                    "Identificación oficial vigente",
                    "Comprobante de domicilio",
                    "2 fotografías tamaño infantil",
                    "Comprobante de pago"
                ],
                "10-15 días hábiles",
                "$150.00 MXN (primera vez), $200.00 MXN (reposición)"
            ),
            "Servicio Social": (
                [
                    "Completar el 70% de créditos",
                    "Registrar proyecto en departamento",
                    "Obtener carta de aceptación",
                    "Realizar horas requeridas",
                    "Presentar reportes mensuales",
                    "Obtener carta de terminación"
                ],
                [
                    "70% de créditos aprobados",
                    "Promedio mínimo de 8.0",
                    "Proyecto autorizado",
                    "Horario compatible con estudios"
                ],
                "Todo el semestre",
                "Gratuito"
            ),
            "Prácticas Profesionales": (
                [
                    "Completar el 60% de créditos",
                    "Buscar empresa afín a la carrera",
                    "Firmar convenio con la empresa",
                    "Registrar práctica en el sistema",
                    "Realizar horas establecidas",
                    "Presentar reporte final"
                ],
                [
                    "60% de créditos aprobados",
                    "Promedio mínimo de 8.5",
                    "Convenio con empresa",
                    "Seguro de prácticas profesionales"
                ],
                "1-2 semestres",
                "Seguro: $500.00 MXN"
            ),
            "Baja Temporal": (
                [
                    "Solicitar cita con tutor académico",
                    "Llenar formato de solicitud",
                    "Presentar documentación comprobatoria",
                    "Obtener autorización del departamento",
                    "Realizar pago administrativo",
                    "Recibir constancia de baja temporal"
                ],
                [
                    "Motivo justificado (salud, económico, personal)",
                    "Estar al corriente en pagos",
                    "No tener sanciones disciplinarias",
                    "Tutor académico que avale la solicitud"
                ],
                "5-7 días hábiles",
                "$300.00 MXN"
            )
        }
        return info_tramites.get(nombre_tramite, ([], [], "No especificado", "No especificado"))
    
    def solicitar_tramite(self, nombre_tramite, ventana):
        """Procesa la solicitud del trámite"""
        respuesta = messagebox.askyesno(
            "Solicitar Trámite",
            f"¿Confirmas que deseas solicitar el trámite:\n\n'{nombre_tramite}'?\n\n"
            f"Recibirás un correo de confirmación en:\n{self.user_email}"
        )
        
        if respuesta:
            messagebox.showinfo(
                "Solicitud Enviada",
                f"✅ Tu solicitud para '{nombre_tramite}' ha sido registrada.\n\n"
                f"📧 Se ha enviado un comprobante a: {self.user_email}\n"
                f"⏰ Tiempo estimado de procesamiento: 24-48 horas\n"
                f"📋 Puedes dar seguimiento en la sección de Trámites"
            )
            ventana.destroy()
    
    def mostrar_seccion_principal(self):
        self.limpiar_contenido()
        
        # Bienvenida personalizada
        welcome_frame = tk.Frame(self.content_frame, bg='white', relief='raised', bd=1)
        welcome_frame.pack(fill='x', pady=(0, 10))
        
        # Extraer nombre de usuario del email
        username = self.user_email.split('@')[0].capitalize()
        
        tk.Label(welcome_frame, text=f"¡Bienvenido, {username}!", bg='white', 
                fg='#2c3e50', font=('Arial', 14, 'bold')).pack(anchor='w', padx=10, pady=5)
        tk.Label(welcome_frame, text=f"Correo: {self.user_email}", bg='white', 
                fg='#7f8c8d', font=('Arial', 10)).pack(anchor='w', padx=10, pady=(0, 10))
        
        # Estadísticas rápidas
        stats_frame = tk.Frame(self.content_frame, bg='white', relief='raised', bd=1)
        stats_frame.pack(fill='x', pady=(0, 10))
        
        personas = self.db.obtener_personas()
        materias = self.db.obtener_materias()
        
        tk.Label(stats_frame, text="Resumen General", bg='white', 
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', padx=10, pady=5)
        
        info_text = f"• {len(personas)} Estudiantes registrados\n• {len(materias)} Materias disponibles\n• Sistema actualizado"
        tk.Label(stats_frame, text=info_text, bg='white', fg='#7f8c8d',
                font=('Arial', 10), justify='left').pack(anchor='w', padx=10, pady=(0, 10))
        
        # Últimas personas registradas
        if personas:
            tk.Label(self.content_frame, text="Últimos Estudiantes", bg='#f8f9fa',
                    fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', pady=(10, 5))
            
            for persona in personas[-5:]:  # Últimas 5 personas para forzar scroll
                persona_frame = tk.Frame(self.content_frame, bg='white', relief='raised', bd=1)
                persona_frame.pack(fill='x', pady=2)
                
                nombre_completo = f"{persona[1]} {persona[2]}"
                tk.Label(persona_frame, text=nombre_completo, bg='white',
                        fg='#2c3e50', font=('Arial', 10, 'bold')).pack(anchor='w', padx=10, pady=5)
                tk.Label(persona_frame, text=f"Email: {persona[3]}", bg='white',
                        fg='#7f8c8d', font=('Arial', 8)).pack(anchor='w', padx=10, pady=(0, 5))
    
    def mostrar_seccion_personas(self):
        self.limpiar_contenido()
        
        # Formulario para agregar persona
        form_frame = tk.Frame(self.content_frame, bg='white', relief='raised', bd=1)
        form_frame.pack(fill='x', pady=(0, 10))
        
        tk.Label(form_frame, text="Agregar Nueva Persona", bg='white',
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', padx=10, pady=5)
        
        # Campos del formulario
        fields_frame = tk.Frame(form_frame, bg='white')
        fields_frame.pack(fill='x', padx=10, pady=5)
        
        tk.Label(fields_frame, text="Nombre:", bg='white').grid(row=0, column=0, sticky='w', pady=2)
        entry_nombre = tk.Entry(fields_frame, width=20)
        entry_nombre.grid(row=0, column=1, pady=2, padx=5)
        
        tk.Label(fields_frame, text="Apellido:", bg='white').grid(row=1, column=0, sticky='w', pady=2)
        entry_apellido = tk.Entry(fields_frame, width=20)
        entry_apellido.grid(row=1, column=1, pady=2, padx=5)
        
        tk.Label(fields_frame, text="Email:", bg='white').grid(row=2, column=0, sticky='w', pady=2)
        entry_email = tk.Entry(fields_frame, width=20)
        entry_email.grid(row=2, column=1, pady=2, padx=5)
        
        tk.Label(fields_frame, text="Teléfono:", bg='white').grid(row=3, column=0, sticky='w', pady=2)
        entry_telefono = tk.Entry(fields_frame, width=20)
        entry_telefono.grid(row=3, column=1, pady=2, padx=5)
        
        def guardar_persona():
            if entry_nombre.get() and entry_apellido.get():
                if self.db.agregar_persona(
                    entry_nombre.get(),
                    entry_apellido.get(),
                    entry_email.get(),
                    entry_telefono.get()
                ):
                    messagebox.showinfo("Éxito", "Persona agregada correctamente")
                    entry_nombre.delete(0, tk.END)
                    entry_apellido.delete(0, tk.END)
                    entry_email.delete(0, tk.END)
                    entry_telefono.delete(0, tk.END)
                    self.mostrar_lista_personas()
                else:
                    messagebox.showerror("Error", "El email ya existe")
            else:
                messagebox.showwarning("Advertencia", "Nombre y apellido son obligatorios")
        
        tk.Button(form_frame, text="Guardar Persona", bg=self.success_color,
                 fg='white', command=guardar_persona).pack(pady=10)
        
        # Lista de personas
        self.mostrar_lista_personas()
    
    def mostrar_lista_personas(self):
        # Frame para la lista
        lista_frame = tk.Frame(self.content_frame, bg='#f8f9fa')
        lista_frame.pack(fill='both', expand=True)
        
        tk.Label(lista_frame, text="Lista de Estudiantes", bg='#f8f9fa',
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', pady=(10, 5))
        
        # Frame para treeview y scrollbar
        tree_frame = tk.Frame(lista_frame, bg='#f8f9fa', height=300)
        tree_frame.pack(fill='both', expand=True)
        tree_frame.pack_propagate(False)
        
        # Treeview para mostrar personas
        columns = ('ID', 'Nombre', 'Apellido', 'Email', 'Teléfono')
        tree = ttk.Treeview(tree_frame, columns=columns, show='headings', height=12)
        
        for col in columns:
            tree.heading(col, text=col)
        
        tree.column('ID', width=50, anchor='center')
        tree.column('Nombre', width=100, anchor='w')
        tree.column('Apellido', width=100, anchor='w')
        tree.column('Email', width=150, anchor='w')
        tree.column('Teléfono', width=100, anchor='center')
        
        # Scrollbars
        v_scrollbar = ttk.Scrollbar(tree_frame, orient=tk.VERTICAL, command=tree.yview)
        h_scrollbar = ttk.Scrollbar(tree_frame, orient=tk.HORIZONTAL, command=tree.xview)
        tree.configure(yscrollcommand=v_scrollbar.set, xscrollcommand=h_scrollbar.set)
        
        # Grid layout para treeview y scrollbars
        tree.grid(row=0, column=0, sticky='nsew')
        v_scrollbar.grid(row=0, column=1, sticky='ns')
        h_scrollbar.grid(row=1, column=0, sticky='ew')
        
        # Configurar grid weights
        tree_frame.grid_rowconfigure(0, weight=1)
        tree_frame.grid_columnconfigure(0, weight=1)
        
        # Insertar datos
        personas = self.db.obtener_personas()
        for persona in personas:
            tree.insert('', tk.END, values=persona)
    
    def mostrar_seccion_calificaciones(self):
        self.limpiar_contenido()
        
        # Selección de persona
        seleccion_frame = tk.Frame(self.content_frame, bg='white', relief='raised', bd=1)
        seleccion_frame.pack(fill='x', pady=(0, 10))
        
        tk.Label(seleccion_frame, text="Seleccionar Estudiante", bg='white',
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', padx=10, pady=5)
        
        personas = self.db.obtener_personas()
        persona_var = tk.StringVar()
        
        if personas:
            nombres_personas = [f"{p[0]} - {p[1]} {p[2]}" for p in personas]
            combo_personas = ttk.Combobox(seleccion_frame, textvariable=persona_var, 
                                         values=nombres_personas, state='readonly')
            combo_personas.pack(fill='x', padx=10, pady=5)
            combo_personas.bind('<<ComboboxSelected>>', 
                               lambda e: self.mostrar_calificaciones_persona(personas[combo_personas.current()][0]))
        else:
            tk.Label(seleccion_frame, text="No hay personas registradas", 
                    bg='white', fg='#7f8c8d').pack(pady=10)
    
    def mostrar_calificaciones_persona(self, persona_id):
        # Limpiar contenido anterior (excepto el frame de selección)
        for widget in self.content_frame.winfo_children()[1:]:
            widget.destroy()
        
        # Formulario para agregar calificación
        form_frame = tk.Frame(self.content_frame, bg='white', relief='raised', bd=1)
        form_frame.pack(fill='x', pady=(0, 10))
        
        tk.Label(form_frame, text="Agregar Calificación", bg='white',
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', padx=10, pady=5)
        
        fields_frame = tk.Frame(form_frame, bg='white')
        fields_frame.pack(fill='x', padx=10, pady=5)
        
        tk.Label(fields_frame, text="Materia:", bg='white').grid(row=0, column=0, sticky='w', pady=2)
        materias = self.db.obtener_materias()
        materia_var = tk.StringVar()
        combo_materias = ttk.Combobox(fields_frame, textvariable=materia_var, 
                                     values=[f"{m[1]} - {m[0]}" for m in materias], 
                                     state='readonly')
        combo_materias.grid(row=0, column=1, pady=2, padx=5)
        
        tk.Label(fields_frame, text="Calificación:", bg='white').grid(row=1, column=0, sticky='w', pady=2)
        entry_calificacion = tk.Entry(fields_frame, width=10)
        entry_calificacion.grid(row=1, column=1, pady=2, padx=5)
        
        tk.Label(fields_frame, text="Tipo:", bg='white').grid(row=2, column=0, sticky='w', pady=2)
        tipo_var = tk.StringVar(value='Parcial')
        combo_tipo = ttk.Combobox(fields_frame, textvariable=tipo_var,
                                 values=['Parcial', 'Final', 'Proyecto', 'Tarea'], 
                                 state='readonly')
        combo_tipo.grid(row=2, column=1, pady=2, padx=5)
        
        def guardar_calificacion():
            if materia_var.get() and entry_calificacion.get():
                try:
                    calificacion = float(entry_calificacion.get())
                    if 0 <= calificacion <= 100:
                        materia_id = materias[combo_materias.current()][0]
                        self.db.agregar_calificacion(
                            persona_id, materia_id, calificacion, tipo_var.get()
                        )
                        messagebox.showinfo("Éxito", "Calificación agregada correctamente")
                        entry_calificacion.delete(0, tk.END)
                        self.mostrar_cards_materias(persona_id)
                        self.mostrar_historial_calificaciones(persona_id)
                    else:
                        messagebox.showerror("Error", "La calificación debe estar entre 0 y 100")
                except ValueError:
                    messagebox.showerror("Error", "La calificación debe ser un número")
            else:
                messagebox.showwarning("Advertencia", "Todos los campos son obligatorios")
        
        tk.Button(form_frame, text="Guardar Calificación", bg=self.success_color,
                 fg='white', command=guardar_calificacion).pack(pady=10)
        
        # Cards de materias con promedios
        self.mostrar_cards_materias(persona_id)
        
        # Historial de calificaciones
        self.mostrar_historial_calificaciones(persona_id)
    
    def mostrar_cards_materias(self, persona_id):
        cards_container = tk.Frame(self.content_frame, bg='#f8f9fa')
        cards_container.pack(fill='x', pady=(0, 10))
        
        tk.Label(cards_container, text="Materias y Promedios", bg='#f8f9fa',
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', pady=(10, 5))
        
        # Frame scrollable para las cards
        scroll_container = tk.Frame(cards_container, bg='#f8f9fa', height=200)
        scroll_container.pack(fill='both', expand=True)
        scroll_container.pack_propagate(False)
        
        # Canvas y scrollbar
        canvas = tk.Canvas(scroll_container, bg='#f8f9fa', highlightthickness=0)
        scrollbar = ttk.Scrollbar(scroll_container, orient=tk.VERTICAL, command=canvas.yview)
        scrollable_frame = tk.Frame(canvas, bg='#f8f9fa')
        
        scrollable_frame.bind(
            "<Configure>",
            lambda e: canvas.configure(scrollregion=canvas.bbox("all"))
        )
        
        canvas.create_window((0, 0), window=scrollable_frame, anchor="nw")
        canvas.configure(yscrollcommand=scrollbar.set)
        
        # Pack canvas y scrollbar
        canvas.pack(side="left", fill="both", expand=True)
        scrollbar.pack(side="right", fill="y")
        
        materias_con_promedio = self.db.obtener_materias_con_promedio(persona_id)
        
        for materia in materias_con_promedio:
            card = tk.Frame(scrollable_frame, bg='white', relief='raised', bd=1)
            card.pack(fill='x', pady=2, padx=5)
            
            color_frame = tk.Frame(card, bg=materia[3], width=5)
            color_frame.pack(side='left', fill='y')
            color_frame.pack_propagate(False)
            
            info_frame = tk.Frame(card, bg='white')
            info_frame.pack(fill='x', expand=True, padx=10, pady=8)
            
            # Nombre y código de la materia
            tk.Label(info_frame, text=materia[1], bg='white',
                    fg='#2c3e50', font=('Arial', 10, 'bold')).pack(anchor='w')
            tk.Label(info_frame, text=f"Código: {materia[2]}", bg='white',
                    fg='#7f8c8d', font=('Arial', 8)).pack(anchor='w')
            
            # Promedio
            promedio = materia[4] if materia[4] else "Sin calificaciones"
            evaluaciones = materia[5] if materia[5] else 0
            
            if materia[4]:
                color_promedio = self.success_color if materia[4] >= 70 else self.danger_color
                promedio_text = f"Promedio: {promedio:.1f}"
            else:
                color_promedio = '#7f8c8d'
                promedio_text = f"Promedio: {promedio}"
            
            tk.Label(info_frame, text=promedio_text, bg='white', 
                    fg=color_promedio, font=('Arial', 9, 'bold')).pack(anchor='w')
            tk.Label(info_frame, text=f"Evaluaciones: {evaluaciones}", bg='white',
                    fg='#7f8c8d', font=('Arial', 8)).pack(anchor='w')
    
    def mostrar_historial_calificaciones(self, persona_id):
        historial_frame = tk.Frame(self.content_frame, bg='#f8f9fa')
        historial_frame.pack(fill='both', expand=True)
        
        tk.Label(historial_frame, text="Historial de Calificaciones", bg='#f8f9fa',
                fg='#2c3e50', font=('Arial', 12, 'bold')).pack(anchor='w', pady=(10, 5))
        
        calificaciones = self.db.obtener_calificaciones_persona(persona_id)
        promedio_general = self.db.obtener_promedio_persona(persona_id)
        
        # Mostrar promedio general
        if calificaciones:
            promedio_label = tk.Label(historial_frame, 
                                text=f"Promedio General: {promedio_general:.1f}", 
                                bg='#f8f9fa', fg='#2c3e50', font=('Arial', 10, 'bold'))
            promedio_label.pack(anchor='w', pady=(0, 10))
        
        # Frame contenedor para treeview y scrollbar
        tree_container = tk.Frame(historial_frame, bg='#f8f9fa', height=250)
        tree_container.pack(fill='both', expand=True)
        tree_container.pack_propagate(False)
        
        tree_frame = tk.Frame(tree_container, bg='#f8f9fa')
        tree_frame.pack(fill='both', expand=True)
        
        if calificaciones:
            columns = ('Materia', 'Código', 'Calificación', 'Tipo', 'Fecha')
            tree = ttk.Treeview(tree_frame, columns=columns, show='headings', height=10)
            
            # Configurar columnas
            tree.column('Materia', width=120, anchor='w')
            tree.column('Código', width=80, anchor='center')
            tree.column('Calificación', width=80, anchor='center')
            tree.column('Tipo', width=80, anchor='center')
            tree.column('Fecha', width=100, anchor='center')
            
            # Configurar headings
            for col in columns:
                tree.heading(col, text=col)
            
            # Scrollbar vertical
            v_scrollbar = ttk.Scrollbar(tree_frame, orient=tk.VERTICAL, command=tree.yview)
            tree.configure(yscrollcommand=v_scrollbar.set)
            
            # Scrollbar horizontal
            h_scrollbar = ttk.Scrollbar(tree_frame, orient=tk.HORIZONTAL, command=tree.xview)
            tree.configure(xscrollcommand=h_scrollbar.set)
            
            # Insertar datos con colores según calificación
            for cal in calificaciones:
                calificacion_val = cal[2]
                # Determinar color según calificación
                if calificacion_val >= 90:
                    tags = ('excelente',)
                elif calificacion_val >= 80:
                    tags = ('bueno',)
                elif calificacion_val >= 70:
                    tags = ('regular',)
                else:
                    tags = ('bajo',)
                    
                tree.insert('', tk.END, values=cal[:5], tags=tags)
            
            # Configurar estilos para las calificaciones
            tree.tag_configure('excelente', background='#d4edda')  # Verde claro
            tree.tag_configure('bueno', background='#fff3cd')      # Amarillo claro
            tree.tag_configure('regular', background='#ffeaa7')    # Amarillo
            tree.tag_configure('bajo', background='#f8d7da')       # Rojo claro
            
            # Empaquetar treeview y scrollbars usando grid
            tree.grid(row=0, column=0, sticky='nsew')
            v_scrollbar.grid(row=0, column=1, sticky='ns')
            h_scrollbar.grid(row=1, column=0, sticky='ew')
            
            # Configurar el grid para expandirse
            tree_frame.grid_rowconfigure(0, weight=1)
            tree_frame.grid_columnconfigure(0, weight=1)
            
            # Agregar información de cantidad
            total_label = tk.Label(historial_frame, 
                                 text=f"Total de calificaciones: {len(calificaciones)}",
                                 bg='#f8f9fa', fg='#7f8c8d', font=('Arial', 9))
            total_label.pack(anchor='w', pady=(5, 0))
            
        else:
            # Mensaje cuando no hay calificaciones
            no_data_frame = tk.Frame(historial_frame, bg='#f8f9fa', height=100)
            no_data_frame.pack(fill='both', expand=True, pady=20)
            no_data_frame.pack_propagate(False)
            
            tk.Label(no_data_frame, text="No hay calificaciones registradas", 
                    bg='#f8f9fa', fg='#7f8c8d', font=('Arial', 11)).pack(expand=True)
            tk.Label(no_data_frame, text="Agrega calificaciones usando el formulario superior", 
                    bg='#f8f9fa', fg='#95a5a6', font=('Arial', 9)).pack()
            
    def mostrar_seccion_tramites(self):
        self.limpiar_contenido()

        titulo = tk.Label(
            self.content_frame,
            text="Trámites Universitarios",
            bg='#f8f9fa', fg='#2c3e50',
            font=('Arial', 14, 'bold')
        )
        titulo.pack(anchor='w', pady=(5, 15))

        tram_frame = tk.Frame(self.content_frame, bg='#f8f9fa')
        tram_frame.pack(fill='both', expand=True)

        tramites = [
            ("Constancia de Estudios", "Documento oficial que certifica tu estatus activo.", "#2E86AB"),
            ("Historial Académico", "Resumen detallado de tus materias y calificaciones.", "#A23B72"),
            ("Kardex", "Registro completo de tu avance académico.", "#F18F01"),
            ("Credencial Universitaria", "Trámite para solicitar o reponer credencial.", "#1B998B"),
            ("Servicio Social", "Información y estatus del servicio social.", "#9D4EDD"),
            ("Prácticas Profesionales", "Gestión y seguimiento de tus prácticas.", "#E71D36"),
            ("Baja Temporal", "Solicitud para suspender temporalmente tus estudios.", "#34495E")
        ]

        for nombre, desc, color in tramites:
            card = tk.Frame(tram_frame, bg='white', relief='raised', bd=1)
            card.pack(fill='x', pady=5, padx=5)

            left = tk.Frame(card, bg=color, width=8)
            left.pack(side='left', fill='y')

            info = tk.Frame(card, bg='white')
            info.pack(fill='both', expand=True, padx=10, pady=10)

            tk.Label(info, text=nombre, bg='white', fg='#2c3e50',
                    font=('Arial', 11, 'bold')).pack(anchor='w')
            tk.Label(info, text=desc, bg='white', fg='#7f8c8d',
                    font=('Arial', 9), wraplength=250, justify='left').pack(anchor='w', pady=(2, 5))

            tk.Button(
                info,
                text="Ver trámite",
                bg=color,
                fg='white',
                relief='flat',
                font=('Arial', 9, 'bold'),
                padx=10,
                pady=3,
                command=lambda t=nombre: self.mostrar_detalle_tramite(t)
            ).pack(anchor='e', pady=5)

# =============================================
# EJECUCIÓN PRINCIPAL
# =============================================

if __name__ == "__main__":
    # Iniciar con la pantalla de login
    root_login = tk.Tk()
    login_app = LoginSystem(root_login)
    root_login.mainloop()
