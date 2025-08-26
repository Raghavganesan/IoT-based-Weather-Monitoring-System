  <header>
      <h1>IoT-Based Weather Monitoring System</h1>
      <p class="muted">ESP8266-powered station capturing temperature, humidity, atmospheric pressure, air quality, and light intensity with real-time dashboards on ThingSpeak.</p>
      <div class="taglist" aria-label="keywords">
        <span class="tag">ESP8266</span>
        <span class="tag">ThingSpeak</span>
        <span class="tag">IoT</span>
        <span class="tag">Sensors</span>
        <span class="tag">Edge Processing</span>
        <span class="tag">Time-Series</span>
      </div>
    </header>
   <hr />

  <section id="overview">
      <h2>Overview</h2>
      <p>
        This project implements an end-to-end <strong>IoT weather monitoring system</strong> built on an <strong>ESP8266</strong> (NodeMCU) microcontroller. 
        It samples <em>temperature</em>, <em>humidity</em>, <em>atmospheric pressure</em>, <em>air quality</em>, and <em>light intensity</em> using 
        off-the-shelf electronic sensors and publishes the data to <strong>ThingSpeak</strong> for storage, visualization, and analytics.
      </p>
      <p>
        The design emphasizes <strong>real-time updates</strong>, <strong>reliable wireless transmission</strong>, and a clean <strong>cloud dashboard</strong> that 
        supports historical trend analysis, alerts, and data export for further processing.
      </p>
    </section>
 <section id="features">
      <h2>Main Features</h2>
      <ul>
        <li>Multi-sensor acquisition: temperature, humidity, pressure, air quality (IAQ/ppm), and illuminance (lux).</li>
        <li>Configurable sampling &amp; publishing intervals (e.g., 15–60s).</li>
        <li>Local <strong>edge filtering</strong> (median/moving average) to stabilize noisy readings.</li>
        <li>Wi-Fi connectivity with auto-reconnect and exponential backoff.</li>
        <li>Secure publishing to <strong>ThingSpeak</strong> using API keys (HTTPS capable, where supported).</li>
        <li>ThingSpeak dashboards, charts, field math, and email/SMS alerts via integrations.</li>
        <li>Modular firmware with OTA-ready structure for future updates.</li>
      </ul>
    </section>
   <section id="architecture">
      <h2>System Architecture</h2>
      <div class="grid grid-2">
        <div class="card">
          <h3>Edge (Device)</h3>
          <ul>
            <li><strong>MCU:</strong> ESP8266 (NodeMCU/WeMOS D1 Mini)</li>
            <li><strong>Sensors (example set):</strong>
              <ul>
                <li>Temperature/Humidity: <code>DHT22</code> or <code>BME280</code></li>
                <li>Pressure: <code>BMP280/BME280</code></li>
                <li>Air Quality: <code>MQ135</code> (IAQ estimate) or digital AQ sensors</li>
                <li>Light: <code>BH1750</code> (lux) or LDR + voltage divider</li>
              </ul>
            </li>
            <li>Edge filters: median &amp; moving average</li>
            <li>Push via HTTP/MQTT to ThingSpeak</li>
          </ul>
        </div>
  <div class="card">
          <h3>Cloud (ThingSpeak)</h3>
          <ul>
            <li>Channel with mapped fields (F1–F8)</li>
            <li>Time-series storage and charts</li>
            <li>MATLAB &amp; apps for analysis</li>
            <li>Alerts/integrations (email, webhooks)</li>
          </ul>
        </div>
      </div>
 <section id="hardware">
      <h2>Hardware</h2>
      <table>
        <thead>
          <tr><th>Component</th><th>Suggested Model</th><th>Purpose</th><th>Notes</th></tr>
        </thead>
        <tbody>
          <tr><td>MCU</td><td>ESP8266 NodeMCU / D1 Mini</td><td>Wi-Fi MCU &amp; I/O</td><td>5V USB power; 3.3V logic</td></tr>
          <tr><td>Temp/Humidity</td><td>DHT22 or BME280</td><td>T (°C), RH (%)</td><td>BME280 also supports pressure</td></tr>
          <tr><td>Pressure</td><td>BMP280/BME280</td><td>Pressure (hPa)</td><td>I²C interface</td></tr>
          <tr><td>Air Quality</td><td>MQ135 (basic)</td><td>IAQ/ppm (approx.)</td><td>Requires warm-up &amp; calibration</td></tr>
          <tr><td>Light</td><td>BH1750 / LDR</td><td>Illuminance (lux)</td><td>BH1750 gives direct lux via I²C</td></tr>
          <tr><td>Power</td><td>5V USB / Power Bank</td><td>Supply</td><td>Optional Li-ion + TP4056 charger</td></tr>
          <tr><td>Enclosure</td><td>Stevenson-style shield</td><td>Weather proofing</td><td>Ventilated, UV-resistant</td></tr>
        </tbody>
      </table>
    </section>
  <section id="software">
      <h2>Software Stack</h2>
      <ul>
        <li><strong>Firmware:</strong> Arduino core for ESP8266 (C/C++), OTA-ready structure.</li>
        <li><strong>Libraries (examples):</strong> <code>ESP8266WiFi</code>, <code>WiFiClient</code>, <code>ThingSpeak</code>, <code>DHT</code>, <code>Adafruit_BME280</code>, <code>BMP280</code>, <code>BH1750</code>.</li>
        <li><strong>Cloud:</strong> ThingSpeak channel, API keys, dashboards, MATLAB apps.</li>
      </ul>

