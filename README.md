[
    {
        "id": "4f42e827.9ff1d8",
        "type": "tab",
        "label": "Cohete ESP32",
        "disabled": false,
        "info": ""
    },
    {
        "id": "b20d47f2.0370e8",
        "type": "json",
        "z": "4f42e827.9ff1d8",
        "d": true,
        "name": "Convertir a JSON",
        "property": "payload",
        "action": "",
        "pretty": false,
        "x": 170,
        "y": 40,
        "wires": [
            [
                "bbae888060db0348",
                "cb28f3ab0f65e285"
            ]
        ]
    },
    {
        "id": "ed4b0248.85a2b",
        "type": "function",
        "z": "4f42e827.9ff1d8",
        "name": "Extraer datos grafica",
        "func": "// EXTRAER DATOS (Configurado con 5 salidas)\n\n// Convertimos a número (aunque vengan como string de toFixed) para asegurar compatibilidad\nlet alt = parseFloat(msg.payload.alt);\nlet vel = parseFloat(msg.payload.vel);\nlet pres = parseFloat(msg.payload.pres);\nlet temp = parseFloat(msg.payload.temp);\nlet lat = parseFloat(msg.payload.lat); \nlet lon = parseFloat(msg.payload.lon); \n\n// 1. Altitud (Salida 1)\nlet msg_alt = { payload: alt };\n\n// 2. Velocidad (Salida 2)\nlet msg_vel = { payload: vel };\n\n// 3. Batería (Salida 3) <-- SOLUCIONA EL MEDIDOR DE BATERÍA\nlet msg_pres = { payload: pres };\n\n// 4. Temperatura (Salida 4) <-- SOLUCIONA EL MEDIDOR DE TEMPERATURA\nlet msg_temp = { payload: temp };\n\n// 5. Trayectoria de Vuelo / World Map (Salida 5)\n// Colocamos las coordenadas en el objeto raíz del mensaje para que la\n// función 'funcion world map' las pueda usar.\nlet msg_map = { \n    payload: msg.payload, // Mantenemos el JSON original en payload (opcional)\n    lat: lat, // <-- Solución para el mapa (msg.lat y msg.lon)\n    lon: lon\n};\n\n// Devuelve los 5 mensajes a sus respectivas salidas\nreturn [msg_alt, msg_vel, msg_pres, msg_temp, msg_map];",
        "outputs": 5,
        "timeout": "",
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 580,
        "y": 80,
        "wires": [
            [
                "8e67301ea040f8d0",
                "b96c15df.b958a8"
            ],
            [
                "f911b8ea.19a668"
            ],
            [
                "3512198eb341f852"
            ],
            [
                "a3394d3fcb7dcc17"
            ],
            [
                "26d9f53c78564b75"
            ]
        ]
    },
    {
        "id": "b96c15df.b958a8",
        "type": "ui_gauge",
        "z": "4f42e827.9ff1d8",
        "name": "Altitud",
        "group": "97e73ce47ec5e405",
        "order": 1,
        "width": "0",
        "height": "0",
        "gtype": "gage",
        "title": "Altitud (m)",
        "label": "m",
        "format": "{{msg.alt}}",
        "min": 0,
        "max": "4000",
        "colors": [
            "#00b4ff",
            "#e6e600",
            "#ca3838"
        ],
        "seg1": "",
        "seg2": "",
        "diff": false,
        "className": "",
        "x": 890,
        "y": 20,
        "wires": []
    },
    {
        "id": "f911b8ea.19a668",
        "type": "ui_gauge",
        "z": "4f42e827.9ff1d8",
        "name": "Velocidad",
        "group": "97e73ce47ec5e405",
        "order": 2,
        "width": 0,
        "height": 0,
        "gtype": "gage",
        "title": "Velocidad (m/s)",
        "label": "m/s",
        "format": "{{msg.vel}}",
        "min": 0,
        "max": "400",
        "colors": [
            "#00b4ff",
            "#e6e600",
            "#ca3838"
        ],
        "seg1": "",
        "seg2": "",
        "diff": false,
        "className": "",
        "x": 900,
        "y": 100,
        "wires": []
    },
    {
        "id": "5b967b39.3cf5e4",
        "type": "serial in",
        "z": "4f42e827.9ff1d8",
        "d": true,
        "name": "Entrada Serial Cohete",
        "serial": "port1",
        "x": 100,
        "y": 100,
        "wires": [
            [
                "b20d47f2.0370e8"
            ]
        ]
    },
    {
        "id": "bbae888060db0348",
        "type": "file",
        "z": "4f42e827.9ff1d8",
        "name": "Avionica_Datos",
        "filename": "C:\\Users\\arath\\Desktop\\Coheteria\\Avionica_Datos.txt",
        "filenameType": "str",
        "appendNewline": true,
        "createDir": false,
        "overwriteFile": "false",
        "encoding": "none",
        "x": 380,
        "y": 340,
        "wires": [
            []
        ]
    },
    {
        "id": "62d3052176bd43c5",
        "type": "function",
        "z": "4f42e827.9ff1d8",
        "name": "function 1",
        "func": "// SIMULACIÓN DE DATOS DINÁMICOS DEL COHETE\n// Las variables se declaran al inicio (sólo usa una vez)\nlet lat = 28.6352 + (Math.random() - 0.5) / 1000; // Simulación de cambio de posición\nlet lon = -106.0769 + (Math.random() - 0.5) / 1000;\nlet alt = 1000 + Math.random() * 500;            // Altitud entre 1000 y 1500\nlet vel = 100 + Math.random() * 50;              // Velocidad entre 100 y 150\nlet temp = 25 + Math.random() * 3;\nlet pres = 3.7 + Math.random() * 0.1;\n\n// CONSTRUYE EL OBJETO JSON (msg.payload)\n// Utilizamos las variables 'alt', 'vel', etc., para que sean fáciles de leer\n// en el nodo Extraer datos (msg.payload.alt)\n\n// Código en function 1 (Generación de JSON)\nmsg.payload = {\n    \"alt\": alt.toFixed(2),\n    \"vel\": vel.toFixed(2),\n    \"temp\": temp.toFixed(1),  // <<-- CLAVE: debe ser 'temp'\n    \"pres\": pres.toFixed(2),  // <<-- CLAVE: debe ser 'batt'\n    \"lat\": lat.toFixed(6),\n    \"lon\": lon.toFixed(6)\n};\nreturn msg;\n",
        "outputs": 1,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 260,
        "y": 200,
        "wires": [
            [
                "bbae888060db0348",
                "ed4b0248.85a2b",
                "769e8759cf0bd202"
            ]
        ]
    },
    {
        "id": "bfcafeeddc6ff9de",
        "type": "inject",
        "z": "4f42e827.9ff1d8",
        "name": "",
        "props": [],
        "repeat": "1",
        "crontab": "",
        "once": false,
        "onceDelay": "1",
        "topic": "",
        "x": 50,
        "y": 360,
        "wires": [
            [
                "62d3052176bd43c5"
            ]
        ]
    },
    {
        "id": "802555feb8f93130",
        "type": "worldmap",
        "z": "4f42e827.9ff1d8",
        "name": "",
        "lat": "",
        "lon": "",
        "zoom": "",
        "layer": "OSMG",
        "cluster": "0",
        "maxage": "",
        "usermenu": "show",
        "layers": "show",
        "panit": "false",
        "panlock": "false",
        "zoomlock": "false",
        "hiderightclick": "false",
        "coords": "none",
        "showgrid": "false",
        "showruler": "false",
        "allowFileDrop": "false",
        "path": "/worldmap",
        "overlist": "DR,CO,RA,DN",
        "maplist": "OSMG,OSMC,EsriC,EsriS,UKOS",
        "mapname": "",
        "mapurl": "",
        "mapopt": "",
        "mapwms": false,
        "x": 900,
        "y": 360,
        "wires": []
    },
    {
        "id": "26d9f53c78564b75",
        "type": "function",
        "z": "4f42e827.9ff1d8",
        "name": "function world map",
        "func": "// Revisamos si viene del celular (OwnTracks)\nif (msg.topic && msg.topic.includes(\"owntracks\")) {\n    // Mensaje proveniente del GPS del celular\n    msg.payload = {\n        name: \"Celular Arath\",\n        lat: msg.payload.lat,\n        lon: msg.payload.lon,\n        icon: \"fa-mobile\",\n        layer: \"Recuperación\"\n    };\n}\n\n// Revisamos si viene del cohete (ESP32 o telemetría)\nelse if (msg.payload && msg.payload.lat !== undefined && msg.payload.lon !== undefined) {\n    msg.payload = {\n        name: \"Cohete\",\n        lat: msg.payload.lat,\n        lon: msg.payload.lon,\n        icon: \"fa-rocket\",\n        layer: \"Cohete\"\n    };\n}\n\n// Si no cumple ninguna condición, no enviar nada\nelse {\n    return null;\n}\n\nreturn msg;\n",
        "outputs": 1,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 590,
        "y": 380,
        "wires": [
            [
                "802555feb8f93130"
            ]
        ]
    },
    {
        "id": "3512198eb341f852",
        "type": "ui_gauge",
        "z": "4f42e827.9ff1d8",
        "name": "Presion",
        "group": "2cad02b60cc11284",
        "order": 2,
        "width": 0,
        "height": 0,
        "gtype": "gage",
        "title": "Presion",
        "label": "hpo",
        "format": "{{msg.batt}}",
        "min": "1200",
        "max": "700",
        "colors": [
            "#00b4ff",
            "#e6e600",
            "#ca3838"
        ],
        "seg1": "",
        "seg2": "",
        "diff": false,
        "className": "",
        "x": 900,
        "y": 180,
        "wires": []
    },
    {
        "id": "a3394d3fcb7dcc17",
        "type": "ui_gauge",
        "z": "4f42e827.9ff1d8",
        "name": "Temperatura",
        "group": "2cad02b60cc11284",
        "order": 1,
        "width": "0",
        "height": "0",
        "gtype": "gage",
        "title": "Temperatura",
        "label": "Grados",
        "format": "{{msg.temp}}",
        "min": 0,
        "max": "100",
        "colors": [
            "#00b4ff",
            "#e6e600",
            "#ca3838"
        ],
        "seg1": "",
        "seg2": "",
        "diff": false,
        "className": "",
        "x": 910,
        "y": 260,
        "wires": []
    },
    {
        "id": "8e67301ea040f8d0",
        "type": "ui_chart",
        "z": "4f42e827.9ff1d8",
        "name": "Trayectoria del Cohete",
        "group": "19b95a32.66b7a6",
        "order": 1,
        "width": 0,
        "height": 0,
        "label": "Trayectoria del Cohete",
        "chartType": "line",
        "legend": "false",
        "xformat": "HH:mm:ss",
        "interpolate": "linear",
        "nodata": "",
        "dot": false,
        "ymin": "0",
        "ymax": "3000",
        "removeOlder": "10",
        "removeOlderPoints": "1000",
        "removeOlderUnit": "60",
        "cutout": 0,
        "useOneColor": false,
        "useUTC": false,
        "colors": [
            "#002387",
            "#aec7e8",
            "#ff7f0e",
            "#2ca02c",
            "#98df8a",
            "#002387",
            "#ff9896",
            "#9467bd",
            "#c5b0d5"
        ],
        "outputs": 1,
        "useDifferentColor": false,
        "className": "",
        "x": 720,
        "y": 480,
        "wires": [
            []
        ]
    },
    {
        "id": "2e7556c9779aa5ed",
        "type": "ui_text",
        "z": "4f42e827.9ff1d8",
        "group": "f622c07c4ae3a40a",
        "order": 3,
        "width": 0,
        "height": 0,
        "name": "Presion",
        "label": "Presion",
        "format": "{{msg.payload}} hpa",
        "layout": "row-left",
        "className": "",
        "style": false,
        "font": "",
        "fontSize": 16,
        "color": "#000000",
        "x": 900,
        "y": 220,
        "wires": []
    },
    {
        "id": "a4cd947fe6d04e4b",
        "type": "ui_text",
        "z": "4f42e827.9ff1d8",
        "group": "f622c07c4ae3a40a",
        "order": 4,
        "width": 0,
        "height": 0,
        "name": "Temperatura",
        "label": "Temperatura",
        "format": "{{msg.payload}} m°C",
        "layout": "row-left",
        "className": "",
        "style": false,
        "font": "",
        "fontSize": 16,
        "color": "#000000",
        "x": 890,
        "y": 300,
        "wires": []
    },
    {
        "id": "78b491dfc4f05be6",
        "type": "ui_text",
        "z": "4f42e827.9ff1d8",
        "group": "f622c07c4ae3a40a",
        "order": 2,
        "width": 0,
        "height": 0,
        "name": "Velocidad",
        "label": "Velocidad",
        "format": "{{msg.payload}} m/s",
        "layout": "row-left",
        "className": "",
        "style": false,
        "font": "",
        "fontSize": 16,
        "color": "#000000",
        "x": 900,
        "y": 140,
        "wires": []
    },
    {
        "id": "8d5436cc600d889e",
        "type": "ui_text",
        "z": "4f42e827.9ff1d8",
        "group": "f622c07c4ae3a40a",
        "order": 1,
        "width": 0,
        "height": 0,
        "name": "Altitud",
        "label": "Altitud",
        "format": "{{msg.payload}} m",
        "layout": "row-left",
        "className": "",
        "style": false,
        "font": "",
        "fontSize": 16,
        "color": "#000000",
        "x": 890,
        "y": 60,
        "wires": []
    },
    {
        "id": "769e8759cf0bd202",
        "type": "function",
        "z": "4f42e827.9ff1d8",
        "name": "Extraer datos text",
        "func": "// ✅ msg.payload ya es un JSON válido gracias al nodo Convertir a JSON\n\n// Extraemos y convertimos a número\nlet alt = parseFloat(msg.payload.alt);\nlet vel = parseFloat(msg.payload.vel);\nlet pres = parseFloat(msg.payload.pres);\nlet temp = parseFloat(msg.payload.temp);\n\n// 1. Altitud\nlet msg_alt = { payload: alt };\n\n// 2. Velocidad\nlet msg_vel = { payload: vel };\n\n// 3. Presión\nlet msg_pres = { payload: pres };\n\n//  4. Temperatura\nlet msg_temp = { payload: temp };\n\n\n// Regresar las 5 salidas\nreturn [msg_alt, msg_vel, msg_pres, msg_temp];\n",
        "outputs": 4,
        "timeout": "",
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 530,
        "y": 220,
        "wires": [
            [
                "8d5436cc600d889e"
            ],
            [
                "78b491dfc4f05be6"
            ],
            [
                "2e7556c9779aa5ed"
            ],
            [
                "a4cd947fe6d04e4b"
            ]
        ]
    },
    {
        "id": "60074198999411e8",
        "type": "mqtt in",
        "z": "4f42e827.9ff1d8",
        "name": "GPS Recuperacion",
        "topic": "owntracks/+/+",
        "qos": "2",
        "datatype": "auto-detect",
        "broker": "a0c027e7de5f647a",
        "nl": false,
        "rap": true,
        "rh": 0,
        "inputs": 0,
        "x": 370,
        "y": 520,
        "wires": [
            [
                "5cc1f1a4e2329b79",
                "26d9f53c78564b75"
            ]
        ]
    },
    {
        "id": "5cc1f1a4e2329b79",
        "type": "debug",
        "z": "4f42e827.9ff1d8",
        "name": "debug 1",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "false",
        "statusVal": "",
        "statusType": "auto",
        "x": 680,
        "y": 540,
        "wires": []
    },
    {
        "id": "cb28f3ab0f65e285",
        "type": "function",
        "z": "4f42e827.9ff1d8",
        "d": true,
        "name": "Convertir Datos",
        "func": "// Ejemplo de entrada: \"ALT:1.87;VZ:0.62;P:836.89;AX:0.54;AY:0.64;LAT:N/A;LON:N/A\"\n\nlet raw = msg.payload;\n\n// Dividir por ';'\nlet parts = raw.split(\";\");\n\nlet data = {};\n\nparts.forEach(p => {\n    if (p.includes(\":\")) {\n        let [key, value] = p.split(\":\");\n\n        // Convertir a número si aplica\n        if (value === \"N/A\") {\n            data[key.toLowerCase()] = null;  // lat/lon N/A\n        } else {\n            let num = Number(value);\n            data[key.toLowerCase()] = isNaN(num) ? value : num;\n        }\n    }\n});\n\nmsg.payload = data;\nreturn msg;\n",
        "outputs": 1,
        "timeout": "",
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 380,
        "y": 120,
        "wires": [
            [
                "ed4b0248.85a2b",
                "769e8759cf0bd202"
            ]
        ]
    },
    {
        "id": "ddd256cabe9c5b54",
        "type": "ui_spacer",
        "z": "4f42e827.9ff1d8",
        "name": "spacer",
        "group": "19b95a32.66b7a6",
        "order": 2,
        "width": "1",
        "height": "1"
    },
    {
        "id": "97e73ce47ec5e405",
        "type": "ui_group",
        "name": "Graficas",
        "tab": "b4d0e97f.84c7e8",
        "order": 3,
        "disp": true,
        "width": "5",
        "collapse": false,
        "className": ""
    },
    {
        "id": "port1",
        "type": "serial-port",
        "name": "",
        "serialport": "COM8",
        "serialbaud": "9600",
        "databits": "8",
        "parity": "none",
        "stopbits": "1",
        "waitfor": "",
        "newline": "\\n",
        "bin": "false",
        "out": "char",
        "addchar": "false",
        "responsetimeout": ""
    },
    {
        "id": "2cad02b60cc11284",
        "type": "ui_group",
        "name": "",
        "tab": "b4d0e97f.84c7e8",
        "order": 4,
        "disp": true,
        "width": "5",
        "collapse": false,
        "className": ""
    },
    {
        "id": "19b95a32.66b7a6",
        "type": "ui_group",
        "name": "Trayectoria ",
        "tab": "b4d0e97f.84c7e8",
        "order": 1,
        "disp": true,
        "width": "11",
        "collapse": false,
        "className": ""
    },
    {
        "id": "f622c07c4ae3a40a",
        "type": "ui_group",
        "name": "Datos",
        "tab": "b4d0e97f.84c7e8",
        "order": 2,
        "disp": true,
        "width": "4",
        "collapse": false,
        "className": ""
    },
    {
        "id": "a0c027e7de5f647a",
        "type": "mqtt-broker",
        "name": "",
        "broker": "192.168.159.159",
        "port": 1883,
        "clientid": "nodered_pc",
        "autoConnect": true,
        "usetls": false,
        "protocolVersion": 4,
        "keepalive": 60,
        "cleansession": true,
        "autoUnsubscribe": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthRetain": "false",
        "birthPayload": "",
        "birthMsg": {},
        "closeTopic": "",
        "closeQos": "0",
        "closeRetain": "false",
        "closePayload": "",
        "closeMsg": {},
        "willTopic": "",
        "willQos": "0",
        "willRetain": "false",
        "willPayload": "",
        "willMsg": {},
        "userProps": "",
        "sessionExpiry": ""
    },
    {
        "id": "b4d0e97f.84c7e8",
        "type": "ui_tab",
        "name": "Mohinora By CUUheteria Telemetria",
        "icon": "dashboard",
        "disabled": false,
        "hidden": false
    },
    {
        "id": "6a83e57eb16acb86",
        "type": "global-config",
        "env": [],
        "modules": {
            "node-red-dashboard": "3.6.6",
            "node-red-node-serialport": "2.0.3",
            "node-red-contrib-web-worldmap": "5.5.2"
        }
    }
]
