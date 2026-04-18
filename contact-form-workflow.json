{
  "name": "Contact Form AI Classification",
  "nodes": [
    {
      "parameters": {
        "path": "contact-form",
        "httpMethod": "POST"
      },
      "id": "webhook_node",
      "name": "Webhook - Contact Form",
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 1,
      "position": [250, 300]
    },
    {
      "parameters": {
        "resource": "message",
        "action": "create",
        "model": "claude-3-5-sonnet-20241022",
        "messages": {
          "messageValues": [
            {
              "role": "user",
              "content": "=Analiza el siguiente mensaje de contacto y clasifica si es URGENTE o NO URGENTE. Responde en JSON con formato: {\"urgency\": \"URGENTE|NO URGENTE\", \"reason\": \"breve razón\", \"summary\": \"resumen del mensaje\"}\n\nNombre: {{ $json.body.nombre }}\nEmail: {{ $json.body.email }}\nMensaje: {{ $json.body.mensaje }}"
            }
          ]
        }
      },
      "id": "claude_node",
      "name": "Claude AI - Classification",
      "type": "n8n-nodes-base.openAiGpt",
      "typeVersion": 3,
      "position": [500, 300],
      "credentials": {
        "openAiApi": "claude_api_credential"
      }
    },
    {
      "parameters": {
        "authentication": "oAuth2",
        "resource": "message",
        "operation": "send",
        "toAddress": "mi@email.com",
        "subject": "=Nuevo contacto - {{ $json.body.urgency }}",
        "bodyHtml": "=<h2>Nuevo mensaje de contacto</h2>\n<p><strong>Nombre:</strong> {{ $json.body.nombre }}</p>\n<p><strong>Email:</strong> {{ $json.body.email }}</p>\n<p><strong>Urgencia:</strong> <span style=\"color: {{ $json.body.urgency === 'URGENTE' ? 'red' : 'green' }}\">{{ $json.body.urgency }}</span></p>\n<p><strong>Razón:</strong> {{ $json.body.reason }}</p>\n<hr>\n<p><strong>Resumen del mensaje:</strong></p>\n<p>{{ $json.body.summary }}</p>\n<hr>\n<p><strong>Mensaje completo:</strong></p>\n<p>{{ $json.body.mensaje }}</p>"
      },
      "id": "gmail_node",
      "name": "Gmail - Send Email",
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2,
      "position": [750, 150],
      "credentials": {
        "googleApi": "gmail_credential"
      }
    },
    {
      "parameters": {
        "authentication": "oAuth2",
        "operation": "append",
        "spreadsheetId": "YOUR_SPREADSHEET_ID",
        "sheetName": "Contactos",
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "Fecha": "={{ new Date().toISOString().split('T')[0] }}",
            "Nombre": "={{ $json.body.nombre }}",
            "Email": "={{ $json.body.email }}",
            "Mensaje": "={{ $json.body.mensaje }}",
            "Urgencia": "={{ $json.body.urgency }}",
            "Razón": "={{ $json.body.reason }}",
            "Resumen": "={{ $json.body.summary }}"
          }
        }
      },
      "id": "sheets_node",
      "name": "Google Sheets - Save Data",
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4,
      "position": [750, 450],
      "credentials": {
        "googleApi": "sheets_credential"
      }
    }
  ],
  "connections": {
    "webhook_node": {
      "main": [
        [
          {
            "node": "claude_node",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "claude_node": {
      "main": [
        [
          {
            "node": "gmail_node",
            "type": "main",
            "index": 0
          },
          {
            "node": "sheets_node",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1"
  },
  "versionId": "0f4e4c3c-85a4-4b24-a44e-2c8f5d5c5e5f",
  "meta": {
    "instanceId": "67890abcdef"
  },
  "pinData": {}
}
