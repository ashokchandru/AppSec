# API Explorer

<div id="swagger-ui"></div>

<link rel="stylesheet" href="https://unpkg.com/swagger-ui-dist/swagger-ui.css" />

<script src="https://unpkg.com/swagger-ui-dist/swagger-ui-bundle.js"></script>
<script src="https://unpkg.com/swagger-ui-dist/swagger-ui-standalone-preset.js"></script>

<script>
window.onload = function () {
  SwaggerUIBundle({
    urls: [
      { url: "/openapi/appsec_plus_openapi.json", name: "JSON Spec" },
      { url: "/openapi/appsec_plus_openapi.yaml", name: "YAML Spec" }
    ],
    dom_id: '#swagger-ui',
    deepLinking: true,
    presets: [
      SwaggerUIBundle.presets.apis,
      SwaggerUIStandalonePreset
    ],
    layout: "StandaloneLayout"
  });
};
</script>
