<!DOCTYPE html>
<html lang="es">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Detector de Emociones por Microexpresiones</title>
        <meta name="description" content="Sistema avanzado de detección de emociones mediante análisis de microexpresiones faciales para prevención de depresión y violencia">
        <style>
            /* Reset y estilos base */
            :root {
                --primary-color: #03a9f4;
                --secondary-color: #0288d1;
                --dark-color: #34495e;
                --light-color: #f5f5f5;
                --text-color: #333;
                --text-light: #fff;
                --accent-color: #ff6b6b;
            }

            * {
                box-sizing: border-box;
                margin: 0;
                padding: 0;
            }

            body {
                font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
                line-height: 1.6;
                color: var(--text-color);
                background-color: #f9f9f9;
            }

            a {
                text-decoration: none;
                color: inherit;
            }

            ul {
                list-style: none;
            }

            img {
                max-width: 100%;
                height: auto;
            }

            /* Layout principal */
            .container {
                display: flex;
                min-height: calc(100vh - 60px);
            }

            /* Barra de navegación */
            .navbar {
                background-color: var(--primary-color);
                display: flex;
                justify-content: space-between;
                align-items: center;
                padding: 0 20px;
                height: 60px;
                box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            }

            .navbar-brand {
                color: var(--text-light);
                font-size: 1.3rem;
                font-weight: bold;
                display: flex;
                align-items: center;
            }

            /* Menú de navegación */
            .nav-items {
                display: flex;
            }

            .nav-item {
                position: relative;
            }

            .nav-link, .dropdown-btn {
                color: var(--text-light);
                padding: 20px 15px;
                display: block;
                transition: background-color 0.3s;
                font-size: 0.95rem;
                text-align: center;
                border: none;
                background: transparent;
                cursor: pointer;
            }

            .nav-link:hover, .dropdown-btn:hover {
                background-color: var(--secondary-color);
            }

            .active {
                background-color: var(--secondary-color);
            }

            /* Menús desplegables */
            .dropdown-content {
                display: none;
                position: absolute;
                background-color: var(--text-light);
                min-width: 200px;
                box-shadow: 0 8px 16px rgba(0,0,0,0.1);
                z-index: 100;
                border-radius: 0 0 4px 4px;
                overflow: hidden;
            }

            .dropdown-link {
                padding: 12px 16px;
                display: block;
                transition: background-color 0.3s;
                color: var(--text-color);
            }

            .dropdown-link:hover {
                background-color: var(--light-color);
            }

            .dropdown:hover .dropdown-content {
                display: block;
            }

            /* Barra lateral */
            .sidebar {
                width: 250px;
                background-color: var(--text-light);
                padding: 20px;
                box-shadow: 2px 0 5px rgba(0,0,0,0.05);
            }

            .sidebar h2 {
                margin-bottom: 20px;
                color: var(--primary-color);
                font-size: 1.2rem;
                border-bottom: 2px solid var(--primary-color);
                padding-bottom: 10px;
            }

            .sidebar ul li {
                margin-bottom: 10px;
            }

            .sidebar ul li a {
                display: block;
                padding: 10px 12px;
                border-radius: 4px;
                transition: all 0.3s ease;
            }

            .sidebar ul li a:hover {
                background-color: var(--light-color);
                transform: translateX(5px);
            }

            .sidebar-image {
                margin-top: 20px;
                text-align: center;
                border-top: 1px solid #eee;
                padding-top: 20px;
            }

            /* Contenido principal */
            .main-content {
                flex: 1;
                padding: 30px;
                background-color: var(--text-light);
                box-shadow: 0 0 10px rgba(0,0,0,0.05);
            }

            .main-content h1 {
                color: var(--primary-color);
                margin-bottom: 20px;
                font-size: 2rem;
            }

            .main-content h2 {
                color: var(--secondary-color);
                margin: 25px 0 15px;
                font-size: 1.5rem;
            }

            .main-content p {
                margin-bottom: 15px;
                line-height: 1.7;
            }

            .feature-list {
                margin: 20px 0 20px 30px;
            }

            .feature-list li {
                margin-bottom: 10px;
                position: relative;
                padding-left: 25px;
            }

            .feature-list li:before {
                content: "✓";
                color: var(--accent-color);
                position: absolute;
                left: 0;
                font-weight: bold;
            }

            /* Pie de página */
            footer {
                background-color: var(--dark-color);
                color: var(--text-light);
                text-align: center;
                padding: 20px;
                font-size: 0.9rem;
            }

            /* Estilos para móviles */
            .menu-toggle {
                display: none;
                background: none;
                border: none;
                color: var(--text-light);
                font-size: 1.5rem;
                cursor: pointer;
                padding: 10px;
            }

            @media (max-width: 768px) {
                .container {
                    flex-direction: column;
                }

                .navbar {
                    padding: 0 10px;
                }

                .navbar-brand {
                    font-size: 1.1rem;
                }

                .menu-toggle {
                    display: block;
                }

                .nav-items {
                    display: none;
                    flex-direction: column;
                    width: 100%;
                    position: absolute;
                    top: 60px;
                    left: 0;
                    background-color: var(--dark-color);
                }

                .nav-items.active {
                    display: flex;
                }

                .nav-item {
                    width: 100%;
                    text-align: left;
                }

                .dropdown-content {
                    position: static;
                    width: 100%;
                    box-shadow: none;
                    display: none;
                }

                .dropdown.active .dropdown-content {
                    display: block;
                }

                .sidebar {
                    width: 100%;
                    padding: 20px;
                    order: 2;
                }

                .main-content {
                    padding: 20px;
                    order: 1;
                }

                .main-content h1 {
                    font-size: 1.8rem;
                }
            }
        </style>
    </head>
    <body>
        <nav class="navbar" aria-label="Navegación principal">
            <a href="index.html" class="navbar-brand">Detección de Emociones</a>

            <button class="menu-toggle" aria-expanded="false" aria-controls="nav-items" aria-label="Menú">
                ☰
            </button>

            <ul class="nav-items" id="nav-items">
                <li class="nav-item">
                    <a href="index.html" class="nav-link active" aria-current="page">Inicio</a>
                </li>
                
                <li class="nav-item dropdown">
                    <button class="dropdown-btn" aria-expanded="false" aria-haspopup="true">
                        Productos ▼
                    </button>
                    <ul class="dropdown-content" aria-label="Submenú de productos">
                        <li><a href="Programa.html" class="dropdown-link">Codigo</a></li>
                        <li><a href="Dispositivos.html" class="dropdown-link">Dispositivos</a></li>
                        <li><a href="api.html" class="dropdown-link">Prueba </a></li>
                    </ul>
                </li>

                <li class="nav-item">
                    <a href="registro.html" class="nav-link">Registro</a>
                </li>

                <li class="nav-item dropdown">
                    <button class="dropdown-btn" aria-expanded="false" aria-haspopup="true">
                        Recursos ▼
                    </button>
                    <ul class="dropdown-content" aria-label="Submenú de recursos">
                        <li><a href="blog.html" class="dropdown-link">Blog</a></li>
                        <li><a href="casos-estudio.html" class="dropdown-link">Casos de estudio</a></li>
                        <li><a href="faq.html" class="dropdown-link">Preguntas frecuentes</a></li>
                    </ul>
                </li>

                <li class="nav-item">
                    <a href="contacto.html" class="nav-link">Contacto</a>
                </li>
            </ul>
        </nav>

        <div class="container">
            <aside class="sidebar" aria-label="Menú secundario">
                <h2>Menú</h2>
                <ul>
                    <li><a href="tecnologia.html">Nuestra Tecnología</a></li>
                    <li><a href="aplicaciones.html">Aplicaciones</a></li>
                    <li><a href="precios.html">Planes y Precios</a></li>
                    <li><a href="demo.html">Solicitar Demo</a></li>
                    <li><a href="equipo.html">Nuestro Equipo</a></li>
                </ul>

                <div class="sidebar-image">
                    <img src="img/logo.png" alt="Logo del sistema de detección de emociones" width="200"> 
                </div>
            </aside>

            <main class="main-content">
                <h1>Detección de Emociones por Microexpresiones</h1>
                <p>Nuestro sistema avanzado de análisis facial permite detectar emociones a través de microexpresiones, proporcionando información valiosa para diversos campos como psicología, recursos humanos, seguridad, y especialmente para la prevención de enfermedades como la depresión y situaciones de violencia.</p>
                <p>El detectar las emociones y comprenderlas nos ayuda a tener un mejor estilo de vida. El estudio de las emociones hoy en día nos ayuda a prevenir muchas enfermedades que siempre han estado presentes, pero durante los últimos años han cobrado una mayor importancia, volviéndose un tema común en el hablar de enfermedades como la depresión, ansiedad o temas de mayor impacto como la prevención de violencia.</p>
                <p>Debemos entender que hay muchas emociones que no conocemos. Hay emociones positivas y emociones negativas. A veces experimentamos emociones que anteriormente no habíamos experimentado y al no saber qué es lo que sentimos, podemos omitirlas o confundirlas con otras emociones o incluso con síntomas de salud.</p>
                <p>Al comprender que las emociones están relacionadas con nuestra salud, debemos saber que es importante entender que hay emociones que pueden afectar de manera grave nuestra salud mental y provocar cambios en nuestro cuerpo. Podemos generar una enfermedad sin darnos cuenta y vivir con ella asociándola a algo normal.</p>
                <! aqui debe de ir una imagen!> 
                <section>
                    <h2>¿Por qué es importante detectar las emociones?</h2>
                    <p>Las emociones son fundamentales en nuestras vidas desde que las desarrollamos. Saber cómo manejarlas nos ayuda a tener un mejor control de nuestra vida y prevenir enfermedades mentales como la depresión. La detección temprana de patrones emocionales negativos puede ser clave para intervenciones preventivas.</p>
                    <p>El saber qué tipos de emociones tenemos y cuál es la emoción que generalmente predomina es un factor clave para prevenir alguna enfermedad que pueda afectar nuestra salud. El ir con un especialista nos puede asesorar y detallar de mejor manera las emociones. Conocer mejor nuestras emociones nos puede dar herramientas para poder cambiar nuestro estilo de vida y así poder manejar nuestras emociones de mejor manera para la prevención de enfermedades.</p>
                <! aqui debe de ir una imagen !>
                </section>

                <section>
                    <h2>¿Cómo funciona?</h2>
                    <p>Nuestra tecnología utiliza algoritmos de inteligencia artificial para analizar pequeños movimientos faciales que ocurren en fracciones de segundo (microexpresiones), revelando emociones genuinas que podrían pasar desapercibidas para el ojo humano. El sistema puede identificar patrones asociados con depresión, ansiedad o agresividad.</p>
                    <p>Nuestra plataforma utiliza inteligencia artificial avanzada para detectar microexpresiones faciales imperceptibles al ojo humano, identificando patrones asociados a depresión, ansiedad y maltrato en tiempo real. El proceso se divide en 3 etapas clave:</p>
                    
                    <ul class="feature-list">
                        <li>Captura y Análisis Facial
                            <ul>
                                <li>Registra movimientos sutiles en menos de 0.5 segundos mediante cámaras convencionales.</li>
                                <li>Enfoque en zonas críticas:
                                    <ul>
                                        <li>Cejas y frente: Contracciones por estrés emocional.</li>
                                        <li>Área ocular: Micropicores o parpadeo irregular.</li>
                                        <li>Boca y mandíbula: Asimetrías en sonrisas o tensión.</li>
                                    </ul>
                                </li>
                            </ul>
                        </li>
                        <li>Procesamiento con IA Especializada
                            <ul>
                                <li>Redes neuronales profundas entrenadas con +10,000 expresiones faciales etiquetadas.</li>
                                <li>Detecta patrones como:
                                    <ul>
                                        <li>Depresión: Disminución de movimientos faciales (hipomimia).</li>
                                        <li>Maltrato: Microgestos de miedo/ansiedad recurrentes.</li>
                                    </ul>
                                </li>
                                <li>Precisión del 92.4% en pruebas clínicas</li>
                            </ul>
                        </li>
                    </ul>
                </section>

                <section>
                    <h2>Características principales</h2>
                    <ul class="feature-list">
                        <li>Análisis en tiempo real con retroalimentación inmediata</li>
                        <li>Precisión del 92% en condiciones controladas</li>
                        <li>Integración con múltiples plataformas y dispositivos</li>
                        <li>Informes detallados y personalizables para profesionales</li>
                        <li>Alertas tempranas para patrones emocionales de riesgo</li>
                        <li>Sistema de seguimiento emocional a largo plazo</li>
                    </ul>
                </section>

                <section>
                    <h2>Aplicaciones en salud mental</h2>
                    <p>Nuestro sistema es especialmente útil para:</p>
                    <ul class="feature-list">
                        <li>Detección temprana de signos de depresión</li>
                        <li>Monitoreo de pacientes en terapia psicológica</li>
                        <li>Prevención de crisis emocionales</li>
                        <li>Identificación de riesgo de conductas violentas</li>
                        <li>Seguimiento del progreso en tratamientos</li>
                    </ul>
                </section>
            </main>
        </div>

        <footer>
            <p>&copy; 2025 Detector de Emociones. Todos los derechos reservados.</p>
            <p style="margin-top: 10px; font-size: 0.8rem;">Centro de Cooperación Académica Industria (CCAI)</p>
    <p> M@nu </p>
        </footer>

        <script>
            document.addEventListener('DOMContentLoaded', function () {
                const toggle = document.querySelector('.menu-toggle');
                const navItems = document.querySelector('.nav-items');
                const dropdownBtns = document.querySelectorAll('.dropdown-btn');

                // Menú principal para móviles
                toggle.addEventListener('click', function () {
                    const isExpanded = this.getAttribute('aria-expanded') === 'true';
                    this.setAttribute('aria-expanded', !isExpanded);
                    navItems.classList.toggle('active');
                });

                // Submenús para móviles
                dropdownBtns.forEach(btn => {
                    btn.addEventListener('click', function (e) {
                        if (window.innerWidth <= 768) {
                            e.preventDefault();
                            const dropdown = this.parentElement;
                            const isExpanded = this.getAttribute('aria-expanded') === 'true';

                            this.setAttribute('aria-expanded', !isExpanded);
                            dropdown.classList.toggle('active');
                        }
                    });
                });

                // Cerrar menús al hacer clic fuera
                document.addEventListener('click', function (e) {
                    if (!e.target.closest('.navbar') && window.innerWidth <= 768) {
                        toggle.setAttribute('aria-expanded', 'false');
                        navItems.classList.remove('active');

                        dropdownBtns.forEach(btn => {
                            btn.setAttribute('aria-expanded', 'false');
                            btn.parentElement.classList.remove('active');
                        });
                    }
                });
            });
        </script>
    </body>
</html>
