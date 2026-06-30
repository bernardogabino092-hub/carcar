<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CardioSana Perú v3.0 - Versión Final Estable</title>
    <style>
        :root {
            --bg-color: #f4f6f9;
            --primary: #1e3a8a;
            --primary-light: #3b82f6;
            --accent-red: #ef4444;
            --accent-orange: #f97316;
            --accent-green: #10b981;
            --accent-purple: #8b5cf6;
            --text-dark: #1e293b;
            --white: #ffffff;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #cbd5e1;
            color: var(--text-dark);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 10px;
        }

        .phone-container {
            width: 100%;
            max-width: 420px;
            height: 890px;
            background-color: var(--white);
            border-radius: 35px;
            box-shadow: 0 12px 28px rgba(0,0,0,0.3);
            display: flex;
            flex-direction: column;
            border: 9px solid #334155;
            position: relative;
            overflow: hidden;
        }

        .status-bar {
            background-color: #334155;
            color: white;
            padding: 6px 20px;
            font-size: 11px;
            display: flex;
            justify-content: space-between;
            flex-shrink: 0;
        }

        header {
            background-color: var(--primary);
            color: white;
            padding: 15px;
            text-align: center;
            border-bottom-left-radius: 20px;
            border-bottom-right-radius: 20px;
            flex-shrink: 0;
        }

        header h1 { font-size: 20px; font-weight: bold; }
        header p { font-size: 11px; opacity: 0.8; margin-top: 2px; }

        .main-content {
            flex: 1;
            overflow-y: auto;
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 15px;
            padding-bottom: 30px;
            background-color: var(--bg-color);
        }

        .app-screen {
            display: none;
            flex-direction: column;
            gap: 15px;
            width: 100%;
        }

        .app-screen.active {
            display: flex;
        }

        .card {
            background-color: var(--white);
            border-radius: 16px;
            padding: 16px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.03);
            border-left: 5px solid var(--primary-light);
        }

        .card h2 {
            font-size: 14px;
            margin-bottom: 10px;
            color: var(--primary);
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .nav-bar {
            background-color: #ffffff;
            border-top: 1px solid #e2e8f0;
            display: flex;
            justify-content: space-around;
            padding: 10px 0;
            flex-shrink: 0;
        }

        .nav-item {
            background: none;
            border: none;
            display: flex;
            flex-direction: column;
            align-items: center;
            font-size: 10px;
            color: #64748b;
            cursor: pointer;
            width: 25%;
            font-weight: 500;
        }

        .nav-item.active {
            color: var(--primary-light);
            font-weight: bold;
        }

        .nav-item .icon { font-size: 18px; margin-bottom: 2px; }

        .btn {
            background-color: var(--primary-light);
            color: white;
            border: none;
            padding: 10px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            font-size: 13px;
            width: 100%;
            margin-top: 5px;
        }
        .btn:hover { background-color: var(--primary); }
        .btn-purple { background-color: var(--accent-purple); }
        .btn-purple:hover { background-color: #6d28d9; }

        .selector-box, .input-box {
            width: 100%;
            padding: 10px;
            border: 1px solid #cbd5e1;
            border-radius: 8px;
            margin-bottom: 8px;
            font-size: 13px;
            background-color: var(--white);
        }

        .alert-banner {
            background-color: #fef2f2;
            border: 1px solid var(--accent-red);
            color: var(--accent-red);
            padding: 12px;
            border-radius: 10px;
            font-size: 12px;
            font-weight: bold;
            display: none;
        }

        .notification-push {
            background-color: #1e293b; color: white; padding: 12px; border-radius: 12px; font-size: 12px;
            position: absolute; top: 45px; left: 15px; right: 15px; z-index: 100; display: none; box-shadow: 0 4px 15px rgba(0,0,0,0.3);
        }

        .stress-badge {
            display: inline-block; padding: 4px 10px; border-radius: 12px; font-size: 12px; font-weight: bold; color: white;
        }

        .motivation-box {
            background: linear-gradient(135deg, #eff6ff, #dbeafe);
            border-left: 5px solid var(--primary-light);
            padding: 12px; border-radius: 12px; font-style: italic; font-size: 12px; color: #1e40af; text-align: center;
            min-height: 45px; display: flex; align-items: center; justify-content: center;
        }

        .menu-detail-box {
            background-color: #f8fafc; border: 1px solid #e2e8f0; padding: 10px; border-radius: 8px; margin-top: 5px; font-size: 12px;
        }
        .menu-tag { font-weight: bold; color: var(--primary); }
    </style>
</head>
<body>

    <div class="phone-container">
        <!-- Barra de Estado Superior -->
        <div class="status-bar">
            <span>CardioSana Sistema v3.0 📲</span>
            <span>12:15 🫀</span>
        </div>

        <!-- Alertas Push -->
        <div id="pushNotification" class="notification-push">
            <strong>CardioSana Notificación:</strong> <span id="pushText"></span>
            <button onclick="document.getElementById('pushNotification').style.display='none'" style="float:right; background:none; border:none; color:white; font-weight:bold; cursor:pointer;">X</button>
        </div>

        <!-- Encabezado -->
        <header>
            <h1 id="pageTitle">CardioSana Perú</h1>
            <p id="pageSubtitle">Historial Clínico de Precisión</p>
        </header>

        <!-- CONTENIDO DE LAS PÁGINAS -->
        <div class="main-content">
            
            <!-- PÁGINA 1: INICIO -->
            <div id="screen-inicio" class="app-screen active">
                <div class="card" style="border-left-color: var(--accent-red);">
                    <h2>👤 Datos de Telemonitoreo</h2>
                    <p style="font-size:12px; line-height: 1.4;">
                        <strong>Paciente:</strong> Carlos Alberto | Edad: 45 años<br>
                        <strong>Diagnóstico:</strong> Hipertensión Arterial (Estadio 1)<br>
                        <strong>Acción Enfermera:</strong> Regular estrés emocional y romper sedentarismo.
                    </p>
                </div>

                <!-- Módulo de Mensajes Alentadores Robustecido -->
                <div class="card" style="border-left-color: var(--accent-purple);">
                    <h2>✨ Soporte Psicoemocional y Motivación</h2>
                    <p style="font-size: 12px; color:#555; margin-bottom: 8px;">Mensajes aleatorios de tu enfermera para calmar el sistema nervioso y motivarte a entrenar:</p>
                    <div id="motivationText" class="motivation-box">
                        "Cada paso cuenta para proteger tu salud. Respira hondo, estás haciendo un excelente trabajo hoy."
                    </div>
                    <button class="btn btn-purple" onclick="generarMensajeAlentador()">🍀 Siguiente Mensaje Motivador</button>
                </div>

                <!-- Simulación Corregida del Wearbox Bluetooth -->
                <div class="card">
                    <h2>⌚ Vinculación de Reloj Inteligente</h2>
                    <p style="font-size: 11px; color: #64748b; margin-bottom: 10px;">Transfiere manualmente los datos de Estrés (VFC) recopilados por tu dispositivo mediante Bluetooth:</p>
                    <div style="text-align: center; margin-bottom: 10px;">
                        <span id="stressStatus" class="stress-badge" style="background-color: #94a3b8;">RELOJ DESCONECTADO (Sin Datos)</span>
                    </div>
                    <button class="btn" onclick="sincronizarRelojBluetooth()">🔄 Sincronizar Reloj vía Bluetooth</button>
                </div>
            </div>

            <!-- PÁGINA 2: BIENESTAR -->
            <div id="screen-bienestar" class="app-screen">
                <div class="card" style="border-left-color: var(--accent-orange);">
                    <h2>🚶‍♀️ Pausas de Descompresión Vascular</h2>
                    <p style="font-size: 12px; color: #555; margin-bottom: 8px;">Prescripción: Levántate y camina 5 minutos por cada hora de oficina para liberar óxido nítrico endotelial.</p>
                    <div style="font-size: 18px; font-weight: bold; text-align: center; background: #f1f5f9; padding: 10px; border-radius: 8px; color: var(--primary);">
                        Pausas Completadas: <span id="countPausas">0</span> / 6
                    </div>
                    <button class="btn" onclick="registrarPausaCardio()">+ Registrar Pausa Realizada</button>
                </div>

                <div class="card" style="border-left-color: var(--accent-green);">
                    <h2>🧘‍♂️ Respiración Guiada (Nervio Vago)</h2>
                    <p style="font-size: 12px; color: #555; margin-bottom: 8px;">Ejercicios rítmicos controlados de 3 minutos para modular la hipertensión reactiva por estrés.</p>
                    <button class="btn" style="background-color: var(--accent-green);" onclick="iniciarRespiracion()">Iniciar Coherencia Cardíaca</button>
                </div>
            </div>

            <!-- PÁGINA 3: NUTRICIÓN INTEGRAL CON SELECTORES MÚLTIPLES -->
            <div id="screen-nutricion" class="app-screen">
                <div class="card" style="border-left-color: var(--accent-green);">
                    <h2>📅 Planificación Diaria de Platos (Bajo Sodio)</h2>
                    <p style="font-size: 12px; color: #555; margin-bottom: 8px;">1. Selecciona el día de la semana:</p>
                    <select class="selector-box" id="daySelector" onchange="mostrarOpcionesPlatos()">
                        <option value="">-- Elige un día --</option>
                        <option value="lunes">Lunes - Control Semanal</option>
                        <option value="martes">Martes - Rutina Laboral</option>
                        <option value="miercoles">Miércoles - Mitad de Semana</option>
                        <option value="jueves">Jueves - Estabilidad</option>
                        <option value="viernes">Viernes - Cierre de Jornada</option>
                        <option value="sabado">Sábado - Almuerzo Familiar</option>
                        <option value="domingo">Domingo - Descanso Consciente</option>
                    </select>

                    <div id="platoSelectorContainer" style="display:none; margin-top: 10px;">
                        <p style="font-size: 12px; color: #555; margin-bottom: 8px;">2. Elige la alternativa de comida que deseas para hoy:</p>
                        <select class="selector-box" id="platoAlternativaSelector" onchange="renderizarMenuFinal()">
                            <option value="A">Alternativa Tradicional A (Baja en Sal)</option>
                            <option value="B">Alternativa Ligera B (Cardioprotectora)</option>
                        </select>
                    </div>

                    <div id="menuContainer" style="display:none; margin-top: 15px;">
                        <div class="menu-detail-box"><span class="menu-tag">🍳 Desayuno Recomendado:</span> <span id="txtDesayuno"></span></div>
                        <div class="menu-detail-box"><span class="menu-tag">🍲 Almuerzo Peruano Adaptado:</span> <span id="txtAlmuerzo"></span></div>
                        <div class="menu-detail-box"><span class="menu-tag">🥗 Cena Ligera:</span> <span id="txtCena"></span></div>
                    </div>
                </div>
            </div>

            <!-- PÁGINA 4: MONITOREO -->
            <div id="screen-monitoreo" class="app-screen">
                <div class="card" style="border-left-color: var(--primary-light);">
                    <h2>🫀 Captura de Presión Arterial</h2>
                    <p style="font-size: 11px; color: #64748b; margin-bottom: 8px;">Simulación de datos inalámbricos del tensiómetro de brazo:</p>
                    
                    <div style="display:flex; gap:8px; margin-bottom: 8px;">
                        <input type="number" id="inputSistolica" class="input-box" placeholder="Sis (Ej: 120)" style="margin-bottom:0;">
                        <input type="number" id="inputDiastolica" class="input-box" placeholder="Dia (Ej: 80)" style="margin-bottom:0;">
                    </div>
                    <button class="btn" onclick="evaluarPresionManual()">Guardar en Servidor Clínico</button>
                    
                    <div style="margin-top: 10px; display: flex; gap: 5px;">
                        <button class="btn" style="background-color: #475569; font-size: 11px;" onclick="simularTensiometro(136, 87)">Rango Oficina (Alerta)</button>
                        <button class="btn" style="background-color: var(--accent-red); font-size: 11px;" onclick="simularTensiometro(182, 121)">Crisis Hipertensiva (Urgente)</button>
                    </div>
                </div>

                <div id="crisisAlert" class="alert-banner">
                    🚨 CRISIS HIPERTENSIVA DETECTADA (≥180/120 mmHg).<br>
                    <span style="font-weight: normal; display:block; margin: 4px 0;">La aplicación ha suspendido las pantallas comunes y ha enviado tu geolocalización GPS a la central de enfermería.</span>
                    <button class="btn" style="background-color: var(--accent-red);" onclick="alert('Conectando con la unidad de trauma-shock y cardiólogo de guardia...')">📞 ENLAZAR CON EMERGENCIAS</button>
                </div>
            </div>

        </div>

        <!-- BARRA DE NAVEGACIÓN CORREGIDA -->
        <div class="nav-bar">
            <button class="nav-item active" onclick="navegarApp('inicio', 'CardioSana Perú', 'Historial Clínico de Precisión', this)">
                <span class="icon">🏠</span>
                <span>Inicio</span>
            </button>
            <button class="nav-item" onclick="navegarApp('bienestar', 'Bienestar Vascular', 'Modulación del Estrés', this)">
                <span class="icon">🏃‍♀️</span>
                <span>Bienestar</span>
            </button>
            <button class="nav-item" onclick="navegarApp('nutricion', 'Nutrición DASH', 'Control de Sodio y Calorías', this)">
                <span class="icon">🥗</span>
                <span>Nutrición</span>
            </button>
            <button class="nav-item" onclick="navegarApp('monitoreo', 'Módulo Alertas', 'Telemetría y Control Crítico', this)">
                <span class="icon">🫀</span>
                <span>Monitoreo</span>
            </button>
        </div>
    </div>

    <script>
        // FUNCIÓN DE NAVEGACIÓN EN PÁGINAS (CORREGIDA Y ESTABLE)
        function navegarApp(screenId, title, subtitle, elementoBoton) {
            document.querySelectorAll('.app-screen').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(item => item.classList.remove('active'));

            document.getElementById(`screen-${screenId}`).classList.add('active');
            if(elementoBoton) {
                elementoBoton.classList.add('active');
            }

            document.getElementById('pageTitle').innerText = title;
            document.getElementById('pageSubtitle').innerText = subtitle;
        }

        function lanzarNotificacion(mensaje) {
            const push = document.getElementById('pushNotification');
            document.getElementById('pushText').innerText = mensaje;
            push.style.display = 'block';
            setTimeout(() => { push.style.display = 'none'; }, 4000);
        }

        // BATERÍA AMPLIA DE FRASES ENCOURAGING Y ENERGETIC
        const mensajesMotivacionales = [
            "«Respira profundo, Carlos. Tu salud arterial mejora con cada minuto que decides tomar el control.»",
            "«Bajar la velocidad en la oficina 5 minutos no es perder tiempo, es ganar años de vida junto a tu familia.»",
            "«¡Gran trabajo! Tu enfermera valida tu esfuerzo diario. Tu constancia es el mejor escudo para tu corazón.»",
            "«Hoy es un día perfecto para cuidar tus arterias. Haz tu pausa activa y siente cómo se oxigena tu cuerpo.»",
            "«Mantén la calma. El estrés laboral es pasajero, pero el cuidado de tu corazón es para siempre.»",
            "«¡Muévete por ti! Cinco minutos de caminata reducen la presión arterial y recargan tu energía laboral.»",
            "«Tu corazón late por tus sueños. Regálale un respiro, camina un momento y estira los músculos.»",
            "«La salud no se compra, se construye paso a paso. ¡Registra tu pausa cardiovascular ahora!»",
            "«Un vaso de agua, un suspiro hondo y una breve caminata: la mejor receta de enfermería para tu tarde.»",
            "«Estás al mando de tu bienestar. Deja que las tensiones salgan con cada exhalación profunda.»"
        ];
        
        function generarMensajeAlentador() {
            const randomMsg = mensajesMotivacionales[Math.floor(Math.random() * mensajesMotivacionales.length)];
            document.getElementById('motivationText').innerText = randomMsg;
            lanzarNotificacion("Mensaje de motivación actualizado.");
        }

        // CORRECCIÓN SENSOR ÓPTICO DE RELOJ A ACCIÓN MANUAL BLUETOOTH
        function sincronizarRelojBluetooth() {
            const status = document.getElementById('stressStatus');
            status.innerText = "⏳ ENLAZANDO HARDWARE...";
            status.style.backgroundColor = "#f97316";
            
            setTimeout(() => {
                status.innerText = "⚠️ REPORTE BLUETOOTH: ESTRÉS ELEVADO";
                status.style.backgroundColor = "#f97316";
                lanzarNotificacion("¡Datos del Smartwatch importados! Se detecta estrés tensional. Te sugerimos ejecutar Coherencia Cardíaca.");
            }, 1200);
        }

        // BIENESTAR
        let pausas = 0;
        function registrarPausaCardio() {
            pausas++;
            document.getElementById('countPausas').innerText = pausas;
            lanzarNotificacion(`Pausa #${pausas} guardada. Vasodilatación activa.`);
        }

        function iniciarRespiracion() {
            alert("🧘‍♂️ Ejercicio activo: Inhala en 5 segundos... Exhala en 5 segundos. Estimulando tono vagal parasimpático.");
            document.getElementById('stressStatus').innerText = "✅ REPORTE BLUETOOTH: ESTRÉS BAJO";
            document.getElementById('stressStatus').style.backgroundColor = "#10b981";
        }

        // BASE DE DATOS AMPLIADA: MENÚ DE 7 DÍAS CON OPCIONES MÚLTIPLES (A y B)
        const baseMenusMundiales = {
            lunes: {
                A: { d: "Avena con manzana y chía.", a: "Lomo Saltado Cardio (Carne magra, sal baja en sodio y papas al horno).", c: "Tortilla de claras con espinaca." },
                B: { d: "Yogurt light con almendras.", a: "Pechuga a la plancha con puré de quinua y vainitas al vapor.", c: "Ensalada de alcachofas con cubos de pollo." }
            },
            martes: {
                A: { d: "Jugo de papaya + Tostada con palta.", a: "Pescado a la plancha (Bonito) con puré de camote y lechuga.", c: "Sopa clara de pollo picado con verduras." },
                B: { d: "Batido de fresas con leche descremada.", a: "Guiso de bife magro con arroz integral medido y ensalada de rabanito.", c: "Crema de zapallo ligera (sin crema de leche) con tostadas integrales." }
            },
            miercoles: {
                A: { d: "Quinua caliente + 2 huevos duros.", a: "Ají de Gallina Fit (Base de crema de quinua en lugar de pan blanco).", c: "Ensalada de pota sancochada con limón y choclo desgranado." },
                B: { d: "Ensalada de frutas con linaza.", a: "Estofado de pollo sin piel cargado de zanahoria, alverjas y papa sancochada.", c: "Omelette de champiñones con queso fresco light." }
            },
            jueves: {
                A: { d: "Yogurt griego descremado con fresas.", a: "Seco de Pollo con arroz integral medido y bastante culantro fresco.", c: "Brochetas de pechuga de pollo con pimientos y cebolla." },
                B: { d: "Avena líquida con canela + Galletas de salvado.", a: "Sudado de pescado (Chita o Cojinova) sazonado con tomate, cebolla y chicha de jora.", c: "Ensalada tricolor (Betarraga, zanahoria, brócoli) con pechuga deshilachada." }
            },
            viernes: {
                A: { d: "Emoliente sin azúcar + Sándwich de pollo.", a: "Escabeche de Pescado usando vinagre natural (para evitar exceso de sal).", c: "Puré de espinacas con filete de pollo." },
                B: { d: "Papaya picada con semillas de girasol.", a: "Arroz con pollo versión ligera (utilizando arroz integral y pechuga picada).", c: "Consomé de verduras con dados de queso light serrano." }
            },
            sabado: {
                A: { d: "Avena con plátano e hilos de miel de abeja.", a: "Cebiche Clásico Peruano (Omega-3 marino, con porción medida de camote).", c: "Ensalada tibia de quinoa con verduras grilladas." },
                B: { d: "Tostadas integrales con revuelto de claras y tomate.", a: "Cau Cau de Pollo Fit (utilizando pechuga en lugar de mondongo y hierbabuena fresca).", c: "Filete de pescado al horno con espárragos al vapor." }
            },
            domingo: {
                A: { d: "Ensalada de frutas (Papaya, kiwi, melón) con chía.", a: "Pollo a la brasa casero (sin piel, con hierbas aromáticas) y papas nativas.", c: "Infusión de manzanilla + Galletas de arroz con palta." },
                B: { d: "Yogurt descremado con copos de avena.", a: "Arroz con Mariscos Cardio (Mariscos al vapor, sazonados con pimentón y arroz integral).", c: "Sopa criolla de dieta (fideos integrales, carne molida magra y leche light)." }
            }
        };

        function mostrarOpcionesPlatos() {
            const day = document.getElementById('daySelector').value;
            const subSelector = document.getElementById('platoSelectorContainer');
            if (day) {
                subSelector.style.display = 'block';
                renderizerMenuFinal();
            } else {
                subSelector.style.display = 'none';
                document.getElementById('menuContainer').style.display = 'none';
            }
        }

        function renderizerMenuFinal() {
            const day = document.getElementById('daySelector').value;
            const alt = document.getElementById('platoAlternativaSelector').value;
            const container = document.getElementById('menuContainer');
            
            if (day && alt && baseMenusMundiales[day] && baseMenusMundiales[day][alt]) {
                const seleccion = baseMenusMundiales[day][alt];
                document.getElementById('txtDesayuno').innerText = seleccion.d;
                document.getElementById('txtAlmuerzo').innerText = seleccion.a;
                document.getElementById('txtCena').innerText = seleccion.c;
                container.style.display = 'block';
            } else {
                container.style.display = 'none';
            }
        }

        // TENSIÓMETRO
        function simularTensiometro(sistolica, diastolica) {
            document.getElementById('inputSistolica').value = sistolica;
            document.getElementById('inputDiastolica').value = diastolica;
            evaluarPresion(sistolica, diastolica);
        }

        function evaluarPresionManual() {
            const sis = parseInt(document.getElementById('inputSistolica').value);
            const dia = parseInt(document.getElementById('inputDiastolica').value);
            if (!sis || !dia) { alert("Digita valores correctos."); return; }
            evaluarPresion(sis, dia);
        }

        function evaluarPresion(sis, dia) {
            const alertBox = document.getElementById('crisisAlert');
            if (sis >= 180 || dia >= 120) {
                alertBox.style.display = 'block';
                lanzarNotificacion("🚨 CRÍTICO: Bloqueo de seguridad por Crisis Hipertensiva.");
            } else {
                alertBox.style.display = 'none';
                lanzarNotificacion(`✅ Captura exitosa: ${sis}/${dia} mmHg enviado.`);
            }
        }
    </script>
</body>
</html>
