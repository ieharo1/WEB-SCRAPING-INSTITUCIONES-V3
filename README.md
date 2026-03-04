# 🌐 Web Scraping Instituciones v0.0.3 - Selenium Version

Sistema de web scraping con Selenium para sitios web dinámicos y JavaScript-heavy. Desarrollado por **Isaac Esteban Haro Torres**.

---

## 📝 Descripción

Implementación avanzada usando Selenium WebDriver para extraer datos de sitios web que requieren JavaScript para renderizar contenido. Ideal para sitios dinámicos.

### ¿Qué hace este proyecto?

- **JavaScript rendering**: Ejecuta JS para cargar contenido
- **Automación de navegador**: Control completo del browser
- **Screenshot capture**: Captura pantallas de páginas
- **Interactive scraping**: Manejo de modals, infinite scroll
- **WAIT strategies**: Espera inteligente de elementos
- **Cross-browser**: Chrome, Firefox, Edge support

---

## ✨ Características Principales

| Característica | Descripción |
|----------------|-------------|
| 🌐 **JS Rendering** | Ejecuta todo el JavaScript |
| ⏳ **Smart Waits** | Espera explícita de elementos |
| 📸 **Screenshots** | Captura de pantalla integrada |
| 🔄 **Infinite Scroll** | Maneja scroll infinito |
| 🤖 **Bot detection** | Evade detección básica |
| 🎯 **Shadow DOM** | Soporte para Shadow DOM |

---

## 🛠️ Stack Tecnológico

- **Framework**: Selenium WebDriver
- **Python**: 3.10+
- **Browsers**: Chrome, Firefox, Edge
- **WebDriver Manager**: Auto-instalación de drivers
- **By**: Localización de elementos
- **Expected Conditions**: Waits

---

## 🚀 Instalación y Uso

### Instalación

```bash
pip install selenium webdriver-manager
```

### ChromeDriver automático

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# Configuración automática
driver = webdriver.Chrome(
    service=Service(ChromeDriverManager().install())
)
```

### Ejemplo básico

```python
from selenium_scraper import ScraperSelenium

scraper = ScraperSelenium(browser='chrome', headless=False)

# Navegar y esperar
scraper.navegar('https://www.ejemplo.edu.ec')
scraper.esperar_elemento('div.contactos')

# Extraer datos
datos = scraper.extraer({
    'email': 'a.email::text',
    'telefono': 'span.phone::text',
    'direccion': 'div.address::text'
})

scraper.cerrar()
```

---

## 📁 Estructura del Proyecto

### Scraping con waits

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.chrome.options import Options

# Configuración avanzada
options = Options()
options.add_argument('--headless')  # Sin interfaz gráfica
options.add_argument('--disable-gpu')
options.add_argument('--no-sandbox')
options.add_argument('--disable-dev-shm-usage')
options.add_argument('--window-size=1920,1080')
options.add_argument('--disable-blink-features=AutomationControlled')

# User-Agent personalizado
options.add_argument('user-agent=Mozilla/5.0...')

driver = webdriver.Chrome(options=options)

# Navegar
driver.get('https://sitio.edu.ec')

# Espera explícita - esperar que cargue
wait = WebDriverWait(driver, 10)
elemento = wait.until(
    EC.presence_of_element_located((By.CSS_SELECTOR, '.contact-email'))
)
email = elemento.text

# Múltiples elementos
contactos = driver.find_elements(By.CSS_SELECTOR, '.contact-item')
for contacto in contactos:
    print(contacto.text)
```

---

## 🔄 Infinite Scroll

```python
def scroll_infinitas(driver, limite=50):
    """Maneja páginas con scroll infinito"""
    driver.execute_script("window.scrollTo(0, 0);")
    
    ultimo_height = driver.execute_script("return document.body.scrollHeight")
    contador = 0
    
    while contador < limite:
        driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
        time.sleep(2)  # Esperar carga
        
        nuevo_height = driver.execute_script("return document.body.scrollHeight")
        if nuevo_height == ultimo_height:
            break
            
        ultimo_height = nuevo_height
        contador += 1
```

---

## 🎯 Estrategias de Espera

```python
from selenium.webdriver.support import expected_conditions as EC

# Esperar elemento visible
wait.until(EC.visibility_of_element_located((By.ID, 'content')))

# Esperar clickable
wait.until(EC.element_to_be_clickable((By.XPATH, '//button[@id="submit"]')))

# Esperar texto presente
wait.until(EC.text_to_be_present_in_element((By.CLASS_NAME, 'title'), 'Bienvenido'))

# Esperar alerta
wait.until(EC.alert_is_present())

# Esperar número de ventanas
wait.until(lambda d: len(d.window_handles) == 2)
```

---

## 📸 Screenshots y Logs

```python
# Captura de pantalla
scraper.screenshot('pagina_completa.png')

# Captura de elemento específico
elemento = driver.find_element(By.CLASS_NAME, 'contact-section')
elemento.screenshot('contactos.png')

# Obtener código fuente después de JS
html_renderizado = driver.page_source
```

---

## 🤖 Evitar Detección

```python
from selenium.webdriver import ChromeOptions

options = ChromeOptions()
options.add_experimental_option("excludeSwitches", ["enable-automation"])
options.add_experimental_option('useAutomationExtension', False)

# Ocultar webdriver
driver.execute_cdp_cmd('Page.addScriptToEvaluateOnNewDocument', {
    'source': '''
        Object.defineProperty(navigator, 'webdriver', {
            get: () => undefined
        })
    '''
})
```

---

## 📊 Comparación con Otros Métodos

| Característica | Selenium | BeautifulSoup | Scrapy |
|----------------|----------|---------------|--------|
| JavaScript | ✅ | ❌ | ❌ |
| Velocidad | ❌ Lento | ✅ Rápido | ✅ Rápido |
| Facilidad | ✅ Alta | ✅ Alta | ⚠️ Media |
| Recursos | ❌ Alto | ✅ Bajo | ✅ Bajo |
| Detección | ⚠️ Media | ✅ Baja | ✅ Baja |

---

## ⚠️ Consideraciones

- **Lento**: No es el más rápido, pero muy versátil
- **Recursos**: Consume más memoria que otros métodos
- **Detección**: Algunos sitios detectan Selenium
- **Mantenimiento**: Necesita actualizar drivers

---

## 💡 Cuándo Usar Selenium

1. Sitios con JavaScript pesado
2. Infinite scroll
3. SPAs (Single Page Applications)
4. Formularios dinámicos
5. Autenticación requerida
6. Interacción con elementos

---

## 🤝 Contribuciones

¿Agregaste nuevos scrapers?
¡Abre un Pull Request!

---

## 👨‍💻 Desarrollado por Isaac Esteban Haro Torres

**Ingeniero en Sistemas · Full Stack · Automatización · Data**

- 📧 Email: zackharo1@gmail.com
- 📱 WhatsApp: 098805517
- 💻 GitHub: https://github.com/ieharo1
- 🌐 Portafolio: https://ieharo1.github.io/portafolio-isaac.haro/

---

© 2026 Isaac Esteban Haro Torres - Todos los derechos reservados.
