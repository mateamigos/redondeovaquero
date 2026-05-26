<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Pistolero del Redondeo - Edición Oeste</title>
    <style>
        :root { --brown: #5d4037; }
        
        body { 
            margin: 0; 
            height: 100vh; 
            width: 100vw;
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            justify-content: flex-start;
            font-family: 'Verdana', sans-serif;
            overflow: hidden;
            background: url('https://lh3.googleusercontent.com/d/1qhcbTh9rG-QDZPK8RleRqX3QdGvnJlBv=s3000') no-repeat center center fixed;
            background-size: cover;
            cursor: url("data:image/svg+xml;charset=utf-8,%3Csvg xmlns='http://www.w3.org/2000/svg' width='64' height='64' viewBox='0 0 64 64'%3E%3Cg stroke='%23ff0000' stroke-width='3'%3E%3Cline x1='32' y1='0' x2='32' y2='64'/%3E%3Cline x1='0' y1='32' x2='64' y2='32'/%3E%3Ccircle cx='32' cy='32' r='20' fill='none'/%3E%3Ccircle cx='32' cy='32' r='2' fill='%23ff0000'/%3E%3C/g%3E%3C/svg%3E") 32 32, crosshair;
            user-select: none;
            touch-action: manipulation;
        }

        body::after {
            content: "";
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background-image: radial-gradient(rgba(0,0,0,0.1) 1px, transparent 1px);
            background-size: 20px 20px;
            pointer-events: none;
            z-index: -1;
        }

        /* --- EFECTOS VISUALES --- */
        .agujero {
            position: absolute;
            width: 12px;
            height: 12px;
            background: #111;
            border-radius: 50%;
            box-shadow: 0 0 0 1px rgba(255,255,255,0.2), inset 0 0 5px #000;
            pointer-events: none;
            z-index: 2; 
            transform: translate(-50%, -50%);
            opacity: 0.9;
            transition: opacity 2s;
        }

        .humo {
            position: absolute;
            width: 30px;
            height: 30px;
            background: radial-gradient(circle, rgba(200,200,200,0.8) 0%, rgba(255,255,255,0) 70%);
            border-radius: 50%;
            pointer-events: none;
            z-index: 100;
            transform: translate(-50%, -50%);
            animation: anim-humo 0.8s ease-out forwards;
        }

        @keyframes anim-humo {
            0% { transform: translate(-50%, -50%) scale(0.5); opacity: 0.8; }
            100% { transform: translate(-50%, -150px) scale(3); opacity: 0; }
        }

        #boca-canon {
            position: absolute;
            top: 0;
            left: 50%;
            width: 1px;
            height: 1px;
            transform: translateX(-50%);
        }

        /* --- PANTALLA DE INICIO --- */
        #inicio {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 20;
            background: rgba(0,0,0,0.4); 
            overflow-y: auto; 
        }

        h1 {
            color: #ffcc00;
            font-family: 'Courier New', Courier, monospace;
            font-size: clamp(1.8rem, 5vw, 3.5rem);
            text-shadow: 4px 4px 0 #3e2723;
            margin-bottom: 5px;
            text-align: center;
            background: #3e2723;
            padding: 10px 20px;
            border: 2px solid #deb887;
            transform: rotate(-2deg);
        }

        .subtitulo {
            color: #deb887;
            font-family: 'Courier New', monospace;
            font-size: clamp(0.8rem, 3vw, 1.2rem);
            font-weight: bold;
            margin-bottom: 15px;
            text-shadow: 2px 2px 4px #000;
            text-align: center;
            background: rgba(62, 39, 35, 0.8);
            padding: 5px 10px;
            border-radius: 4px;
        }

        #tabla-records {
            background: #fdf5e6;
            color: #3e2723;
            border: 4px double #3e2723;
            padding: 10px;
            margin-bottom: 25px;
            width: 80%;
            max-width: 350px;
            text-align: center;
            font-family: 'Courier New', monospace;
            box-shadow: 5px 5px 15px rgba(0,0,0,0.5);
        }
        #tabla-records h3 { margin: 5px 0 10px 0; border-bottom: 2px dashed #3e2723; }
        .fila-record { display: flex; justify-content: space-between; font-weight: bold; padding: 2px 0; }

        .btn-modo {
            background: #3e2723;
            color: #deb887;
            font-family: 'Courier New', monospace;
            font-size: 1.2rem;
            font-weight: bold;
            padding: 10px 20px;
            margin: 5px;
            border: 2px solid #deb887;
            border-radius: 5px;
            cursor: pointer;
            cursor: inherit;
            transition: transform 0.1s, background 0.2s;
            width: 85%;
            max-width: 350px;
        }

        .btn-modo:hover { background: #5d4037; transform: scale(1.05); }
        .btn-modo:active { transform: scale(0.95); }

        /* --- PANTALLA FINAL --- */
        #pantalla-final {
            display: none;
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8);
            z-index: 200;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        .wanted-poster {
            background: #fdf5e6; 
            background-image: radial-gradient(#dccbb6 10%, transparent 10%), radial-gradient(#dccbb6 10%, transparent 10%);
            background-size: 10px 10px;
            width: 90%;
            max-width: 400px;
            padding: 20px;
            text-align: center;
            border: 8px solid #3e2723;
            box-shadow: 0 0 0 4px #fdf5e6 inset, 10px 10px 30px rgba(0,0,0,0.7);
            color: #000;
            font-family: 'Courier New', monospace;
            transform: rotate(1deg);
        }

        .wanted-title {
            font-size: 3.5rem;
            font-weight: 900;
            letter-spacing: 5px;
            margin: 0;
            border-bottom: 4px solid #000;
            line-height: 1;
        }

        .wanted-sub { font-size: 1.2rem; font-weight: bold; margin: 10px 0; }
        .wanted-reward { font-size: 2rem; font-weight: bold; margin: 20px 0; color: #3e2723; }
        
        #input-nombre {
            background: transparent;
            border: none;
            border-bottom: 3px solid #000;
            font-family: 'Courier New', monospace;
            font-size: 1.5rem;
            text-align: center;
            width: 80%;
            text-transform: uppercase;
            outline: none;
            font-weight: bold;
            margin-bottom: 20px;
        }

        #btn-guardar {
            background: #3e2723;
            color: #ffcc00;
            border: 2px solid #000;
            padding: 10px 20px;
            font-family: 'Courier New', monospace;
            font-weight: bold;
            font-size: 1.2rem;
            cursor: pointer;
            width: 100%;
        }

        /* --- JUEGO --- */
        #juego {
            display: none;
            width: 100%;
            height: 100%;
            flex-direction: column;
            align-items: center;
        }

        #pizarra {
            background: #3e2723;
            color: #deb887;
            padding: 1.5vh 2vw;
            border: 0.8vh solid #281815;
            border-radius: 4px;
            margin-top: 2vh;
            box-shadow: 0 10px 20px rgba(0,0,0,0.5);
            z-index: 10;
            text-align: center;
            width: 90%;
            max-width: 650px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: clamp(0.8rem, 2.2vh, 1.6rem);
            font-family: 'Courier New', Courier, monospace;
            font-weight: bold;
            cursor: inherit; 
        }

        #timer-container { display: none; color: #ff4444; }

        .btn-pizarra {
            background: #d32f2f;
            color: white;
            border: 1px solid #fff;
            padding: 5px 10px;
            font-size: 0.8rem;
            cursor: inherit;
            margin-left: 5px;
            font-family: sans-serif;
            min-width: 30px;
        }
        
        #btn-music, #btn-sfx { background: #ff9800; }

        /* --- PANEL MUNICIÓN --- */
        #panel-municion {
            position: absolute;
            top: 14vh; 
            left: 50%;
            transform: translateX(-50%);
            z-index: 150;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
            pointer-events: auto;
        }

        #info-balas {
            background: rgba(0,0,0,0.6);
            color: #fff;
            padding: 3px 12px;
            border-radius: 12px;
            font-family: 'Courier New', monospace;
            font-weight: bold;
            font-size: 1.1rem;
            border: 2px solid #deb887;
            text-shadow: 1px 1px 0 #000;
        }
        
        .tambor-visual { color: #ffcc00; letter-spacing: 2px; font-size: 1.3rem; }

        .btn-accion-arma {
            display: none; 
            background: #d32f2f;
            color: white;
            border: 2px solid #fff;
            padding: 5px 15px;
            font-size: 1.1rem;
            font-family: 'Courier New', monospace;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 5px 10px rgba(0,0,0,0.5);
            animation: palpitar 1s infinite;
            text-transform: uppercase;
        }

        #btn-comprar { background: #2e7d32; border-color: #85bb65; }

        @keyframes palpitar {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        #zona-tiro {
            position: relative;
            flex: 1;
            width: 100%;
            display: flex; 
            justify-content: center;
            align-items: center; 
            overflow: hidden;
            cursor: inherit;
        }

        /* --- ARMA --- */
        #arma-contenedor {
            position: absolute;
            bottom: -5vh; 
            right: 15vw;  
            left: auto;   
            width: 0;
            height: 0;
            z-index: 50; 
            pointer-events: none; 
            transform: scale(0.5); 
            transform-origin: bottom center;
        }

        #arma {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 0;
            height: 0;
            transform-origin: center bottom; 
        }

        .culata {
            position: absolute;
            bottom: -5vh; 
            left: -5vh; 
            width: 10vh;
            height: 25vh;
            background: linear-gradient(90deg, #3e2723 0%, #6d4c41 40%, #5d4037 60%, #3e2723 100%);
            border-radius: 10px 10px 0 0;
            box-shadow: inset 0 0 20px rgba(0,0,0,0.8);
            z-index: 1;
            clip-path: polygon(10% 0, 90% 0, 100% 100%, 0% 100%);
        }

        .mecanismo {
            position: absolute;
            bottom: 18vh;
            left: -3vh;
            width: 6vh;
            height: 12vh;
            background: linear-gradient(90deg, #2c2c2c, #777, #999, #777, #2c2c2c);
            border-radius: 4px;
            z-index: 2;
            box-shadow: 0 5px 15px rgba(0,0,0,0.5);
        }

        .martillo {
            position: absolute;
            bottom: 11vh;
            left: 50%;
            transform: translateX(-50%);
            width: 1.5vh;
            height: 3vh;
            background: linear-gradient(to right, #111, #444);
            border-radius: 5px 5px 0 0;
            z-index: 3;
        }

        .canon {
            position: absolute;
            bottom: 28vh;
            left: -1.5vh; 
            width: 3vh;
            height: 40vh; 
            background: linear-gradient(90deg, #000 0%, #333 20%, #111 40%, #444 50%, #111 60%, #333 80%, #000 100%);
            z-index: 1;
        }

        .deposito {
            position: absolute;
            bottom: 28vh;
            left: -1.5vh;
            width: 3vh;
            height: 35vh;
            background: linear-gradient(90deg, #1a1a1a, #333, #1a1a1a);
            border-radius: 50% 50% 0 0;
            transform: scale(0.8); 
            z-index: 0;
        }

        .mira {
            position: absolute;
            bottom: 66vh; 
            left: -0.2vh;
            width: 0.4vh;
            height: 1.5vh;
            background: #000;
            z-index: 4;
        }

        .retroceso .culata, .retroceso .mecanismo, .retroceso .canon, .retroceso .deposito, .retroceso .martillo, .retroceso .mira {
            animation: anim-retroceso 0.15s ease-out;
        }

        @keyframes anim-retroceso {
            0% { transform: translateY(0); }
            50% { transform: translateY(5vh); } 
            100% { transform: translateY(0); }
        }

        #valla-contenedor {
            display: flex;
            justify-content: center;
            align-items: flex-end; 
            gap: 2vw;
            width: 95%;
            height: 55vh; 
            position: absolute;
            bottom: 15vh; 
            padding-bottom: 0;
            border-bottom: 4vh solid #5d4037;
            border-image: linear-gradient(to right, #5d4037, #4e342e, #5d4037) 1;
            background-image: linear-gradient(to right, transparent 48%, #3e2723 48%, #3e2723 52%, transparent 52%);
            background-size: 25vw 100%;
            background-repeat: repeat-x;
            background-position: bottom;
            z-index: 1;
        }

        #valla-contenedor::after {
            content: "";
            position: absolute;
            bottom: -5vh; 
            width: 100%;
            height: 5vh;
            background: rgba(0,0,0,0.5);
            z-index: 0;
        }

        .lata {
            width: 14.4vmin; 
            height: 20vmin;
            min-width: 72px;  
            min-height: 104px;
            max-width: 128px;
            max-height: 176px;
            position: relative; 
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            background: linear-gradient(to right, #444 0%, #aaa 25%, #fff 40%, #888 100%);
            border-top: 0.5vmin solid #999;
            border-bottom: 0.5vmin solid #222;
            border-radius: 4px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.6);
            cursor: inherit;
            -webkit-tap-highlight-color: transparent;
            z-index: 5;
            margin-bottom: -1vh; 
            transition: transform 0.1s; 
        }

        .lata::before { 
            content: '';
            position: absolute;
            top: 20%; bottom: 20%; width: 100%;
            background: #fdf5e6; z-index: 1;
            box-shadow: 0 1px 2px rgba(0,0,0,0.2);
            border-top: 1px solid #dba; border-bottom: 1px solid #dba;
        }

        .lata::after {
            content: '';
            position: absolute; top: -5px; width: 100%; height: 10px;
            background: linear-gradient(to right, #555, #ddd, #555);
            border-radius: 50%; border: 1px solid #333; z-index: 0;
        }
        
        .billete {
            position: absolute;
            width: 120px;
            height: 60px;
            background-color: #85bb65;
            border: 2px solid #2e7d32;
            border-radius: 4px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.3);
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Courier New', monospace;
            font-weight: bold;
            color: #004d00;
            font-size: 1.5rem;
            z-index: 100; 
            cursor: inherit; 
            user-select: none;
            pointer-events: auto; 
        }
        .billete::before { content: '$'; position: absolute; left: 5px; top: 5px; font-size: 0.8rem; }
        .billete::after { content: '$'; position: absolute; right: 5px; bottom: 5px; font-size: 0.8rem; }
        .billete-centro {
            border: 1px dashed #004d00;
            padding: 2px 10px;
            border-radius: 20px;
            background: rgba(255,255,255,0.2);
        }

        .contenido-fraccion {
            position: relative; z-index: 3; display: flex; flex-direction: column; align-items: center;
            font-size: clamp(1.1rem, 2.5vmin, 1.8rem); 
            font-weight: 900; 
            color: #3e2723; 
            font-family: 'Courier New', monospace;
            text-align: center;
            padding: 0 5px;
        }

        .lata:active { transform: scale(0.95); }

        #feedback {
            position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%);
            font-size: clamp(3rem, 10vw, 6rem); font-weight: bold; text-shadow: 4px 4px 8px rgba(0,0,0,0.5);
            pointer-events: none; z-index: 100; white-space: nowrap;
        }
    </style>
</head>
<body onload="cargarRecords()">

    <div id="inicio">
        <h1>PISTOLERO DEL<br>REDONDEO</h1>
        <div class="subtitulo">DISPARA A LA CANTIDAD CORRECTA</div>
        
        <div id="tabla-records">
            <h3>SALÓN DE LA FAMA</h3>
            <div id="lista-records"></div>
        </div>

        <button class="btn-modo" onclick="iniciarJuego('vaquero')">MODO VAQUERO<br><span style="font-size:0.8rem">(Estático)</span></button>
        <button class="btn-modo" onclick="iniciarJuego('pistolero')">MODO PISTOLERO<br><span style="font-size:0.8rem">(Dinámico)</span></button>
        <button class="btn-modo" onclick="iniciarJuego('caballo_loco')">MODO CABALLO LOCO<br><span style="font-size:0.8rem">(Dinámico + 10s)</span></button>
    </div>

    <div id="pantalla-final">
        <div class="wanted-poster">
            <div class="wanted-title">WANTED</div>
            <div class="wanted-sub">DEAD OR ALIVE</div>
            <div style="font-size:0.8rem; margin-bottom:15px;">FOR MATH CRIMES</div>
            
            <div class="wanted-reward">REWARD<br>$<span id="final-score">0</span></div>
            
            <input type="text" id="input-nombre" maxlength="20" placeholder="NOMBRE DEL BANDIDO">
            <button id="btn-guardar" onclick="guardarYSalir()">COBRAR RECOMPENSA</button>
        </div>
    </div>

    <div id="juego">
        <div id="pizarra">
            <div>REWARD: <span id="puntos">0</span>$</div>
            <div>RETO:<br><span id="obj-texto" style="color:#ffcc00">10,00</span></div>
            <div>LEVEL: <span id="nivel-texto">1</span>/50</div>
            <div id="timer-container">TIME: <span id="timer">10</span></div>
            <div>
                <button id="btn-music" class="btn-pizarra" onclick="toggleMusic()">🔇</button>
                <button id="btn-sfx" class="btn-pizarra" onclick="toggleSfx()">🔇</button>
                <button id="btn-volver" class="btn-pizarra" onclick="volverInicio()">X</button>
            </div>
        </div>

        <div id="zona-tiro"></div>

        <div id="panel-municion">
            <div id="info-balas">
                <div class="tambor-visual" id="tambor-visual">●●●●●●</div>
                <div style="font-size:0.7rem; text-align:center;">RESERVA: <span id="reserva-balas">50</span></div>
            </div>
            <button id="btn-recargar" class="btn-accion-arma" onclick="recargar(event)">RECARGAR ⟳</button>
            <button id="btn-comprar" class="btn-accion-arma" onclick="comprarMunicion(event)">COMPRAR BALAS (-200$)</button>
        </div>
        
        <div id="arma-contenedor">
            <div id="arma">
                <div class="culata"></div>
                <div class="mecanismo"></div>
                <div class="martillo"></div>
                <div class="deposito"></div>
                <div class="canon">
                    <div id="boca-canon"></div>
                </div>
                <div class="mira"></div>
            </div>
        </div>
    </div>
    
    <div id="feedback"></div>

    <script>
        let puntos = 0;
        let modoJuego = 'vaquero'; 
        let intervaloMovimiento;
        let intervaloTimer;
        let tiempoRestante = 10;
        let nivelesCompletados = 0; 
        const MAX_NIVELES = 50; 

        // Variables de Munición
        let balasTambor = 6;
        let balasReserva = 50;
        
        let enBonus = false;
        let intervaloSpawnBonus;
        let intervaloMovBonus;
        let count100 = 0;
        let count1000 = 0;
        
        const zona = document.getElementById('zona-tiro');
        const arma = document.getElementById('arma'); 

        // --- LÓGICA MATEMÁTICA MEJORADA ---
        function obtenerOpciones(numero, tipo) {
            let factorPrincipal = 1;
            let factorSecundario = 10; // por defecto, provocaremos duda con las décimas

            if(tipo === 'decenas') {
                factorPrincipal = 0.1;
                factorSecundario = 1; // duda con las unidades
            } else if(tipo === 'unidades') {
                factorPrincipal = 1;
                factorSecundario = 10; // duda con las décimas
            } else if(tipo === 'décimas') {
                factorPrincipal = 10;
                factorSecundario = 1; // duda con las unidades
            }

            // 1. Respuesta correcta
            let correcta = (Math.round(numero * factorPrincipal) / factorPrincipal).toFixed(2);
            // 3. Respuesta correcta del distractor (ej. redondeó a décimas cuando pedían unidades)
            let correctaSecundaria = (Math.round(numero * factorSecundario) / factorSecundario).toFixed(2);
            
            // Función auxiliar para forzar el redondeo en la dirección contraria a la correcta
            function opuesto(num, factor, roundedCorrect) {
                let valFloor = (Math.floor(num * factor) / factor).toFixed(2);
                let valCeil = (Math.ceil(num * factor) / factor).toFixed(2);
                
                // Si el número original ya era redondo (ej. 14.00), forzamos una diferencia
                if (valFloor === valCeil) {
                    return (parseFloat(valFloor) + (1/factor)).toFixed(2);
                }
                return roundedCorrect === valFloor ? valCeil : valFloor;
            }

            // 2. Respuesta errónea (se equivocó redondeando al alza/baja en la medida principal)
            let opuestoPrincipal = opuesto(numero, factorPrincipal, correcta);
            // 4. Respuesta errónea del distractor (se equivocó de medida y encima redondeó al revés)
            let opuestoSecundario = opuesto(numero, factorSecundario, correctaSecundaria);

            // Juntamos las 4 opciones
            let opcionesGeneradas = [correcta, opuestoPrincipal, correctaSecundaria, opuestoSecundario];
            
            // Eliminamos duplicados por si algún número genera colisiones
            let unicas = [...new Set(opcionesGeneradas)];

            // Si por alguna razón nos faltan opciones para llegar a 4 latas, rellenamos con valores cercanos
            let paso = 1 / factorPrincipal;
            let extra = 1;
            while(unicas.length < 4) {
                let candidato = (parseFloat(correcta) + paso * extra * (Math.random() < 0.5 ? 1 : -1)).toFixed(2);
                if (!unicas.includes(candidato) && parseFloat(candidato) >= 0) {
                    unicas.push(candidato);
                }
                extra++;
            }

            // Convertimos al formato final asegurando el orden aleatorio
            let opciones = unicas.slice(0, 4).map(texto => ({
                texto: texto,
                correcta: texto === correcta
            }));
            
            return opciones.sort(() => Math.random() - 0.5);
        }

        // --- AUDIO ---
        let audioCtx;
        let masterGain;
        let isMusicOn = false;
        let isSfxOn = false;
        let nextNoteTime = 0;
        let beatCount = 0;
        const tempo = 240; 
        const secondsPerBeat = 60.0 / tempo;
        let timerID;
        let noiseBuffer = null;

        const F = { 
            C2: 65.41, G2: 98.00, C3: 130.81, D3: 146.83, E3: 164.81, F3: 174.61, G3: 196.00, A3: 220.00, Bb3: 233.08, B3: 246.94,
            C4: 261.63, D4: 293.66, Eb4: 311.13, E4: 329.63, F4: 349.23, G4: 392.00, A4: 440.00, Bb4: 466.16, C5: 523.25, E5: 659.25, G5: 783.99
        };

        const chordProgression = [ 'C', 'C', 'C', 'C', 'F', 'F', 'C', 'C', 'G', 'F', 'C', 'G' ];
        const scales = { 'C': [F.C4, F.D4, F.Eb4, F.E4, F.G4, F.A4, F.C5], 'F': [F.F3, F.G3, F.A3, F.C4, F.D4, F.F4], 'G': [F.G3, F.B3, F.D4, F.E4, F.G4, F.Bb4] };
        const bassLines = { 'C': [F.C2, F.G2], 'F': [F.F3/2, F.C3], 'G': [F.G2, F.D3] };
        const chordsHigh = { 'C': [F.E3, F.G3, F.C4], 'F': [F.A3, F.C4, F.F4], 'G': [F.B3, F.D4, F.G4] };

        function initAudio() {
            if (!audioCtx) {
                const AudioContext = window.AudioContext || window.webkitAudioContext;
                audioCtx = new AudioContext();
                masterGain = audioCtx.createGain();
                masterGain.gain.value = 0.15; 
                masterGain.connect(audioCtx.destination);
                const bufferSize = audioCtx.sampleRate * 1; 
                noiseBuffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
                const data = noiseBuffer.getChannelData(0);
                for (let i = 0; i < bufferSize; i++) { data[i] = Math.random() * 2 - 1; }
            }
        }

        function toggleMusic() {
            initAudio();
            const btn = document.getElementById('btn-music');
            if (audioCtx.state === 'suspended') { audioCtx.resume(); }
            if (!isMusicOn) {
                isMusicOn = true; btn.innerText = "🎵"; nextNoteTime = audioCtx.currentTime + 0.1; scheduler();
            } else { isMusicOn = false; btn.innerText = "🔇"; clearTimeout(timerID); }
        }

        function toggleSfx() {
            initAudio();
            const btn = document.getElementById('btn-sfx');
            if (audioCtx.state === 'suspended') { audioCtx.resume(); }
            if (!isSfxOn) { isSfxOn = true; btn.innerText = "🔫"; } else { isSfxOn = false; btn.innerText = "🔇"; }
        }

        function playShoot() {
            if (!isSfxOn || !audioCtx) return;
            const t = audioCtx.currentTime;
            const noise = audioCtx.createBufferSource();
            noise.buffer = noiseBuffer;
            const filter = audioCtx.createBiquadFilter();
            filter.type = 'lowpass'; filter.frequency.setValueAtTime(3500, t); filter.frequency.exponentialRampToValueAtTime(500, t + 0.2);
            const shootGain = audioCtx.createGain();
            shootGain.gain.setValueAtTime(0, t); shootGain.gain.linearRampToValueAtTime(12.0, t + 0.01); shootGain.gain.exponentialRampToValueAtTime(0.01, t + 0.4); 
            noise.connect(filter); filter.connect(shootGain); shootGain.connect(masterGain);
            noise.start(t); noise.stop(t + 0.5);
        }

        function playClick() {
            if (!isSfxOn || !audioCtx) return;
            const t = audioCtx.currentTime;
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = 'square'; osc.frequency.setValueAtTime(1000, t);
            gain.gain.setValueAtTime(0.1, t); gain.gain.exponentialRampToValueAtTime(0.001, t + 0.05);
            osc.connect(gain); gain.connect(masterGain);
            osc.start(t); osc.stop(t + 0.05);
        }
        
        function playReloadSound() {
            if (!isSfxOn || !audioCtx) return;
            const t = audioCtx.currentTime;
            playTone(F.G3, 0.1, t, 0.5);
            playTone(F.C4, 0.1, t + 0.15, 0.5);
            playTone(F.E4, 0.1, t + 0.3, 0.5);
        }

        function playTone(freq, duration, time, volFactor = 1) {
            if (!isMusicOn && volFactor < 1) return; 
            if (volFactor === 1 && !isMusicOn) return; 
            
            const createOsc = (detune) => {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = 'sawtooth'; osc.frequency.value = freq; osc.detune.value = detune; 
                const filter = audioCtx.createBiquadFilter();
                filter.type = 'lowpass'; filter.frequency.value = 800 + (freq * 2); 
                osc.connect(filter); filter.connect(gain); gain.connect(masterGain);
                gain.gain.setValueAtTime(0, time); gain.gain.linearRampToValueAtTime(volFactor * 0.1, time + 0.02); gain.gain.exponentialRampToValueAtTime(0.01, time + duration); 
                osc.start(time); osc.stop(time + duration + 0.1);
            };
            createOsc(0); createOsc(8); 
        }

        function scheduleMusic() {
            if(!isMusicOn) return;
            while (nextNoteTime < audioCtx.currentTime + 0.1) {
                scheduleMeasure(beatCount, nextNoteTime); nextNoteTime += secondsPerBeat; beatCount++;
            }
            timerID = setTimeout(scheduleMusic, 25);
        }
        function scheduler() { scheduleMusic(); }
        function scheduleMeasure(beat, time) {
            const barIndex = Math.floor(beat / 4) % chordProgression.length;
            const currentChordName = chordProgression[barIndex];
            const beatInBar = beat % 4;
            if (beatInBar === 0 || beatInBar === 2) {
                const bassNote = bassLines[currentChordName][beatInBar === 0 ? 0 : 1];
                playTone(bassNote, 0.4, time, 0.8);
            } else {
                const chordNotes = chordsHigh[currentChordName];
                chordNotes.forEach(n => playTone(n, 0.2, time, 0.4));
            }
            if (Math.random() > 0.1) {
                const scale = scales[currentChordName];
                const note = scale[Math.floor(Math.random() * scale.length)];
                const octaveMult = Math.random() > 0.5 ? 2 : 1;
                playTone(note * octaveMult, Math.random() > 0.7 ? 0.2 : 0.1, time + (Math.random() > 0.8 ? secondsPerBeat / 2 : 0), 0.6);
            }
        }
        // --- FIN AUDIO ---

        function actualizarApuntado(x, y) {
            const rect = document.body.getBoundingClientRect();
            const pivotX = rect.width * 0.85; 
            const pivotY = rect.height; 
            const deltaX = x - pivotX;
            const deltaY = y - pivotY;
            const anguloRad = Math.atan2(deltaY, deltaX);
            const anguloDeg = anguloRad * (180 / Math.PI) + 90;
            arma.style.transform = `rotate(${anguloDeg - 5}deg)`;
        }

        document.addEventListener('mousemove', (e) => {
            if(document.getElementById('juego').style.display === 'flex') { actualizarApuntado(e.clientX, e.clientY); }
        });
        document.addEventListener('touchmove', (e) => {
            if(document.getElementById('juego').style.display === 'flex' && e.touches[0]) { actualizarApuntado(e.touches[0].clientX, e.touches[0].clientY); }
        }, {passive: true});


        // --- LOGICA DE MUNICION ---
        function actualizarHUDMunicion() {
            const visual = '●'.repeat(balasTambor) + '○'.repeat(6 - balasTambor);
            document.getElementById('tambor-visual').innerText = visual;
            document.getElementById('reserva-balas').innerText = balasReserva;
            
            const btnRecargar = document.getElementById('btn-recargar');
            const btnComprar = document.getElementById('btn-comprar');
            
            if (balasTambor === 0) {
                if (balasReserva > 0) {
                    btnRecargar.style.display = 'block';
                    btnComprar.style.display = 'none';
                } else {
                    if (puntos >= 200) {
                        btnRecargar.style.display = 'none';
                        btnComprar.style.display = 'block';
                    } else {
                        btnRecargar.style.display = 'none';
                        btnComprar.style.display = 'none';
                        gameOverPorPobreza();
                    }
                }
            } else {
                btnRecargar.style.display = 'none';
                btnComprar.style.display = 'none';
            }
        }

        function gameOverPorPobreza() {
            limpiarIntervalos();
            document.getElementById('juego').style.display = 'none';
            document.getElementById('pantalla-final').style.display = 'flex';
            
            document.querySelector('.wanted-title').innerText = "GAME OVER";
            document.querySelector('.wanted-sub').innerText = "ARRUINADO Y DESARMADO";
            document.querySelector('.wanted-reward').innerHTML = "<div style='font-size:1rem; color:#800; line-height:1.5; margin-top:10px;'>NO TE QUEDAN BALAS<br>Y NO TIENES DINERO PARA COMPRAR</div>";
            
            document.getElementById('input-nombre').style.display = 'none';
            const btn = document.getElementById('btn-guardar');
            btn.innerText = "VOLVER AL MENÚ";
            btn.onclick = volverInicio;
        }

        function intentarDisparar() {
            if (balasTambor > 0) {
                balasTambor--;
                actualizarHUDMunicion();
                return true;
            } else {
                playClick(); 
                actualizarHUDMunicion(); 
                return false;
            }
        }

        function recargar(e) {
            e.stopPropagation();
            if (balasReserva > 0) {
                balasTambor = 6;
                balasReserva = Math.max(0, balasReserva - 6); 
                playReloadSound();
                actualizarHUDMunicion();
            }
        }

        function comprarMunicion(e) {
            e.stopPropagation();
            if (puntos >= 200) {
                puntos -= 200;
                document.getElementById('puntos').innerText = puntos;
                balasReserva += 50;
                balasTambor = 6; 
                playReloadSound();
                actualizarHUDMunicion();
            }
        }

        // --- EFECTOS VISUALES ---
        function animarDisparo(x, y) {
            playShoot();
            arma.classList.remove('retroceso'); 
            void arma.offsetWidth; 
            arma.classList.add('retroceso');
            crearHumo();
        }

        function crearHumo() {
            const boca = document.getElementById('boca-canon');
            const rect = boca.getBoundingClientRect();
            const humo = document.createElement('div');
            humo.className = 'humo';
            humo.style.left = (rect.left + rect.width/2) + 'px';
            humo.style.top = rect.top + 'px';
            document.body.appendChild(humo);
            setTimeout(() => { if(humo.parentNode) humo.remove(); }, 1000);
        }

        function crearAgujero(x, y) {
            const agujero = document.createElement('div');
            agujero.className = 'agujero';
            agujero.style.left = x + 'px';
            agujero.style.top = y + 'px';
            if(document.getElementById('valla-contenedor')) {
                document.getElementById('valla-contenedor').appendChild(agujero);
            } else {
                zona.appendChild(agujero);
            }
            setTimeout(() => { 
                agujero.style.opacity = 0;
                setTimeout(() => { if(agujero.parentNode) agujero.remove(); }, 2000);
            }, 2000);
        }
        
        zona.addEventListener('click', (e) => {
            let esLata = e.target.closest('.lata') || e.target.closest('.billete');
            
            if (intentarDisparar()) {
                animarDisparo(e.clientX, e.clientY);
                if (!esLata) {
                    crearAgujero(e.clientX, e.clientY);
                }
            }
        });


        function iniciarJuego(modo) {
            modoJuego = modo;
            puntos = 0;
            balasTambor = 6;
            balasReserva = 50;
            nivelesCompletados = 0;
            document.getElementById('puntos').innerText = puntos;
            document.getElementById('inicio').style.display = 'none';
            document.getElementById('pantalla-final').style.display = 'none';
            document.getElementById('juego').style.display = 'flex';
            
            actualizarHUDMunicion();
            initAudio();

            if(modo === 'caballo_loco'){
                document.getElementById('timer-container').style.display = 'block';
            } else {
                document.getElementById('timer-container').style.display = 'none';
            }

            generarNivel();
        }

        function volverInicio() {
            limpiarIntervalos();
            document.getElementById('feedback').innerText = "";
            document.getElementById('juego').style.display = 'none';
            document.getElementById('pantalla-final').style.display = 'none';
            document.getElementById('inicio').style.display = 'flex';
            
            document.querySelector('.wanted-title').innerText = "WANTED";
            document.querySelector('.wanted-sub').innerText = "DEAD OR ALIVE";
            document.querySelector('.wanted-reward').innerHTML = "REWARD<br>$<span id='final-score'>0</span>";
            document.getElementById('input-nombre').style.display = 'block';
            const btn = document.getElementById('btn-guardar');
            btn.innerText = "COBRAR RECOMPENSA";
            btn.onclick = guardarYSalir;
            
            cargarRecords();
        }

        function finalizarJuego() {
            limpiarIntervalos();
            document.getElementById('juego').style.display = 'none';
            document.getElementById('pantalla-final').style.display = 'flex';
            document.getElementById('final-score').innerText = puntos;
            document.getElementById('input-nombre').value = "";
            document.getElementById('input-nombre').focus();
        }

        function guardarYSalir() {
            const nombre = document.getElementById('input-nombre').value.trim() || "DESCONOCIDO";
            const nuevoRecord = { nombre: nombre, score: puntos };
            let records = JSON.parse(localStorage.getItem('pistolero_records') || '[]');
            records.push(nuevoRecord);
            records.sort((a, b) => b.score - a.score);
            records = records.slice(0, 5);
            localStorage.setItem('pistolero_records', JSON.stringify(records));
            volverInicio();
        }

        function cargarRecords() {
            let records = JSON.parse(localStorage.getItem('pistolero_records') || '[]');
            const listaDiv = document.getElementById('lista-records');
            listaDiv.innerHTML = "";
            if(records.length === 0) {
                listaDiv.innerHTML = "<div style='font-style:italic'>SIN REGISTROS AÚN</div>";
                return;
            }
            records.forEach((rec, index) => {
                const fila = document.createElement('div');
                fila.className = 'fila-record';
                fila.innerHTML = `<span>#${index+1} ${rec.nombre.substring(0,10)}</span> <span>$${rec.score}</span>`;
                listaDiv.appendChild(fila);
            });
        }

        function limpiarIntervalos() {
            clearInterval(intervaloMovimiento);
            clearInterval(intervaloTimer);
            clearInterval(intervaloSpawnBonus);
            clearInterval(intervaloMovBonus);
        }

        function iniciarTimer() {
            clearInterval(intervaloTimer);
            tiempoRestante = 10;
            document.getElementById('timer').innerText = tiempoRestante;
            intervaloTimer = setInterval(() => {
                tiempoRestante--;
                document.getElementById('timer').innerText = tiempoRestante;
                if(tiempoRestante <= 0) {
                    clearInterval(intervaloTimer);
                    puntos -= 10;
                    document.getElementById('puntos').innerText = puntos;
                    document.getElementById('feedback').innerText = "¡TIEMPO! ⏳";
                    document.getElementById('feedback').style.color = "orange";
                    if(modoJuego !== 'vaquero') clearInterval(intervaloMovimiento);
                    setTimeout(() => {
                        document.getElementById('feedback').innerText = "";
                        generarNivel();
                    }, 1000);
                }
            }, 1000);
        }

        // --- GENERACIÓN DE NIVEL ---
        function generarNivel() {
            if (nivelesCompletados >= MAX_NIVELES) { finalizarJuego(); return; }
            document.getElementById('nivel-texto').innerText = (nivelesCompletados + 1);
            enBonus = false; 
            zona.innerHTML = '';
            limpiarIntervalos();

            if(modoJuego === 'caballo_loco') { iniciarTimer(); }

            // Generar reto matemático: decenas, unidades o décimas
            const tipos = ['decenas', 'unidades', 'décimas'];
            let tipoSeleccionado = tipos[Math.floor(Math.random() * tipos.length)];
            
            // Generar un número base aleatorio (0 a 100, y a veces hasta 500)
            let numeroBase = (Math.random() * 100);
            if (Math.random() < 0.2) numeroBase *= 5; 
            numeroBase = Math.round(numeroBase * 100) / 100; // Mantenemos solo 2 decimales para que sea limpio

            // Mostrar el texto del reto en la pizarra
            document.getElementById('obj-texto').innerHTML = `A ${tipoSeleccionado.toUpperCase()}:<br><span style="font-size:1.3em;">${numeroBase.toFixed(2).replace('.', ',')}</span>`;

            // Obtenemos las opciones con el nuevo sistema que genera la duda
            let opciones = obtenerOpciones(numeroBase, tipoSeleccionado);
            
            let contenedorPadre = zona;
            if (modoJuego === 'vaquero') {
                const valla = document.createElement('div');
                valla.id = 'valla-contenedor';
                zona.appendChild(valla);
                contenedorPadre = valla; 
            }

            let latasElements = [];
            opciones.forEach((opt) => {
                const div = document.createElement('div');
                div.className = 'lata';
                
                // Inyectamos el texto reemplazando punto por coma decimal
                div.innerHTML = `<span class="contenido-fraccion">${opt.texto.replace('.', ',')}</span>`;
                
                div.onclick = (e) => {
                    e.stopPropagation(); 
                    actualizarApuntado(e.clientX, e.clientY);
                    
                    if (intentarDisparar()) {
                        animarDisparo(e.clientX, e.clientY); 
                        disparar(div, opt.correcta); 
                    }
                };
                contenedorPadre.appendChild(div);
                latasElements.push(div);
            });

            if (modoJuego === 'pistolero' || modoJuego === 'caballo_loco') {
                iniciarMovimiento(latasElements);
            }
        }

        function iniciarFaseBonus() {
            enBonus = true;
            zona.innerHTML = ''; 
            limpiarIntervalos();
            count100 = 0; count1000 = 0;
            document.getElementById('obj-texto').innerText = "BONUS!";
            document.getElementById('feedback').innerText = "¡LLUVIA DE $$$!";
            document.getElementById('feedback').style.color = "#85bb65";
            setTimeout(() => { document.getElementById('feedback').innerText = ""; }, 1500);

            document.getElementById('timer-container').style.display = 'block';
            let tiempoBonus = 10;
            document.getElementById('timer').innerText = tiempoBonus;
            
            intervaloTimer = setInterval(() => {
                tiempoBonus--;
                document.getElementById('timer').innerText = tiempoBonus;
                if(tiempoBonus <= 0) { terminarBonus(); }
            }, 1000);

            intervaloSpawnBonus = setInterval(spawnBillete, 250);
            intervaloMovBonus = setInterval(moverBilletes, 16);
        }

        function spawnBillete() {
            let valor = 1;
            let rand = Math.random() * 100;
            if (rand < 50) valor = 1; else if (rand < 70) valor = 5; else if (rand < 85) valor = 10; else if (rand < 92) valor = 20; else if (rand < 96) valor = 50; else if (rand < 99) { if(count100 < 3) { valor = 100; count100++; } else { valor = 50; } } else { if(count1000 < 1) { valor = 1000; count1000++; } else { valor = 20; } }
            const billete = document.createElement('div');
            billete.className = 'billete';
            billete.innerHTML = `<div class="billete-centro">${valor}</div>`;
            billete.dataset.valor = valor;
            const side = Math.floor(Math.random() * 2); 
            const posY = Math.random() * (zona.clientHeight - 80);
            let startX, speedX;
            const speedBase = 3 + Math.random() * 3; 
            if (side === 0) { startX = -150; speedX = speedBase; } else { startX = zona.clientWidth + 50; speedX = -speedBase; }
            billete.style.left = startX + 'px'; billete.style.top = posY + 'px'; billete.dataset.vx = speedX;
            billete.onclick = (e) => { 
                e.stopPropagation(); 
                actualizarApuntado(e.clientX, e.clientY); 
                if (intentarDisparar()) {
                    dispararBillete(billete); 
                }
            };
            billete.ontouchstart = (e) => { 
                e.preventDefault(); e.stopPropagation(); 
                if(e.touches[0]) actualizarApuntado(e.touches[0].clientX, e.touches[0].clientY); 
                if (intentarDisparar()) {
                    dispararBillete(billete); 
                }
            };
            zona.appendChild(billete);
        }

        function moverBilletes() {
            const billetes = document.querySelectorAll('.billete:not(.impactado)');
            billetes.forEach(b => {
                let x = parseFloat(b.style.left);
                let vx = parseFloat(b.dataset.vx);
                x += vx;
                b.style.left = x + 'px';
                if (x < -200 || x > zona.clientWidth + 200) { b.remove(); }
            });
        }

        function dispararBillete(billete) {
            if (billete.classList.contains('impactado')) return;
            animarDisparo(0,0);
            
            let valor = parseInt(billete.dataset.valor);
            puntos += valor;
            document.getElementById('puntos').innerText = puntos;
            billete.classList.add('impactado');
            billete.innerHTML = `💸 +${valor}$`;
            billete.style.border = 'none'; billete.style.background = 'transparent'; billete.style.fontSize = '2rem'; billete.style.color = '#fff'; billete.style.textShadow = '2px 2px 4px #000'; billete.style.zIndex = 200;
            billete.animate([{ transform: 'scale(1) translateY(0)', opacity: 1 }, { transform: 'scale(1.5) translateY(-80px)', opacity: 0 }], { duration: 500, fill: 'forwards' });
            setTimeout(() => { if(billete.parentNode) billete.remove(); }, 500);
        }

        function terminarBonus() {
            limpiarIntervalos();
            document.getElementById('feedback').innerText = "FIN BONUS";
            document.getElementById('feedback').style.color = "white";
            setTimeout(() => {
                document.getElementById('feedback').innerText = "";
                if(modoJuego !== 'caballo_loco') { document.getElementById('timer-container').style.display = 'none'; }
                generarNivel();
            }, 1500);
        }

        function iniciarMovimiento(elementos) {
            const margenInferior = zona.clientHeight * 0.15;
            let objetos = elementos.map(el => {
                el.style.position = 'absolute';
                let maxX = zona.clientWidth - el.clientWidth;
                let maxY = zona.clientHeight - el.clientHeight - margenInferior;
                let x = Math.random() * maxX; let y = Math.random() * Math.max(0, maxY);
                let vx = (Math.random() - 0.5) * 4; let vy = (Math.random() - 0.5) * 4; 
                if(Math.abs(vx) < 1) vx = 1.5; if(Math.abs(vy) < 1) vy = 1.5;
                return { el, x, y, vx, vy };
            });
            intervaloMovimiento = setInterval(() => {
                let maxX = zona.clientWidth; let maxY = zona.clientHeight - margenInferior;
                objetos.forEach(obj => {
                    if (!obj.el.dataset.impactado) { 
                        obj.x += obj.vx; obj.y += obj.vy;
                        let w = obj.el.clientWidth; let h = obj.el.clientHeight;
                        if (obj.x <= 0 || obj.x + w >= maxX) { obj.vx *= -1; obj.x = Math.max(0, Math.min(obj.x, maxX - w)); }
                        if (obj.y <= 0 || obj.y + h >= maxY) { obj.vy *= -1; obj.y = Math.max(0, Math.min(obj.y, maxY - h)); }
                        obj.el.style.left = obj.x + 'px'; obj.el.style.top = obj.y + 'px';
                    }
                });
            }, 16); 
        }

        function disparar(elemento, esCorrecta) {
            if(enBonus) return; 
            clearInterval(intervaloTimer);

            if (esCorrecta) {
                puntos += 10;
                nivelesCompletados++; 
                elemento.dataset.impactado = "true";
                const direccion = Math.random() < 0.5 ? -1 : 1; 
                const anchoPantalla = window.innerWidth;
                elemento.style.transition = 'none';
                elemento.animate([{ transform: 'translate(0, 0) scale(1) rotate(0deg)' }, { transform: `translate(${direccion * anchoPantalla * 2}px, -150px) scale(0) rotate(${direccion * 1000}deg)`}], { duration: 500, easing: 'ease-in', fill: 'forwards' });

                document.getElementById('feedback').innerText = "¡YEE-HAW! 🤠";
                document.getElementById('feedback').style.color = "#ffcc00"; 
                if(modoJuego !== 'vaquero') clearInterval(intervaloMovimiento);

                setTimeout(() => {
                    document.getElementById('feedback').innerText = "";
                    if (nivelesCompletados > 0 && nivelesCompletados % 10 === 0) {
                        iniciarFaseBonus();
                    } else {
                        generarNivel();
                    }
                }, 500); 
            } else {
                puntos -= 5;
                document.getElementById('feedback').innerText = "¡NOPE! 🌵";
                document.getElementById('feedback').style.color = "red";
                
                setTimeout(() => {
                    if(document.getElementById('feedback').innerText.includes("NOPE")) {
                        document.getElementById('feedback').innerText = "";
                    }
                }, 1000);

                elemento.animate([{ transform: 'rotate(0deg)' }, { transform: 'rotate(-10deg)' }, { transform: 'rotate(10deg)' }, { transform: 'rotate(0deg)' }], { duration: 200 });
                if(modoJuego === 'caballo_loco' && tiempoRestante > 0) {
                     intervaloTimer = setInterval(() => {
                        tiempoRestante--;
                        document.getElementById('timer').innerText = tiempoRestante;
                        if(tiempoRestante <= 0) {
                            clearInterval(intervaloTimer);
                            puntos -= 10;
                            document.getElementById('puntos').innerText = puntos;
                            document.getElementById('feedback').innerText = "¡TIEMPO! ⏳";
                            document.getElementById('feedback').style.color = "orange";
                            if(modoJuego !== 'vaquero') clearInterval(intervaloMovimiento);
                            setTimeout(() => { document.getElementById('feedback').innerText = ""; generarNivel(); }, 1000);
                        }
                    }, 1000);
                }
            }
            document.getElementById('puntos').innerText = puntos;
        }
    </script>
</body>
</html>