<h3>Channel Field Mapping (ThingSpeak)</h3>
      <table>
        <thead><tr><th>Field</th><th>Signal</th><th>Units</th></tr></thead>
        <tbody>
          <tr><td>Field 1</td><td>Temperature</td><td>°C</td></tr>
          <tr><td>Field 2</td><td>Humidity</td><td>%RH</td></tr>
          <tr><td>Field 3</td><td>Pressure</td><td>hPa</td></tr>
          <tr><td>Field 4</td><td>Air Quality</td><td>ppm / IAQ index</td></tr>
          <tr><td>Field 5</td><td>Light</td><td>lux</td></tr>
        </tbody>
      </table>
    </section>

  <details>
        <summary>Calibration Notes</summary>
        <ul>
          <li><strong>DHT22/BME280:</strong> Allow a brief warm-up; average multiple reads.</li>
          <li><strong>BMP/BME:</strong> Sea-level pressure adjustments for altitude (optional).</li>
          <li><strong>MQ135:</strong> Needs 24–48h burn-in initially; calibrate <em>R<sub>0</sub></em> with fresh air baseline; temperature/humidity affect readings.</li>
          <li><strong>BH1750/LDR:</strong> Shield from direct LED glare; use diffuser if needed.</li>
        </ul>
      </details>
    </section>
<section id="deployment">
      <h2>Deployment &amp; Setup</h2>
      <ol>
        <li>Install Arduino IDE + ESP8266 board support.</li>
        <li>Install required libraries (ESP8266WiFi, ThingSpeak, DHT, Adafruit_BME280, BMP280, BH1750, etc.).</li>
        <li>Create a ThingSpeak channel; note the <strong>Channel ID</strong> and <strong>Write API Key</strong>.</li>
        <li>Wire sensors to ESP8266 (I²C: SDA/SCL; DHT22: single GPIO + 10k pull-up for data).</li>
        <li>Configure <code>WIFI_SSID</code>, <code>WIFI_PASS</code>, <code>CHANNEL_ID</code>, <code>WRITE_API_KEY</code> in <code>config.h</code>.</li>
        <li>Upload firmware; verify values updating on ThingSpeak.</li>
        <li>Build dashboards and (optional) alerts.</li>
      </ol>
    </section>

    

section id="future-work">
      <h2>Future Enhancements</h2>
      <ul>
        <li>Battery/solar power with deep sleep optimization.</li>
        <li>Replace MQ135 with calibrated PM sensors (e.g., PMS5003) for PM2.5/PM10.</li>
        <li>Local OLED/TFT display for on-device readouts.</li>
        <li>MQTT broker + InfluxDB + Grafana stack as an alternative to ThingSpeak.</li>
        <li>Edge anomaly detection (simple ML) for event alerts.</li>
      </ul>
    </section>

